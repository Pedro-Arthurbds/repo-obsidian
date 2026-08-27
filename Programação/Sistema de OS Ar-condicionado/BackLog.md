backlog do [[Projeto de Gestao para empresa de ar]]

# 🧭 Como vamos trabalhar

Vou dividir nosso desenvolvimento em:

**Épico → Feature → Task → Subtask**

E cada task terá:

- Objetivo
    
- O que você precisa aprender
    
- Pré-requisitos
    
- Implementação
    
- Critério de aceite
    
- Testes
    
- Possíveis erros
    
- Status
    

Além disso, vou separar o backlog em:

> 🔴 **MUST** — obrigatório para o MVP  
> 🟡 **SHOULD** — importante, mas pode esperar  
> 🟢 **COULD** — melhoria futura

---

# 🏗️ BACKLOG COMPLETO DO PROJETO

## EPIC 0 — Fundação do projeto

O Sprint 0 já está marcado como concluído no documento, incluindo Next.js, TypeScript, Tailwind, Prisma, PostgreSQL Docker, `jose`, `zod` e `bcryptjs`.

Mesmo assim, vou manter essas tarefas no backlog histórico.

### SETUP-001 — Criar projeto Next.js

[[Checkliste]]

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-002 — Configurar TypeScript

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-003 — Configurar Tailwind

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-004 — Configurar PostgreSQL via Docker

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-005 — Configurar Prisma 7

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-006 — Configurar adapter PostgreSQL

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-007 — Configurar variáveis de ambiente

**Prioridade:** 🔴 MUST  
**Status:** ✅ Concluído

### SETUP-008 — Configurar estrutura inicial de pastas

**Prioridade:** 🔴 MUST  
**Status:** 🔶 Parcial/Conforme implementação

---

# EPIC 1 — Banco de dados

Essa será nossa **primeira etapa real**.

O próprio documento determina que o primeiro passo seja fechar o `schema.prisma` e executar a primeira migration.

## DB-001 — Modelar Usuario

Campos principais:

- id
    
- nome
    
- email
    
- senha
    
- papel
    
- ativo
    
- createdAt
    
- updatedAt
    

### Regras

- papel = `ADMIN` ou `TECNICO`
    
- senha armazenada somente como hash
    
- usuário pode ser desativado
    

---

## DB-002 — Criar enum UserRole

```text
ADMIN
TECNICO
```

---

## DB-003 — Modelar Empresa

Empresa será singleton.

Campos:

- id
    
- nome
    
- CNPJ
    
- telefone
    
- endereço
    
- logo
    
- createdAt
    
- updatedAt
    

A empresa não é um tenant; funciona como configuração institucional.

---

## DB-004 — Modelar Cliente

Campos:

- id
    
- nome
    
- telefone
    
- email
    
- endereço
    
- observações
    
- ativo
    
- ultimaManutencaoEm
    
- createdAt
    
- updatedAt
    

---

## DB-005 — Modelar TipoServico

Campos:

- id
    
- nome
    
- descrição
    
- periodicidadeDias
    
- ativo
    
- createdAt
    
- updatedAt
    

---

## DB-006 — Modelar Chamado

Campos:

- id
    
- clienteId
    
- tipoServicoId
    
- funcionarioResponsavelId
    
- status
    
- observacao
    
- createdAt
    
- updatedAt
    
- concluidoEm
    

---

## DB-007 — Criar enum TicketStatus

Precisamos definir formalmente:

```text
ABERTO
EM_ANDAMENTO
CONCLUIDO
CANCELADO
```

A regra prevista é:

```text
ABERTO → EM_ANDAMENTO → CONCLUIDO
```

e cancelamento pode ocorrer durante o fluxo.

---

## DB-008 — Modelar NotificacaoLog

Para registrar alertas de manutenção.

---

## DB-009 — Criar relacionamentos Prisma

