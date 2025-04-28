# Problema da Mochila Binária com Programação Dinâmica

## O problema:

Para o "problema da Mochila Binária" a ideia é imaginar que você tem uma mochila que suporta um peso máximo, e vários itens, **cada um com um peso e um valor próprios** *("valor" aqui não é nada relacionado a valor monetário ou coisa do tipo, é somente um valor arbitrário dado ao item para indicar sua "relevância").*

O objetivo é escolher quais itens colocar na mochila para **maximizar o valor total, sem ultrapassar o limite de peso**.

Imagino que tudo esteja um pouco abstrato ainda, portanto, para deixar um pouco mais concreto, observe o exemplo abaixo:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Kit primeiros socorros | 1 | 7 |
| Barraca | 5 | 10 |
| Lanterna | 1 | 5 |
| Saco de dormir | 3 | 9 |
| Nintendo Switch | 4 | 6 |
| Cantil | 2 | 8 |

Nessa tabela existem alguns items aleatórios, cada um com seu respectivo peso e valor. Suponha que a capacidade máxima da sua mochila seja de 10 kg. Seu objetivo é selecionar os itens que vão na mochila de forma que o valor total transportado seja o maior possível, sem ultrapassar esse limite de peso.

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

---
## Programação Dinâmica



