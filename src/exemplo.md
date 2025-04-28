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
## Algoritmos Gulosos

Uma forma de tentar resolver o problema da Mochila Binária é utilizando o que chamamos de **estratégia gulosa** (*greedy*).  
A ideia é simples: **escolher itens que parecem ser a melhor opção no momento**, sem se preocupar com escolhas futuras.

O critério mais comum para fazer essa escolha é:
- Selecionar os itens com **maior valor por peso** primeiro.

Ou seja, calculamos para cada item o valor que ele agrega por cada quilo que ocupa, e tentamos preencher a mochila priorizando os que entregam o **melhor custo-benefício imediato**.

Vamos deixar isso um pouco mais concreto usando o mesmo exemplo anterior:

| Item | Peso (kg) | Valor | Valor/Peso |
|------|-----------|-------|------------|
| Kit primeiros socorros | 1 | 7 | 7.0 |
| Barraca | 5 | 10 | 2.0 |
| Lanterna | 1 | 5 | 5.0 |
| Saco de dormir | 3 | 9 | 3.0 |
| Nintendo Switch | 4 | 6 | 1.5 |
| Cantil | 2 | 8 | 4.0 |

Se usarmos o critério de **maior valor por peso**, a ordem de escolha seria:

1. Kit de primeiros socorros (7.0)
2. Lanterna (5.0)
3. Cantil (4.0)
4. Saco de dormir (3.0)
5. Barraca (2.0)
6. Nintendo Switch (1.5)

E aí vamos colocando os itens na mochila **nessa ordem**, até não caber mais.

!!!

É importante lembrar que **algoritmos gulosos não garantem a melhor solução** para o problema da Mochila Binária.  
Em muitos casos, o caminho guloso **leva a soluções boas, mas não ótimas**.

!!!

---
### Um problema real com o algoritmo guloso:

Seguindo o exemplo acima, usando o método guloso, colocaríamos o kit de primeiros socorros (1kg), a lanterna (1kg), e o cantil (2kg) — totalizando 4kg — ainda sobraria espaço para o saco de dormir (3kg). 

Mas será que isso gera realmente o maior valor possível?  
Em problemas de mochila **não fracionária**, o fato de escolher sempre o melhor custo-benefício imediato pode impedir que combinações melhores sejam formadas depois.

É como montar um quebra-cabeça pegando sempre a "peça mais bonita" — você pode acabar com um monte de peças bonitas, mas sem completar o quadro.

---
??? Exercício mental

Usando o critério guloso de valor por peso, tente montar a mochila até o limite de 10kg e calcule o valor final.  
Depois, tente pensar: será que uma outra combinação daria um valor total maior?

::: Gabarito
Utilizando a estratégia gulosa:
- Kit primeiros socorros (1kg, valor 7)
- Lanterna (1kg, valor 5)
- Cantil (2kg, valor 8)
- Saco de dormir (3kg, valor 9)

Peso total: 1 + 1 + 2 + 3 = 7kg  
Valor total: 7 + 5 + 8 + 9 = **29**

Ainda sobram 3kg, mas os itens restantes (Barraca e Nintendo Switch) são muito pesados ou pouco vantajosos.

No entanto, **com outras combinações**, talvez fosse possível atingir um valor mais alto, priorizando cargas diferentes — é por isso que o guloso não é perfeito para Mochila 0/1!

:::
???

---
## Programação Dinâmica