```text
Usuario 1:N Chamado
Cliente 1:N Chamado
TipoServico 1:N Chamado
Empresa standalone
```

Esses relacionamentos estão definidos no documento.

---

## DB-010 — Criar migration inicial

```bash
npx prisma migrate dev --name init
```

---

## DB-011 — Conferir banco no Prisma Studio

---

## DB-012 — Criar seed

Seed deve:

- criar admin
    
- criar Empresa singleton
    

---

## DB-013 — Testar integridade do banco

Precisamos testar:

- FK
    
- campos obrigatórios
    
- enums
    
- valores default
    
- relacionamentos
    

---

# EPIC 2 — Infraestrutura Backend

## BACK-001 — Criar Prisma Singleton

Arquivo:

```text
lib/prisma.ts
```

Objetivo: evitar múltiplas instâncias do Prisma.

---

## BACK-002 — Criar sistema de erros da API

Padronizar:

```json
{
  "error": "Mensagem",
  "details": {}
}
```

Com:

```text
400
401
403
404
500
```

Essa padronização está explicitamente definida no documento.

---

## BACK-003 — Criar validações Zod

Estrutura:

```text
lib/
└── validation/
    ├── auth.ts
    ├── employee.ts
    ├── client.ts
    ├── service.ts
    └── ticket.ts
```

---

## BACK-004 — Criar helpers HTTP

Exemplo conceitual:

```text
unauthorized()
forbidden()
notFound()
badRequest()
serverError()
```

---

# EPIC 3 — Autenticação

## AUTH-001 — Criar endpoint Login

```http
POST /api/auth/login
```

Entrada:

```text
email
senha
```

---

## AUTH-002 — Validar usuário

Fluxo:

```text
Request
 ↓
Zod
 ↓
buscar usuário
 ↓
verificar ativo
 ↓
bcrypt.compare
 ↓
gerar JWT
 ↓
retornar token
```

---

## AUTH-003 — Gerar JWT

Payload:

```text
sub
papel
```

O documento determina JWT com `jose` e expiração de 8 horas.

---

## AUTH-004 — Criar validação JWT

Criar:

```text
lib/auth.ts
```

---

## AUTH-005 — Criar `requireAuthenticated`

---

## AUTH-006 — Criar `requireAdmin`

---

## AUTH-007 — Criar middleware/helper de autorização

---

## AUTH-008 — Bloquear usuário desativado

Mesmo com senha correta:

```text
ativo = false
→ 401
```

---

## AUTH-009 — Testes de autenticação

Testar:

- login correto
    
- senha errada
    
- email inexistente
    
- usuário desativado
    
- token expirado
    
- token adulterado
    
- ausência de token
    

---

# EPIC 4 — Frontend de autenticação

## UI-AUTH-001 — Criar página Login

---

## UI-AUTH-002 — Formulário

Campos:

```text
Email
Senha
```

---

## UI-AUTH-003 — Loading

---

## UI-AUTH-004 — Tratamento de erro

---

## UI-AUTH-005 — Salvar JWT

O documento prevê `sessionStorage` para o token.

---

## UI-AUTH-006 — Redirecionar após login

Admin:

```text
/dashboard
```

Técnico:

```text
/meus-chamados
```

---

# EPIC 5 — Funcionários

## EMP-001 — GET funcionários

```http
GET /api/admin/employees
```

Somente admin.

---

## EMP-002 — Criar funcionário

```http
POST /api/admin/employees
```

---

## EMP-003 — Hash da senha

Nunca armazenar senha pura.

---

## EMP-004 — Editar funcionário

```http
PUT /api/admin/employees/:id
```

---

## EMP-005 — Desativar funcionário

```http
DELETE /api/admin/employees/:id
```

Aqui temos uma decisão importante: apesar do endpoint ser `DELETE`, o comportamento será **soft delete/desativação**.

---

## EMP-006 — Impedir autodesativação perigosa

