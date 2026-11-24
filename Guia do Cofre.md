---
tags:
  - guia
  - meta
  - documentacao
data_criacao: 2026-11-19
---

# 🧭 Guia do Cofre: Como Usar Seu Segundo Cérebro

> [!tip] Manual Rápido do Usuário
> Este documento explica o fluxo de organização, as convenções de escrita e como tirar proveito dos recursos e plugins instalados no seu Obsidian.

---

## 🗂️ 1. Arquitetura de Pastas e Fluxo de Notas

A estrutura foi desenhada para separar claramente **captura**, **ação**, **hobbies**, **estudo** e **arquivo**:

```
repo-obsidian/
├── 0. Inbox/              📥 Captura rápida (rascunhos, pendências imediatas)
├── 1. Metas e Tarefas/    🎯 Objetivos, quadros e listas de afazeres
├── 2. RPG/                🎲 Campanhas, fichas, sessões e mapas de T20
├── 3. Estudos/            📚 Base de conhecimento e aprendizado contínuo
│   ├── _Templates/        📄 Modelos padronizados (ex: Template - Nota de Estudo)
│   └── Programação/       💻 Trilhas técnicas (Algoritmos, Backend, IA)
└── 4. Arquivo/            📦 Projetos concluídos e histórico
```

### O Ciclo de Vida de uma Informação
1. **Capturar (`0. Inbox`):** Teve uma ideia rápida ou anotou algo correndo? Salve aqui sem se preocupar com estrutura (ex.: [[EJC - Pagamentos]]).
2. **Processar:** Periodicamente, mova a nota do Inbox para a pasta correspondente.
3. **Conectar:** Sempre crie links bidirecionais com notas relacionadas usando `[[Nome da Nota]]`.
4. **Arquivar (`4. Arquivo`):** Finalizou uma meta ou projeto? Mova para o arquivo para manter a visão limpa.

---

## ✍️ 2. Padrões de Escrita e Formatação

Para garantir que tudo renderize com fidelidade no Obsidian, siga estas convenções:

### 🔗 Links Bidirecionais
* **Link direto:** `[[Nome da Nota]]`
* **Link com texto alternativo:** `[[Nome da Nota|Texto personalizado]]`
* **Link para seção específica:** `[[Nome da Nota#Título da Seção]]`

### 📐 Fórmulas Matemáticas (LaTeX)
* **Em linha (inline):** Utilize cifrões simples: `$O(\log n)$` ou `$f(x) = y$`.
* **Bloco destacado (display):** Utilize cifrões duplos:

$$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$$

> [!important]
> Evite usar `\(...\)` ou `\[...\]`, pois o Obsidian prioriza a sintaxe padrão com `$`.

### 📊 Tabelas Markdown
Sempre deixe uma **linha em branco antes da tabela**, caso contrário o Obsidian pode interpretá-la como texto comum:

| Pasta | Finalidade | Exemplo de Nota |
| :--- | :--- | :--- |
| `0. Inbox` | Captura temporária | [[EJC - Pagamentos]] |
| `1. Metas e Tarefas` | Planejamento de ações | [[Quests 2026]] |
| `2. RPG` | Mesa de jogo | [[Rpg]] |
| `3. Estudos` | Aprendizado contínuo | [[Central de Estudos]] |

---

## 🎓 3. Fluxo de Estudos e Modelos

Toda a área de estudos é orientada por duas peças centrais:

1. **Dashboard:** [[Central de Estudos]]
   * Contém o índice de todas as trilhas (Backend, Algoritmos, IA) e a tabela de status de aprendizado.
2. **Modelo Padrão:** [[Template - Nota de Estudo]]
   * Estrutura inspirada na técnica Feynman (Resumo em 1 Minuto, Conceitos, Exemplos Práticos, Fórmulas e Auto-Teste).

### Como criar uma nova nota de estudo:
1. Crie uma nota na pasta desejada (ex.: em `3. Estudos/Programação/`).
2. Abra a paleta de comandos (`Ctrl + P`) e digite: `Templater: Open Insert Template modal`.
3. Escolha o [[Template - Nota de Estudo]].
4. Adicione o link da nova nota na tabela de acompanhamento da [[Central de Estudos]].

---

## 🧩 4. Plugins Instalados e Como Utilizá-los

| Plugin | O que faz | Como usar |
| :--- | :--- | :--- |
| **Obsidian Git** | Backup automático no repositório GitHub | Sincroniza em segundo plano; use `Ctrl + P` > `Obsidian Git: Commit and push` para forçar sync. |
| **Templater** | Injeta modelos inteligentes com data e metadados automáticos | `Alt + N` ou paleta de comandos para inserir modelos da pasta `_Templates`. |
| **Kanban** | Quadros visuais no estilo Trello | Crie uma nota e selecione `Kanban: Convert to Kanban` ou crie novo quadro Kanban. |
| **Excalidraw** | Desenho de diagramas, mapas conceituais e rascunhos | Clique no ícone do Excalidraw na barra lateral para criar um canvas visual. |
| **Dataview** | Cria listas e tabelas dinâmicas a partir de tags e metadados | Escreva blocos `dataview` para filtrar notas automaticamente por tags (`#estudo`, `#hub`, etc.). |
| **Calendar** | Visualização de calendário na barra lateral direita | Ótimo para associar anotações com datas e dias da semana. |

---

## 📌 5. Atalhos Rápidos Úteis (Teclado)

* `Ctrl + O`: Localizador rápido (abre qualquer nota pelo nome).
* `Ctrl + P`: Paleta de comandos (executa qualquer ação ou comando de plugin).
* `Ctrl + E`: Alternar entre modo de Edição (Live Preview) e Leitura.
* `Ctrl + G`: Visualização em Grafo (ver a teia de conexões do seu cérebro digital).
