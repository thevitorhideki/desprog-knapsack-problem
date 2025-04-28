# Problema da Mochila Binária com Programação Dinâmica

## O problema:

Para o "problema da Mochila Binária" a ideia é imaginar que você tem uma mochila que suporta um peso máximo, e vários itens, **cada um com um peso e um valor próprios**.

**Valor**, aqui, pode ser entendido como qualquer medida de importância: por exemplo, "quanto eu quero esse item para a viagem" — não necessariamente um valor financeiro.  
Por exemplo: um **kit de primeiros socorros** pode ser mais "valioso" que um **Nintendo Switch**, mesmo que comercialmente seja o contrário.

O objetivo é escolher quais itens colocar na mochila para **maximizar o valor total, sem ultrapassar o limite de peso**.

Para deixar isso mais concreto, veja o exemplo:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Kit primeiros socorros | 1 | 7 |
| Barraca | 5 | 10 |
| Lanterna | 1 | 5 |
| Saco de dormir | 3 | 9 |
| Nintendo Switch | 4 | 6 |
| Cantil | 2 | 8 |

Sua mochila suporta **10 kg**. Você precisa escolher os itens que geram **o maior valor total** sem ultrapassar esse peso.

É importante perceber que **escolher os itens mais leves ou mais valiosos isoladamente nem sempre é a melhor estratégia**.

!!!
O termo **"mochila binária"** vem do fato de que, para cada item, só há duas opções: (1) levar o item ou (0) não levar. Não é possível levar frações de um item.
!!!

---
??? Exercício mental

Antes de continuar, que abordagens você imagina que poderiam ser usadas para resolver esse problema?

::: Gabarito
Existem algumas estratégias conhecidas:
1. **Algoritmo guloso** (greedy): escolher os itens de maior valor/peso.
2. **Força bruta**: testar todas as combinações possíveis.
3. **Programação dinâmica**: resolver subproblemas menores e reutilizar suas soluções.
:::
???

---

## Algoritmos Gulosos

Uma primeira abordagem é usar uma **estratégia gulosa**.

A ideia é simples: escolher primeiro os itens que **oferecem mais valor por unidade de peso**.

Por exemplo:

| Item | Peso (kg) | Valor | Valor/Peso |
|------|-----------|-------|------------|
| Kit primeiros socorros | 1 | 7 | 7.0 |
| Barraca | 5 | 10 | 2.0 |
| Lanterna | 1 | 5 | 5.0 |
| Saco de dormir | 3 | 9 | 3.0 |
| Nintendo Switch | 4 | 6 | 1.5 |
| Cantil | 2 | 8 | 4.0 |

Ordem gulosa:
1. Kit primeiros socorros (7.0)
2. Lanterna (5.0)
3. Cantil (4.0)
4. Saco de dormir (3.0)
5. Barraca (2.0)
6. Nintendo Switch (1.5)

Vamos preenchendo a mochila **nessa ordem** até atingir o limite de peso.

!!!
Embora seja rápido e intuitivo, o algoritmo guloso **não garante a solução ótima** para o problema da mochila binária.
!!!

---
### Exemplo prático:

Se levarmos os três primeiros itens (kit, lanterna, cantil), teremos 1 + 1 + 2 = 4kg. Ainda sobraria espaço para o saco de dormir (3kg).

Valor total: 7 + 5 + 8 + 9 = **29**.

Ainda assim, **não sabemos se existe uma combinação melhor**, porque o método guloso não explora todas as opções.

---
??? Exercício mental

Você consegue pensar em uma combinação de itens diferente que talvez gere um valor maior?

:::
Gabarito
Com o método guloso, conseguimos 29 pontos de valor.  
Testando outras combinações, poderíamos encontrar combinações com peso melhor distribuído e valor total mais alto.
:::
???

---

## Força Bruta

Outra abordagem é usar **força bruta**.

Aqui, testamos **todas as combinações possíveis** de itens:
- Cada item pode ser escolhido (1) ou não escolhido (0).
- Para `n` itens, existem `2^n` combinações.

---

### Exemplo prático:

Com 6 itens:

- 2² = 4 combinações → fácil de testar.
- 2³ = 8 combinações → ainda ok.
- 2⁶ = 64 combinações → já começa a aumentar.

!!!
Veja que a quantidade de combinações **cresce muito rápido** à medida que novos itens são adicionados — crescimento exponencial.
!!!

---
### Limitações práticas:

- 10 itens → 2¹⁰ = 1.024 combinações
- 20 itens → 2²⁰ = 1.048.576 combinações
- 30 itens → mais de 1 bilhão de combinações!

Ou seja, a força bruta **sempre encontra a solução ótima**, mas **se torna inviável rapidamente**.

---
??? Exercício mental

Quantas combinações teríamos para 7 itens?

:::
Gabarito
2⁷ = **128 combinações**.
:::
???

---

## Programação Dinâmica

A **programação dinâmica** surge como uma solução intermediária:  
**boa eficiência + garante a solução ótima**.

---

### Primeira parte: a ideia recursiva

Antes de construir qualquer tabela, pense no problema assim:

Para resolver o problema para `n` itens e capacidade `W`, temos duas opções:
1. **Ignorar o item `n`**: o subproblema vira "resolver para os primeiros `n-1` itens e mesma capacidade `W`".
2. **Escolher o item `n`** (se couber): o subproblema vira "resolver para os primeiros `n-1` itens e capacidade `W - peso[n]`".

Cada decisão reduz o problema para uma versão **menor** dele mesmo.

Esse é o coração da programação dinâmica:  
**quebrar o problema em subproblemas menores e reaproveitar suas soluções**.

!!!

Modelamos o problema original como uma combinação de subproblemas menores — sem tabela ainda!

!!!

---
### Segunda parte: a tabela

Criamos uma **tabela `dp[i][w]`**:

- **i** representa a quantidade de itens considerados (subconjunto dos primeiros `i` itens).
- **w** representa a capacidade atual da mochila.

Cada célula `dp[i][w]` responde:  
**Qual é o maior valor que posso alcançar com os primeiros `i` itens e capacidade `w`?**

---
#### Preenchimento da tabela:

Para cada célula `dp[i][w]`:

- Se o item `i` **não for incluído**:  
  - dp[i][w] = dp[i-1][w]
- Se o item `i` **for incluído** (e o peso permitir):  
  - dp[i][w] = dp[i-1][w - peso[i]] + valor[i]

Escolhemos a melhor dessas duas opções.

!!!
Cada célula é preenchida em **tempo constante (O(1))**, e preenchemos toda a tabela em O(N × W).
!!!

---
#### Exemplo prático:

- Com 6 itens e capacidade 10kg:
  - 7 linhas (considerando 0 item até 6 itens)
  - 11 colunas (capacidades de 0kg a 10kg)

A resposta final estará em `dp[6][10]`.

---
#### Complexidade:

- **Tempo:** O(N × W)
- **Espaço:** O(N × W)

Com otimizações, podemos usar só **O(W)** de espaço (guardando só a linha anterior).

---
??? Exercício mental

Por que dizemos que a complexidade é "pseudopolinomial"?

:::
Gabarito
Porque o tempo depende do valor numérico de W, e não apenas do número de bits da entrada.  
Se W for muito grande, o tempo de execução pode crescer demais.
:::
???