Regra:

> Nunca permitir que o último admin ativo seja desativado.

Essa regra aparece no checklist de segurança do documento.

---

## EMP-007 — Tela Funcionários

---

## EMP-008 — Formulário funcionário

---

## EMP-009 — Ativar/desativar funcionário

---

# EPIC 6 — Empresa

## COMPANY-001 — GET empresa

```http
GET /api/admin/company
```

---

## COMPANY-002 — PUT empresa

```http
PUT /api/admin/company
```

---

## COMPANY-003 — Tela Perfil da Empresa

Campos:

- nome
    
- CNPJ
    
- telefone
    
- endereço
    
- logo
    

---

# EPIC 7 — Clientes

## CLIENT-001 — GET clientes

---

## CLIENT-002 — Criar cliente

---

## CLIENT-003 — Buscar cliente

---

## CLIENT-004 — Editar cliente

---

## CLIENT-005 — Desativar cliente

Cliente não deve ser apagado fisicamente quando possuir chamados.

---

## CLIENT-006 — Validação telefone BR

---

## CLIENT-007 — Tela Clientes

---

## CLIENT-008 — Busca de clientes

---

## CLIENT-009 — Formulário cliente

---

## CLIENT-010 — Tela detalhe cliente

Essa tela é uma melhoria que eu recomendo colocar ainda no MVP, porque posteriormente teremos histórico de chamados.

---

# EPIC 8 — Tipos de Serviço

## SERVICE-001 — GET serviços

---

## SERVICE-002 — Criar serviço

---

## SERVICE-003 — Editar serviço

---

## SERVICE-004 — Desativar serviço

---

## SERVICE-005 — Campo periodicidade

Exemplo:

```text
Higienização
Periodicidade: 180 dias
```

---

## SERVICE-006 — Permitir periodicidade nula

Se não houver periodicidade:

```text
periodicidadeDias = null
```

Nesse caso não gera alerta.

---

## SERVICE-007 — Tela Tipos de Serviço

---

# EPIC 9 — Chamados / Ordens de Serviço

Essa é uma das partes mais importantes do sistema.

## TICKET-001 — Criar chamado

Informações:

```text
Cliente
Tipo de serviço
Técnico responsável
Observação
```

---

## TICKET-002 — Listar chamados

---

## TICKET-003 — Buscar chamado

---

## TICKET-004 — Filtrar chamados

Por:

- status
    
- técnico
    
- cliente
    
- tipo de serviço
    

---

## TICKET-005 — Editar chamado

---

## TICKET-006 — Definir técnico responsável

---

## TICKET-007 — Status do chamado

Implementar máquina de estados.

---

## TICKET-008 — Validar transições

Exemplo:

```text
ABERTO → EM_ANDAMENTO       ✅
EM_ANDAMENTO → CONCLUIDO    ✅
ABERTO → CONCLUIDO          ❌
CONCLUIDO → ABERTO          ❌
```

O documento determina que transições inválidas sejam rejeitadas.

---

## TICKET-009 — Cancelar chamado

---

## TICKET-010 — Atualizar observação

---

## TICKET-011 — Atualizar última manutenção

Quando o chamado for concluído:

```text
cliente.ultimaManutencaoEm = agora
```

Isso é uma regra central do Sprint 3.

---

## TICKET-012 — Criar Kanban

Colunas:

```text
ABERTO
EM ANDAMENTO
CONCLUÍDO
CANCELADO
```

---

## TICKET-013 — Drag and Drop

---

## TICKET-014 — Validar drag no backend

**Muito importante:**

Não confiar apenas no frontend.

Mesmo que o usuário arraste um card:

```text
Frontend → API → autorização → regra de transição → banco
```

---

## TICKET-015 — Tela detalhe chamado

---

# EPIC 10 — Permissões

Esse épico é especialmente importante para você aprender backend de verdade.

## RBAC-001

