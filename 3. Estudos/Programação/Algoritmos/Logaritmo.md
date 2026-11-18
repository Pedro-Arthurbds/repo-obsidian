---
tags:
  - estudo
  - algoritmos
  - matematica
status: em-progresso
---

# Logaritmo e Complexidade

> [!info] Definição Intuitiva
> $\mathcal{O}(\log_2 n)$ representa quantas vezes precisamos dividir um problema pela metade sucessivamente até chegar a um único elemento.

---

## Exemplo Numérico

$$\log_2(16) = 4$$

Divisões sucessivas por $2$:

1. $16 \div 2 = 8$ (1ª divisão)
2. $8 \div 2 = 4$ (2ª divisão)
3. $4 \div 2 = 2$ (3ª divisão)
4. $2 \div 2 = 1$ (4ª divisão)

São necessárias **$4$ divisões** para reduzir $16$ a $1$.

---

## Onde isso aparece na Computação?

- **Busca Binária (Binary Search):** Em um vetor ordenado de tamanho $n$, no pior caso faremos $\lceil \log_2 n \rceil$ comparações.
- **Árvores Binárias Balanceadas:** Altura máxima é $\mathcal{O}(\log n)$.
- **Algoritmos Dividir para Conquistar:** Ex: Merge Sort opera em $\mathcal{O}(n \log n)$.
