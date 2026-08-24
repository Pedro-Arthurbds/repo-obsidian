# Plano Mestre — Sistema de Gestão para Empresa de Ar-Condicionado (v3)

Decisão confirmada: **single-tenant agora**, com metodologia de planejamento mais rigorosa (arquitetura antes das fases, backlog rastreável, checklist de segurança). Caminho de migração pra multi-tenant fica documentado como decisão arquitetural explícita — não como "nota de rodapé" — caso um segundo cliente real apareça.

---

## 1. Visão geral do produto

Sistema de gestão operacional para **uma empresa de ar-condicionado**: cadastro de clientes, catálogo de serviços, abertura e acompanhamento de chamados (ordens de serviço) em kanban, alerta de manutenção preventiva vencida, e dois papéis de usuário (admin da empresa e técnico de campo). Não é uma plataforma que hospeda múltiplas empresas — é uma ferramenta interna, mas escrita com disciplina suficiente pra não travar se um dia precisar virar isso.

---

## 2. Análise arquitetural

**Arquitetura geral:** monolito Next.js 15 (App Router) — API Routes e frontend no mesmo projeto/deploy. Não separamos backend (Express) de frontend (Vite/React) porque isso multiplicaria a superfície de configuração (CORS, dois deploys, dois `.env`) sem ganho real pro tamanho desse projeto.

**Estrutura de pastas recomendada:**

```
app/
  api/
    auth/            → login
    admin/           → rotas exclusivas de admin (funcionários)
    clients/         → CRUD de clientes
    service-types/   → CRUD de tipos de serviço
    tickets/         → CRUD + transição de status de chamados
    cron/            → jobs (manutenção vencida)
  (rotas de tela)     → login, dashboard, clientes, chamados, etc.
lib/
  prisma.ts           → client singleton
  auth.ts             → JWT (jose)
  api-auth.ts         → extração/validação de token nas rotas
  validation/         → schemas Zod por domínio
  generated/prisma/   → client gerado (gitignored)
components/ui/        → componentes reutilizáveis
prisma/
  schema.prisma
  seed.ts
```

**Autenticação:** JWT via `jose` (Edge-safe), token carrega `{ sub: usuarioId, papel: "admin" | "tecnico" }`. Sem sessão de banco — stateless, expira em algumas horas, refeito via novo login.

**Autorização:** RBAC de 2 papéis. Middleware/helper (`requireAdmin`, `requireAuthenticated`) checa o `papel` do token antes de executar a lógica da rota. Nenhuma rota confia em dado vindo do client pra decidir permissão — só o que está no token assinado.

**Tratamento de erros:** toda rota de API retorna JSON com `{ error: string, details?: ... }` e status HTTP correto (400 validação, 401 não autenticado, 403 sem permissão, 404 não encontrado, 500 erro interno não tratado). Sem stack trace vazando pro client.

**Validação:** Zod em todo body de entrada, antes de qualquer query no banco. Schemas centralizados em `lib/validation/`.

**Comunicação frontend/backend:** fetch direto pras API Routes do próprio Next.js (mesma origem, sem CORS). Token guardado em `sessionStorage` no client (mesmo padrão já usado no super admin do protótipo anterior).

---

## 3. Perfis e permissões

|Papel|Pode|Não pode|
|---|---|---|
|**admin**|Tudo do técnico + cadastrar/editar/desativar funcionários, editar dados da empresa (`Empresa` singleton), ver todos os chamados|—|
|**tecnico**|Ver clientes, ver serviços, ver chamados atribuídos a ele, atualizar status/observação dos próprios chamados|Cadastrar funcionário, editar dados da empresa, ver chamados de outros técnicos (a definir — ver regra de negócio no Sprint 3)|

A arquitetura permite adicionar um terceiro papel no futuro (ex: financeiro) sem redesenhar nada — é só um novo valor no enum `UserRole` e novas checagens de `papel` nas rotas relevantes.

---

## 4. Modelagem conceitual do banco

**Entidades:**