Admin acessa tudo.

## RBAC-002

Técnico não acessa:

```text
/api/admin/*
```

## RBAC-003

Técnico pode visualizar clientes.

## RBAC-004

Técnico pode visualizar serviços.

## RBAC-005

Técnico pode visualizar chamados permitidos.

## RBAC-006

Técnico só altera chamados autorizados.

O documento recomenda que o técnico veja todos os chamados, mas só edite os seus, ou chamados ainda sem responsável. Essa decisão deve ser fechada no Sprint 3.

**Minha recomendação:** vamos adotar essa regra.

---

# EPIC 11 — Dashboard

## DASH-001 — Dashboard Admin

Cards:

```text
Chamados abertos
Chamados em andamento
Chamados concluídos
Manutenções vencidas
```

---

## DASH-002 — Lista de manutenção vencida

---

## DASH-003 — Clientes com manutenção vencida

---

## DASH-004 — Atalhos operacionais

Exemplo:

```text
+ Novo Cliente
+ Novo Chamado
+ Novo Serviço
```

---

## DASH-005 — Dashboard Técnico

Mais simples:

```text
Meus chamados
Abertos
Em andamento
Concluídos
```

---

# EPIC 12 — Manutenção preventiva

## MAINT-001 — Definir regra de vencimento

Conceito:

```text
ultimaManutencaoEm
+
periodicidadeDias
=
data limite
```

---

## MAINT-002 — Criar job

```http
POST /api/cron/maintenance-check
```

---

## MAINT-003 — Proteger CRON_SECRET

---

## MAINT-004 — Buscar clientes elegíveis

---

## MAINT-005 — Identificar manutenção vencida

---

## MAINT-006 — Criar NotificacaoLog

---

## MAINT-007 — Evitar notificações duplicadas

**Essa é uma task importante que não estava detalhada no backlog original.**

Precisamos evitar:

```text
Dia 1 → alerta
Dia 2 → alerta
Dia 3 → alerta
Dia 4 → alerta
```

para a mesma ocorrência.

Precisamos definir uma estratégia de idempotência.

---

## MAINT-008 — Remover alerta após nova manutenção

Ao concluir novo chamado:

```text
manutenção antiga
      ↓
novo chamado concluído
      ↓
ultimaManutencaoEm atualizado
      ↓
alerta deixa de aparecer
```

---

# EPIC 13 — Segurança

Esse épico não será deixado para o final.

Vamos fazer segurança **durante o desenvolvimento** e uma revisão completa antes do deploy.

## SEC-001 — Senhas nunca retornam na API

## SEC-002 — Validar todo input com Zod

## SEC-003 — Validar JWT

## SEC-004 — Token expirado → 401

## SEC-005 — Token adulterado → 401

## SEC-006 — Sem token → 401

## SEC-007 — Técnico em rota admin → 403

## SEC-008 — Técnico tentando editar chamado alheio → 403

## SEC-009 — CRON sem secret → negar

## SEC-010 — Impedir último admin de ser desativado

## SEC-011 — Nunca retornar stack trace

## SEC-012 — Revisar variáveis `.env`

## SEC-013 — Revisar CORS

Como estamos usando mesma origem, não devemos precisar configurar CORS entre frontend/backend.

---

# EPIC 14 — Testes

Eu quero que você aprenda a testar o sistema, não apenas clicar na interface.

## TEST-001 — Testar login

## TEST-002 — Testar RBAC

## TEST-003 — Testar CRUD cliente

## TEST-004 — Testar CRUD serviço

## TEST-005 — Testar chamados

## TEST-006 — Testar transições

## TEST-007 — Testar permissões de técnico

## TEST-008 — Testar manutenção

## TEST-009 — Testar cron

## TEST-010 — Testar usuário desativado

## TEST-011 — Testar token expirado

## TEST-012 — Testar token inválido

---

# EPIC 15 — Frontend / UX

