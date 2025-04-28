# Problema da Mochila Binária com Programação Dinâmica

## O problema:

Imagine que você tem uma mochila que suporta um peso máximo, e vários itens, **cada um com um peso e um valor próprios**.

O **valor** representa a importância do item para você — não precisa ser financeiro. Um kit de primeiros socorros, por exemplo, pode ser mais importante que um videogame.

O objetivo é escolher quais itens colocar na mochila para **maximizar o valor total**, respeitando o limite de peso.

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Kit primeiros socorros | 1 | 7 |
| Barraca | 5 | 10 |
| Lanterna | 1 | 5 |
| Saco de dormir | 3 | 9 |
| Nintendo Switch | 4 | 6 |
| Cantil | 2 | 8 |

A capacidade máxima da mochila é de 10kg.

!!!
O nome **"Mochila Binária"** indica que cada item pode ser levado (1) ou não levado (0). Não é permitido levar frações.
!!!

---
??? Atividade

Antes de ver métodos, como você começaria a decidir quais itens levar?  
Anote sua estratégia inicial.

:::
Dica
Pense em escolher os itens mais importantes ou os mais leves primeiro.  
Mas cuidado: será que isso sempre funciona?
:::
???

---
## Algoritmo Guloso

Uma abordagem rápida é usar uma **estratégia gulosa**:  
**priorizar os itens com maior valor por unidade de peso**.

| Item | Peso (kg) | Valor | Valor/Peso |
|------|-----------|-------|------------|
| Kit primeiros socorros | 1 | 7 | 7.0 |
| Lanterna | 1 | 5 | 5.0 |
| Cantil | 2 | 8 | 4.0 |
| Saco de dormir | 3 | 9 | 3.0 |
| Barraca | 5 | 10 | 2.0 |
| Nintendo Switch | 4 | 6 | 1.5 |

Ordem gulosa:
1. Kit primeiros socorros
2. Lanterna
3. Cantil
4. Saco de dormir
5. Barraca
6. Nintendo Switch

!!!
O algoritmo guloso nem sempre encontra a melhor solução em problemas de mochila binária. Ele é ótimo para mochilas fracionárias, mas não aqui.
!!!

---
??? Atividade

Usando a estratégia gulosa, que itens caberiam na mochila sem ultrapassar 10kg? Qual seria o valor total?

:::
Gabarito
Itens: Kit primeiros socorros, Lanterna, Cantil, Saco de dormir.  
Peso: 7kg  
Valor: 7 + 5 + 8 + 9 = **29**
:::
???

---
## Força Bruta

A força bruta é mais simples:  
**testar todas as combinações possíveis** e escolher a melhor.

Cada item pode estar ou não estar na mochila.  
Com `n` itens, existem `2^n` combinações possíveis.

---
??? Atividade

Quantas combinações você acha que existem para:
- 3 itens?
- 5 itens?
- 6 itens?

:::
Gabarito
- 3 itens: 2³ = 8 combinações
- 5 itens: 2⁵ = 32 combinações
- 6 itens: 2⁶ = 64 combinações
:::
???

!!!
A quantidade de combinações cresce **exponencialmente**.  
Mesmo para 20 itens já são mais de 1 milhão de combinações!
!!!

---
## Programação Dinâmica

### Modelagem Recursiva

Antes de pensar em tabelas, vamos ver a lógica recursiva.

Para resolver o problema para `n` itens e capacidade `W`:

- Se **não pegarmos o item `n`**, resolvemos o subproblema para os primeiros `n-1` itens e mesma capacidade `W`.
- Se **pegarmos o item `n`** (se couber), resolvemos o subproblema para os primeiros `n-1` itens e capacidade `W - peso[n]`.

Assim, reduzimos o problema para versões menores dele mesmo.

!!!
Programação dinâmica funciona resolvendo subproblemas menores **e reaproveitando seus resultados**.
!!!

---
??? Atividade

Qual subproblema você resolve se optar por:
- Não pegar um item?
- Pegar um item?

:::
Gabarito
- Não pegar: resolver para o mesmo peso, menos itens.
- Pegar: resolver para peso reduzido, menos itens.
:::
???

---

### Construindo a tabela

Agora que entendemos a recursão, criamos a tabela `dp[i][w]`:

- `i` representa a quantidade de itens considerados (subconjunto dos primeiros `i` itens).
- `w` representa a capacidade disponível.

Cada célula guarda o **maior valor possível** usando até `i` itens e capacidade `w`.

#### Preenchimento:

Para cada célula `dp[i][w]`:
- Se o item `i` **não couber** no peso `w`, copiamos o valor de cima (`dp[i-1][w]`).
- Se couber, escolhemos o melhor entre:
  - Não levar o item (`dp[i-1][w]`),
  - Levar o item (`valor[i] + dp[i-1][w - peso[i]]`).

!!!
Cada célula é preenchida em **tempo constante (O(1))**, e o tempo total é **O(N × W)**.
!!!

---
### Exemplo prático:

- Com 6 itens e capacidade de 10kg:
  - 7 linhas (para 0 até 6 itens),
  - 11 colunas (para 0kg até 10kg).

A resposta final estará em `dp[6][10]`.

---
??? Atividade

Pense: o que `dp[3][7]` representa?

:::
Gabarito
O maior valor possível usando os **três primeiros itens** com capacidade de **7kg**.
:::
???

---
## Complexidade

- **Tempo:** O(N × W)
- **Espaço:** O(N × W)

É possível otimizar o espaço para O(W) usando apenas a linha atual e anterior.

---
## Conclusão

- Greedy é rápido, mas não garante a melhor solução.
- Força bruta encontra a solução ótima, mas é inviável para muitos itens.
- Programação dinâmica é eficiente e garante a solução ótima, quebrando o problema em subproblemas menores e armazenando os resultados.