- `Usuario` — funcionário do sistema (admin ou técnico). Não tem `companyId` porque não existe conceito de tenant.
- `Empresa` — **singleton** (linha única). Guarda nome, CNPJ, telefone, endereço, logo. Não é um tenant, é config institucional pra aparecer em cabeçalhos/PDFs.
- `Cliente` — cliente final atendido pela empresa.
- `TipoServico` — catálogo de serviços (higienização, manutenção preventiva, etc.), com periodicidade em dias (usada pro alerta de manutenção vencida).
- `Chamado` — ordem de serviço. Relaciona `Cliente`, `TipoServico`, `Usuario` (técnico responsável, opcional).
- `NotificacaoLog` — registro de alertas de manutenção vencida gerados pelo job.

**Relacionamentos:**

- `Usuario` 1—N `Chamado` (como responsável, opcional)
- `Cliente` 1—N `Chamado`
- `TipoServico` 1—N `Chamado`
- `Empresa` — sem relacionamento, é standalone (linha de config)

**Regras de integridade:** `Chamado.clienteId` obrigatório; `Chamado.tipoServicoId` obrigatório; `Chamado.funcionarioResponsavelId` opcional (chamado pode existir sem técnico atribuído ainda). Nenhuma entidade tem `companyId` — se um dia migrar pra multi-tenant, essa é a mudança de schema principal (ver seção 18).

---

## 5. Arquitetura do backend

API Routes do Next.js organizadas por domínio (`/api/clients`, `/api/tickets`, etc.), cada uma seguindo o mesmo padrão: (1) extrair e validar token → (2) checar papel se necessário → (3) validar body com Zod → (4) query Prisma → (5) resposta JSON tipada. `lib/prisma.ts` expõe um client singleton com adapter `pg` (Postgres via Docker).

---

## 6. Arquitetura do frontend

Server Components pra leitura inicial de tela quando fizer sentido (menos JS no client), Client Components pra formulários e interações (kanban drag, formulário de chamado). Sem biblioteca de state management global — o escopo não justifica (React state + fetch já resolve).

---

## 7. Estrutura de APIs

|Endpoint|Método|Objetivo|Papel|
|---|---|---|---|
|`/api/auth/login`|POST|Login (retorna JWT)|público|
|`/api/admin/employees`|GET/POST|Listar/criar funcionários|admin|
|`/api/admin/employees/:id`|PUT/DELETE|Editar/desativar funcionário|admin|
|`/api/admin/company`|GET/PUT|Ver/editar dados institucionais|admin (GET: qualquer autenticado)|
|`/api/clients`|GET/POST|Listar/criar clientes|qualquer autenticado|
|`/api/clients/:id`|GET/PUT/DELETE|Detalhe/editar/desativar cliente|qualquer autenticado|
|`/api/service-types`|GET/POST|Listar/criar tipos de serviço|admin cria; qualquer lê|
|`/api/service-types/:id`|PUT/DELETE|Editar/desativar tipo de serviço|admin|
|`/api/tickets`|GET/POST|Listar/criar chamados|qualquer autenticado (técnico vê só os seus — regra de negócio)|
|`/api/tickets/:id/status`|PATCH|Mudar status do chamado|responsável ou admin|
|`/api/cron/maintenance-check`|POST|Job de manutenção vencida|protegido por `CRON_SECRET`|

---

## 8. Telas do sistema

**Admin:** Login · Dashboard (cards de status + clientes com manutenção vencida) · Funcionários (lista + form) · Clientes (lista + form) · Tipos de serviço (lista + form) · Chamados (kanban) · Perfil da empresa.

**Técnico:** Login · Meus chamados (kanban simplificado, mobile-first) · Detalhe do chamado (atualizar status, observação).

---

## 9. Fases detalhadas

### Sprint 0 — Setup ✅ (concluído)

**Objetivo:** projeto rodando localmente com banco conectado. **Por que agora:** tudo depende disso existir primeiro. **Tarefas:** Next.js 15 + TS + Tailwind · Prisma 7.9.1 (`prisma-client` + `@prisma/adapter-pg`) · `docker-compose.yml` com Postgres 16 · `jose`, `zod`, `bcryptjs`. **Critério de conclusão:** `npm run dev` sobe, `npx prisma migrate dev` conecta no Postgres do Docker sem erro.