## UI-001 — Layout principal

## UI-002 — Sidebar

## UI-003 — Header

## UI-004 — Feedback de loading

## UI-005 — Feedback de erro

## UI-006 — Toasts

## UI-007 — Modal de confirmação

## UI-008 — Empty states

## UI-009 — Estados de erro

## UI-010 — Responsividade

---

# EPIC 16 — Mobile Técnico

O documento define explicitamente a interface do técnico como **mobile-first**.

## MOBILE-001 — Kanban mobile

## MOBILE-002 — Detalhe do chamado mobile

## MOBILE-003 — Alterar status rapidamente

## MOBILE-004 — Registrar observação

## MOBILE-005 — Navegação simplificada

---

# EPIC 17 — Deploy

O deploy faz parte do MVP, não deve ser tratado como algo opcional.

## DEPLOY-001 — Escolher infraestrutura

Opções:

```text
VPS + Docker Compose
VPS + Coolify
Postgres gerenciado + Vercel
```

---

## DEPLOY-002 — Configurar produção

---

## DEPLOY-003 — Configurar PostgreSQL

---

## DEPLOY-004 — Configurar `.env`

---

## DEPLOY-005 — Rodar migrations

Em produção:

```bash
npx prisma migrate deploy
```

---

## DEPLOY-006 — Build

---

## DEPLOY-007 — Deploy Next.js

---

## DEPLOY-008 — Configurar domínio

---

## DEPLOY-009 — HTTPS

---

## DEPLOY-010 — Configurar cron

---

## DEPLOY-011 — Backup PostgreSQL

Usando:

```bash
pg_dump
```

O documento também destaca backup agendado como requisito do Sprint 5.

---

## DEPLOY-012 — Testar restore do backup

Essa é uma tarefa que eu adicionaria.

Backup que nunca foi restaurado **não deve ser considerado confiável**.

---

# EPIC 18 — Observabilidade e manutenção

Não precisa virar um sistema gigantesco.

## OPS-001 — Logs de erro

## OPS-002 — Logs do cron

## OPS-003 — Tratamento de exceções

## OPS-004 — Health check

Exemplo:

```http
GET /api/health
```

Retorno:

```json
{
  "status": "ok"
}
```

## OPS-005 — Verificar conexão com banco

---

# 🗺️ ORDEM OFICIAL DE DESENVOLVIMENTO

Agora vem a parte mais importante.

**Não quero que você siga o backlog simplesmente de cima para baixo.**

A ordem que eu recomendo é:

### FASE 0 — Fundação

```text
SETUP
 ↓
Banco
```

### FASE 1 — Backend fundamental

```text
DB
 ↓
Prisma
 ↓
Zod
 ↓
Erros
```

### FASE 2 — Autenticação

```text
Login
 ↓
JWT
 ↓
requireAuthenticated
 ↓
requireAdmin
 ↓
RBAC
```

### FASE 3 — Funcionários

```text
Admin
 ↓
Criar técnico
 ↓
Editar
 ↓
Desativar
```

### FASE 4 — Clientes

```text
Cliente
 ↓
CRUD
 ↓
Busca
 ↓
Soft delete
```

### FASE 5 — Tipos de serviço

```text
Serviço
 ↓
CRUD
 ↓
Periodicidade
```

### FASE 6 — Chamados

```text
Cliente
+
Serviço
+
Técnico
 ↓
Chamado
 ↓
Status
 ↓
Kanban
```

### FASE 7 — Regras de negócio

```text
Permissões
 ↓
Transições
 ↓
Última manutenção
```

### FASE 8 — Manutenção preventiva

```text
Periodicidade
 ↓
Job
 ↓
NotificacaoLog
 ↓
Dashboard
```

### FASE 9 — UX

```text
Dashboard
 ↓
Mobile
 ↓
Loading
 ↓
Errors
 ↓
Empty states
```

### FASE 10 — Segurança

