# Sistema Básico de Gerenciamento Financeiro (Entradas e Saídas)

Este documento vai guiar o desenvolvimento **front-end e back-end**, passo a passo, de forma **didática**, usando tecnologias que você já conhece:

- **Front-end**: Next.js (App Router), React, TailwindCSS
    
- **Back-end**: Node.js + Express (ou API Routes do Next)
    
- **Banco de dados**: PostgreSQL (com Prisma ORM)
    
- **Autenticação** (opcional): NextAuth / JWT
    

A ideia é construir algo simples, mas **bem estruturado**, para que depois você consiga evoluir sem dor.

---

## 1. O que esse sistema vai fazer?

Funcionalidades básicas:

- Cadastro de **entradas** (ex: salário, venda, recebimento)
    
- Cadastro de **saídas** (ex: aluguel, conta, compra)
    
- Categoria para cada movimentação
    
- Listagem das movimentações
    
- Cálculo automático de:
    
    - Total de entradas
        
    - Total de saídas
        
    - Saldo final
        

---

## 2. Modelagem inicial (pensar antes de codar)

### Entidade principal: Movimentação

Cada entrada ou saída é uma movimentação financeira.

Campos:

- id
    
- description (string)
    
- amount (number)
    
- type (`INCOME` | `EXPENSE`)
    
- category (string)
    
- date (Date)
    
- createdAt
    

---

## 3. Stack escolhida (decisão técnica)

### Por que Prisma?

- Ótimo DX
    
- Tipagem automática
    
- Funciona muito bem com Next.js
    

### Estrutura do projeto

```
/finance-app
 ├─ app
 │   ├─ api
 │   │   └─ transactions
 │   │       └─ route.ts
 │   ├─ dashboard
 │   │   └─ page.tsx
 │   └─ page.tsx
 ├─ prisma
 │   └─ schema.prisma
 ├─ lib
 │   └─ prisma.ts
 └─ components
```

---

## 4. Banco de dados (Prisma)

### schema.prisma

```prisma
model Transaction {
  id          Int      @id @default(autoincrement())
  description String
  amount      Float
  type        TransactionType
  category    String
  date        DateTime
  createdAt   DateTime @default(now())
}

enum TransactionType {
  INCOME
  EXPENSE
}
```

Depois:

```
npx prisma migrate dev
```

---

## 5. Back-end (API)

### lib/prisma.ts

```ts
import { PrismaClient } from '@prisma/client'

export const prisma = new PrismaClient()
```

### API de transações

`app/api/transactions/route.ts`

```ts
import { NextResponse } from 'next/server'
import { prisma } from '@/lib/prisma'

export async function GET() {
  const transactions = await prisma.transaction.findMany({
    orderBy: { date: 'desc' }
  })

  return NextResponse.json(transactions)
}

export async function POST(request: Request) {
  const body = await request.json()

  const transaction = await prisma.transaction.create({
    data: body
  })

  return NextResponse.json(transaction)
}
```

Nesse ponto você já tem um **CRUD básico funcional**.

---

## 6. Front-end (Dashboard)

### Página principal

`app/dashboard/page.tsx`

Responsabilidades:

- Buscar transações
    
- Mostrar resumo financeiro
    
- Listar entradas e saídas
    

Fluxo:

1. `useEffect` → buscar `/api/transactions`
    
2. Calcular totais
    
3. Renderizar cards + tabela
    

---

## 7. Componentes importantes

### Componentes sugeridos:

- `SummaryCard`
    
- `TransactionForm`
    
- `TransactionTable`
    

Separar componentes deixa o projeto **organizado e escalável**.

---

## 8. Estilização (Tailwind)

Padrão visual simples:

- Verde → entradas
    
- Vermelho → saídas
    
- Cinza → neutro
    

Exemplo de card:

```tsx
<div className="bg-white p-4 rounded-xl shadow">
  <p className="text-gray-500">Saldo</p>
  <h2 className="text-2xl font-bold">R$ 1.200,00</h2>
</div>
```

---

## 9. Próximos passos (evolução)

Depois que isso estiver pronto, dá pra evoluir para:

- Autenticação por usuário
    
- Filtro por mês
    
- Gráficos (Recharts)
    
- Exportar CSV / PDF
    
- Deploy (Vercel + Neon)
    

---

## 10. Próximo passo prático

Sugestão de sequência:

1. Criar projeto Next.js
    
2. Configurar Prisma + banco
    
3. Testar API no Postman
    
4. Criar dashboard simples
    

👉 No próximo passo, posso:

- Construir **o dashboard completo com código**
    
- Criar **o formulário de entrada/saída**
    
- Ou refatorar para **Clean Architecture**