---

### Sprint 1 — Modelagem + Auth + RBAC

**Objetivo:** schema definitivo, login funcionando, RBAC bloqueando rota errada. **Por que agora:** nada mais pode ser construído sem usuário autenticado e schema definido. **Tarefas:**

1. Escrever `schema.prisma`: `Usuario`, `Empresa` (singleton), `Cliente`, `TipoServico`, `Chamado`, `NotificacaoLog`.
2. `prisma migrate dev --name init`.
3. `POST /api/auth/login` — bcrypt + JWT via `jose`.
4. Helpers `requireAuthenticated` / `requireAdmin` em `lib/api-auth.ts`.
5. CRUD de funcionários (`/api/admin/employees`) — só admin.
6. Seed: cria admin inicial + linha singleton de `Empresa`.
7. Tela de login única.

**Regras de negócio:** só existe um usuário admin inicial (via seed); admin pode criar quantos técnicos quiser; usuário desativado (`ativo=false`) não consegue logar mesmo com senha certa. **Segurança:** senha nunca retorna em nenhuma resposta de API; token expira em 8h; rota admin-only retorna 403 (não 404) pra técnico — não esconder a existência da rota, só negar acesso. **Critério de conclusão:** login funciona pros dois papéis; técnico batendo em rota admin-only recebe 403; usuário desativado não loga.

---

### Sprint 2 — CRUD de Clientes e Tipos de Serviço

**Objetivo:** cadastro operacional básico. **Por que agora:** chamado (Sprint 3) depende de cliente e tipo de serviço já existirem. **Tarefas:** CRUD `/api/clients`, CRUD `/api/service-types`, validação Zod (telefone formato BR), telas de listagem com busca + formulário. **Regras de negócio:** cliente não pode ser excluído fisicamente se tiver chamado vinculado (soft delete via `active=false`). **Segurança:** validação de entrada em toda rota; nenhum campo aceito sem passar pelo schema Zod. **Critério de conclusão:** cadastro de cliente e tipo de serviço reais da empresa, validado no Prisma Studio.

---

### Sprint 3 — Chamados em Kanban

**Objetivo:** ciclo de vida completo de uma ordem de serviço. **Por que agora:** depende de Cliente, TipoServico e Usuario (técnico) já existirem. **Tarefas:** CRUD `/api/tickets`, transição de status (`aberto→andamento→concluído`, cancelado a qualquer momento), atualização automática de `ultimaManutencaoEm` do cliente ao concluir, UI kanban, tela mobile-first pro técnico. **Regra de negócio a decidir nesse sprint:** técnico vê só os chamados atribuídos a ele, ou vê todos mas só edita os seus? (recomendo: vê todos — ajuda o time a se cobrir mutuamente — mas só edita/muda status dos que é responsável, ou que ainda não têm responsável). **Segurança:** `PATCH /api/tickets/:id/status` valida que quem está mudando é o responsável ou admin — teste específico no checklist da seção 14. **Critério de conclusão:** abrir → mover no kanban → concluir → confirmar atualização automática da última manutenção do cliente.

---

### Sprint 4 — Job de manutenção vencida + Dashboard

**Objetivo:** alerta proativo de manutenção vencida. **Por que agora:** depende de chamados concluídos existindo de verdade pra ter `ultimaManutencaoEm` preenchido. **Tarefas:** job diário comparando `hoje - ultimaManutencaoEm` com periodicidade do tipo de serviço; grava em `NotificacaoLog`; some da lista ao concluir novo chamado; dashboard com cards de status + lista de manutenção vencida. **Mecanismo do job:** rota `/api/cron/maintenance-check` protegida por `CRON_SECRET`, chamada por cron do sistema/Docker ou node-cron — decisão final de "como disparar" junto da hospedagem (Sprint 5). **Critério de conclusão:** cliente com manutenção antiga aparece no dashboard; some ao concluir chamado novo.

---

### Sprint 5 — Polimento e Deploy