```text
RBAC
JWT
Inputs
Cron
Admin
API
```

### FASE 11 — Testes

```text
Backend
 ↓
Autorização
 ↓
Regras
 ↓
Integração
```

### FASE 12 — Produção

```text
Docker
 ↓
Postgres
 ↓
Deploy
 ↓
Cron
 ↓
Backup
 ↓
Restore
```

---

# 🎯 O MVP REAL

O MVP fica:

```text
┌─────────────────────────────┐
│           LOGIN             │
└──────────────┬──────────────┘
               ↓
        ┌──────────────┐
        │     RBAC     │
        └──────┬───────┘
               ↓
       ┌───────┴────────┐
       ↓                ↓
     ADMIN            TÉCNICO
       ↓                ↓
 ┌─────────────┐   ┌─────────────┐
 │ Funcionários│   │Meus chamados│
 │ Clientes    │   │             │
 │ Serviços    │   │ Atualização │
 │ Chamados    │   │ de status   │
 │ Dashboard   │   │             │
 └──────┬──────┘   └──────┬──────┘
        │                  │
        └────────┬─────────┘
                 ↓
          ┌─────────────┐
          │ PostgreSQL  │
          └──────┬──────┘
                 ↓
          Manutenção
           preventiva
                 ↓
              Cron
```

Isso está alinhado com o MVP definido no documento: Sprints 0–4, com login/RBAC, clientes, tipos de serviço, chamados em kanban, alertas de manutenção e dashboard.

---

# 🚫 O que vamos deixar fora

Não vou deixar você cair na armadilha de começar a adicionar funcionalidades aleatoriamente.

Por enquanto:

- ❌ WhatsApp automatizado
    
- ❌ aplicativo mobile nativo
    
- ❌ emissão fiscal
    
- ❌ estoque
    
- ❌ mapas
    
- ❌ geolocalização
    
- ❌ relatórios financeiros avançados
    
- ❌ multi-tenant
    
- ❌ assinatura
    
- ❌ sistema de cobrança
    

Esses itens estão explicitamente fora do MVP no roteiro.

---

# 🧠 E o mais importante: como vou atuar como seu Tech Lead

Quando você disser:

> "não estou conseguindo fazer o login"

eu **não vou imediatamente mandar 200 linhas de código**.

Vou primeiro perguntar coisas como:

```text
1. Qual era o comportamento esperado?
2. O que aconteceu?
3. Qual erro apareceu?
4. O erro acontece no frontend ou backend?
5. Qual endpoint está sendo chamado?
6. O que aparece no terminal?
7. O que aparece no Network do navegador?
```

E vamos fazer:

```text
Problema
   ↓
Hipótese
   ↓
Teste
   ↓
Resultado
   ↓
Nova hipótese
   ↓
Correção
   ↓
Entendimento
```

Isso é muito mais próximo de como você vai trabalhar profissionalmente.

---

# 🚦 NOSSO PRIMEIRO CICLO

Eu sugiro que **não comecemos pelo frontend**.

Começaremos pelo banco.

### Sprint atual

**DB-001 → DB-013**

E especificamente:

> **DB-001 — Modelar `Usuario`**

Depois:

```text
Usuario
 ↓
Empresa
 ↓
Cliente
 ↓
TipoServico
 ↓
Chamado
 ↓
NotificacaoLog
 ↓
Relacionamentos
 ↓
Migration
 ↓
Seed
```

Quando você terminar uma task, me diga algo como:

> `DB-001 concluído`

ou cole o código/erro que encontrou.

Eu vou revisar como Tech Lead, explicar **o que está certo, o que pode melhorar e por quê**, e só então avançamos.

**Não quero que você apenas termine o sistema. Quero que, quando terminar, você consiga olhar para ele e explicar a arquitetura, o banco, autenticação, autorização, API, regras de negócio, Prisma e deploy sem depender de mim.**