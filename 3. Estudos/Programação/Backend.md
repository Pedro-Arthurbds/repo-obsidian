---
tags:
  - estudo
  - backend
  - nodejs
  - typescript
status: em-progresso
---

# Backend

> [!summary] Conceitos Base
> - **Req (Request):** Recebe os dados enviados pelo cliente.
> - **Res (Response):** Envia a resposta de volta ao cliente.

## Ferramentas de Processo e Ambiente

- **Nodemon:** Reinicia automaticamente o servidor ao salvar o arquivo.
  ```bash
  npm install -D nodemon
  ```
- **PM2:** Gerenciador de processos voltado para produção e microsserviços.

---

# Banco de Dados

* **SGBD:** Sistema de Gestão de Banco de Dados.
* **Modelo E/R:** Modelo Entidade-Relacionamento.

### Relacionamentos

- $1 \to 1$ (Um para um)
- $1 \to N$ (Um para muitos)
- $N \to N$ (Muitos para muitos)

### Chave Primária (PK)
- **Única**
- **Não vazia**
- **Não muda**

---

# Ecossistema Node & TypeScript

### Compilar TypeScript para JavaScript
```bash
npx tsc
```

### Iniciar Projeto Node
```bash
npm init -y
```

### Executar Projeto
```bash
node index.js
```

---

# MongoDB & Mongoose

- **Mongoose:** ODM (Object Data Modeling) para gerenciar o MongoDB no Node.
```bash
npm install mongoose
```

---

## ❓ Dúvidas a Aprofundar

- [ ] Arrow functions (`() => {}`)
- [ ] Métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`)
- [ ] `async / await`
- [ ] Arquitetura MVC
- [ ] ORM (Conceito e Mapeamento Objeto-Relacional)
- [ ] Sequelize
- [ ] Interfaces no TypeScript
- [ ] Tratamento de erros com `try / catch`
- [ ] Conceito de Props