**Objetivo:** sistema em produção, usado de verdade. **Tarefas:** revisão de segurança ponta a ponta (checklist seção 14), responsividade mobile, variáveis de ambiente de produção, escolha de hospedagem (VPS+Docker Compose / VPS+Coolify / Postgres gerenciado+Vercel — decisão em aberto), deploy, cron em produção, backup (`pg_dump` agendado). **Critério de conclusão:** sistema rodando em produção com dado real da empresa, job de manutenção confirmado rodando.

---

## 10. Backlog (EPIC → FEATURE → TASK)

Backlog enxuto — cobre os itens de maior risco/complexidade de cada sprint, não every single task (senão o documento fica maior que o código).

```
EPIC: AUTH
  FEATURE: Login e sessão
    AUTH-001
    Rota de login com bcrypt + JWT
    Área: Backend | Prioridade: Alta | Dependência: schema Sprint 1
    Critério de aceite: login válido retorna token; login inválido retorna 401 sem revelar se o e-mail existe.

    AUTH-002
    Helpers de RBAC (requireAuthenticated/requireAdmin)
    Área: Backend | Prioridade: Alta | Dependência: AUTH-001
    Critério de aceite: rota admin-only retorna 403 pra técnico autenticado e 401 pra não autenticado.

EPIC: FUNCIONARIOS
  FEATURE: Gestão de equipe
    EMP-001
    CRUD de funcionários (admin-only)
    Área: Backend+Frontend | Prioridade: Alta | Dependência: AUTH-002
    Critério de aceite: admin cria técnico; técnico não acessa essa rota; usuário desativado não loga.

EPIC: OPERACAO
  FEATURE: Clientes e serviços
    CLI-001
    CRUD de clientes com soft delete
    Área: Backend+Frontend | Prioridade: Alta | Dependência: AUTH-002
    Critério de aceite: exclusão marca active=false, não apaga linha; chamado antigo continua referenciando o cliente.

    SVC-001
    CRUD de tipos de serviço com periodicidadeDias
    Área: Backend+Frontend | Prioridade: Alta | Dependência: AUTH-002
    Critério de aceite: tipo de serviço sem periodicidade não gera alerta de manutenção (nullable).

  FEATURE: Chamados
    TKT-001
    CRUD de chamados + transição de status
    Área: Backend+Frontend | Prioridade: Alta | Dependência: CLI-001, SVC-001
    Critério de aceite: transição inválida de status (ex: concluído→aberto direto) é rejeitada.

    TKT-002
    Autorização de edição de chamado (responsável ou admin)
    Área: Backend | Prioridade: Alta | Dependência: TKT-001
    Critério de aceite: técnico não responsável recebe 403 ao tentar mudar status de chamado alheio.

    TKT-003
    Atualização automática de ultimaManutencaoEm ao concluir
    Área: Backend | Prioridade: Média | Dependência: TKT-001
    Critério de aceite: concluir chamado atualiza o campo do cliente sem ação manual.

EPIC: ALERTAS
  FEATURE: Manutenção vencida
    MNT-001
    Job de verificação diária de manutenção vencida
    Área: Backend+DevOps | Prioridade: Média | Dependência: TKT-003
    Critério de aceite: cliente vencido aparece em NotificacaoLog e some ao concluir novo chamado.

EPIC: DEPLOY
  FEATURE: Produção
    DEP-001
    Escolha e configuração de hospedagem
    Área: DevOps | Prioridade: Alta | Dependência: todos os sprints anteriores
    Critério de aceite: app acessível publicamente, banco persistente, cron rodando.
```

---

## 11. Testes de segurança (checklist de autorização)

Adaptado do princípio de "tentar quebrar o isolamento" do doc original — aqui aplicado a RBAC de 2 papéis, não a tenant:

