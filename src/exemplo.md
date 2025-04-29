# Problema da Mochila Binária com Programação Dinâmica

## O problema:

Para o "problema da Mochila Binária" a ideia é imaginar que você tem uma mochila que suporta um peso máximo, e vários itens, **cada um com um peso e um valor próprios** *("valor" aqui não é nada relacionado a valor monetário ou coisa do tipo, é somente um valor arbitrário dado ao item para indicar sua "relevância").*

O objetivo é escolher quais itens colocar na mochila para **maximizar o valor total, sem ultrapassar o limite de peso**.

Imagino que tudo esteja um pouco abstrato ainda, portanto, para deixar um pouco mais concreto, observe o exemplo abaixo:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Item 1 | 20 | 100 |
| Item 2 | 30 | 120 |
| Item 3 | 10 | 60 |

Nessa tabela existem alguns items aleatórios, cada um com seu respectivo peso e valor. Suponha que a capacidade máxima da sua mochila seja de 50 kg. Seu objetivo é selecionar os itens que vão na mochila de forma que o valor total transportado seja o maior possível, sem ultrapassar esse limite de peso.

É importante perceber que colocar os itens mais leves ou os mais valiosos isoladamente **nem sempre é a melhor estratégia**, mas você já deve ter imaginado isso.

!!!
É importante ter em mente que o nome **"Mochila binária"** indica que os itens são ( 1 ) ou não ( 0 ) levados. Não é possível levar metade ou uma fração de um item.
!!!


??? Exercício mental

Quais você imagina que são algumas das outras abordagens possíveis para resolver esse problema? 

::: Gabarito
Dentre os algoritmos existentes para a resolução desse problema, nesse handout discutiremos a respeito de 3 mais famosos:

1. **Força bruta**, que envolve testar todas as combinações possíveis de itens (todas as sequências de 0s e 1s) e escolher a melhor.
2. **Algoritmos gulosos** ("*greedy*"), em que os itens são escolhidos com base em um critério simples como **valor/peso**.
3. **Programação Dinâmica**, que baseia-se em resolver subproblemas menores e usar essas soluções para montar a solução do problema maior.

*Mais informações sobre esses e outros algoritmos podem ser acessados [nesse link](https://en.wikipedia.org/wiki/Knapsack_problem).*
:::

???


---
## Força bruta

---
## Algoritmos gulosos

A segunda abordagem, que é também uma das mais fáceis de pensar intuitivamente, é a dos algoritmos gulosos. 

Nessa estratégia, a cada iteração escolhemos o item que parece o melhor naquele momento.  No caso da mochila, o critério clássico é ordenar pelos itens de maior valor por peso e enchê-la nessa ordem.

??? Atividade
Considerando uma mochila de 50 kg, tente pensar quais itens seriam levados de acordo com os critérios de seleção do algoritmo guloso, para o exemplo abaixo:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Item 1 | 20 | 100 |
| Item 2 | 30 | 120 |
| Item 3 | 10 | 60 |

::: Gabarito
Primeiro, o algoritmo define qual é a ordem dos itens a partir do critério **valor/peso**.

| Item | Peso (kg) | Valor | Valor/Peso |
|------|-----------|-------|------------|
| Item 3 | 10 | 60 | 6.0 |
| Item 1 | 20 | 100 | 5.0 |
| Item 2 | 30 | 120 | 4.0 |

E a partir disso:

Tenta pegar o **Item 3**: cabe (10 kg), soma peso = 10, valor = 60

Tenta pegar o **Item 1**: cabe (10+20 ≤ 50), soma peso = 30, valor = 160

Tenta pegar o **Item 2**: cabe? (30+30 = 60 kg > 50) → **não cabe, ignora**

Seguindo a ideia do algoritmo guloso, colocaríamos os itens 3 e 1 na mochila e teríamos um valor final de 160. Contudo, vemos claramente que **{red}(essa não é a melhor solução)**, já que o maior valor possível de ser obtido para esse mochila, seria de 220 ao escolher os itens 1 e 2.
:::
???


---
## Programação Dinâmica