1. Técnico autenticado chama rota `/api/admin/*` diretamente (sem passar pela tela) → deve retornar 403.
2. Técnico tenta mudar status de chamado de outro técnico via `PATCH /api/tickets/:id/status` → 403.
3. Usuário com `ativo=false` tenta logar com senha correta → 401.
4. Token expirado é usado numa rota protegida → 401, não 500.
5. Token adulterado (assinatura inválida) é usado → 401.
6. Requisição sem header `Authorization` numa rota protegida → 401, não erro de servidor.
7. Rota de cron (`/api/cron/maintenance-check`) chamada sem `CRON_SECRET` correto → 401/403, nunca executa o job.
8. Admin tenta se autodesativar (ou desativar o único admin restante) → deve ser bloqueado (regra de negócio a implementar: sempre precisa sobrar 1 admin ativo).

---

## 12. MVP

Sprints 0 a 4 completos: login + RBAC, clientes, tipos de serviço, chamados em kanban, alerta de manutenção vencida, dashboard básico. Sprint 5 (deploy) é o que transforma o MVP em algo que a empresa usa de verdade — não é "extra", é parte do objetivo, só vem por último porque depende de tudo antes.

## 13. NÃO FAZER AGORA

WhatsApp automatizado (link `wa.me` manual resolve por enquanto) · app mobile nativo (PWA/responsivo resolve) · emissão fiscal · controle de estoque de peças · mapas/geolocalização de chamados · relatórios financeiros avançados · qualquer coisa de multi-tenant/assinatura (ver seção 18).

## 14. Roadmap

- **V1 (MVP):** Sprints 0–5 desse documento.
- **V1.1:** equipamento vinculado ao cliente (não só "última manutenção" genérica), histórico de peças usadas no chamado, assinatura digital simples da OS.
- **V2 (condicional):** se e somente se aparecer demanda real de um segundo cliente — migração pra multi-tenant (ver seção 18 pra saber exatamente o que muda).

---

## 15. Riscos técnicos

- **Migração de schema em produção** conforme o app evolui — mitigar com `prisma migrate deploy` disciplinado, nunca editar schema direto em produção.
- **Cron job perdido** se a hospedagem escolhida não garantir execução confiável (ex: serverless sem cron nativo) — decisão de hospedagem no Sprint 5 precisa considerar isso.
- **Único admin ativo desativado por engano** trava o sistema (ninguém mais cria usuário) — regra de negócio no Sprint 1 (item 8 do checklist de segurança) evita isso.

---

## 16. Decisões arquiteturais importantes

- **Single-tenant por decisão consciente, não por limitação técnica.** Documentado aqui pra não ser relido como "esqueceram de pensar em crescer".
- **Next.js App Router monolítico**, não Express+React separados — reduz superfície operacional pro tamanho do projeto.
- **Postgres via Docker**, não Neon — controle total, sem dependência de serviço gerenciado enquanto o projeto é pequeno.
- **Prisma 7.9.1** com gerador `prisma-client` (sem engine Rust) + `@prisma/adapter-pg`.

### Caminho de migração pra multi-tenant (se e quando fizer sentido)

Documentando agora pra decisão futura ser rápida, não uma re-arquitetura:

1. Adicionar `companyId` (nullable inicialmente) em `Usuario`, `Cliente`, `TipoServico`, `Chamado`.
2. Criar `Company` (dados da empresa atual vira a primeira linha) e `SuperAdmin`.
3. Popular `companyId` de todos os registros existentes com o id dessa primeira empresa (migration de dado, não só de schema).
4. Tornar `companyId` obrigatório (`NOT NULL`) depois da migração de dado confirmada.
5. Adicionar `companyId` ao payload do JWT.
6. Adicionar filtro `where: { companyId }` em **toda** query — esse é o ponto de maior risco, exige revisão rota por rota (reaproveitar o checklist da seção 11, adaptado pra isolamento de tenant).
7. Construir CRUD de empresas + assinatura (reaproveitando o que já existe do protótipo v1, se ainda estiver no repositório/histórico).

---

## 17. Primeiro passo

**Tarefa imediata:** Sprint 1, item 1 — escrever o `schema.prisma` definitivo (single-tenant) e rodar a primeira migration no Postgres do Docker. Tudo mais do Sprint 1 (auth, RBAC, seed) depende dessa modelagem estar fechada primeiro. Só depois disso faz sentido tocar em qualquer rota de API.