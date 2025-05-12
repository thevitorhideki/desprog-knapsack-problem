# Problema da Mochila Binária com Programação Dinâmica

## O problema

Para o "problema da Mochila Binária" a ideia é imaginar que você tem uma mochila que suporta um peso máximo, e vários itens, **cada um com um peso e um valor próprios**.

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

---

## Algoritmos gulosos

A primeira abordagem que iremos trazer para esse problema, que é também uma das mais fáceis de pensar intuitivamente, é a dos algoritmos gulosos.

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

Dessa forma, colocaríamos os itens 3 e 1 na mochila e teríamos um {red}(**valor final de 160.**) 
:::
???

??? Exercício
Considerando o resultado anterior, esse é o maior valor que podemos obter para essa mochila?

::: Gabarito
Como o exemplo não é difícil vemos claramente que a solução do algoritmo guloso **{red}(não é a melhor solução)**, já que o maior valor possível de ser obtido para esse mochila, seria de 220 ao escolher os itens 1 e 2.
:::
???

---

## Força bruta

Outra abordagem é usar **força bruta**. Essa abordagem é mais simples, pois basta **testar todas as combinações possíveis** e escolher a melhor.

??? Atividade
Sem listar todas as combinações, **deduza a complexidade** do algoritmo de força bruta em termos de `c n` (número de itens).

::: Gabarito
Para cada um dos `c n` itens, há duas escolhas: incluí-lo (1) ou não (0).  
- Total de combinações possíveis:  $2^n$
- Avaliar a possibilidade de ser a combinação correta: $n$

Portanto, podemos concluir que o algoritmo de força bruta possui complexidade exponencial $O(2^n)$
   
:::
???

---

## Abordagem recursiva ingênua

Como foi possível observar, o algoritmo guloso nem sempre resolve o problema, já que ele usa o critério de "eficiência" (valor/peso), e o algoritmo de força bruta rapidamente se torna impraticável por conta de sua complexidade exponencial.

Agora que percebemos que a questão não é tão simples de resolver, vamos para a abordagem recursiva. Nela vamos dividir o problema em subproblemas menores, com menos items e/ou menor capacidade, seguindo a ideia de [dividir e conquistar](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm).

??? Atividade
Por se tratar de um algoritmo recursivo, vamos precisar implementar a sua base. Com base no que foi visto até agora, qual deverá ser a base dessa recursão?

::: Gabarito
- Se não temos mais items para considerar (`c n = 0`), o valor é 0;
- Se a capacidade da mochila é 0, não podemos incluir mais nada, então o valor é 0

Portanto:

``` c
if (n == 0 || W == 0) {
    return 0;
}
```
:::
???

Após termos a base da recursão definida, precisaremos nos atentar aos casos em que a recursão será invocada.

??? Atividade
Liste, de forma objetiva,  

1. os dois subproblemas que aparecem em cada chamada (incluir o item ou não),
2. como mudam os parâmetros `c n` (número de itens restantes) e `c W` (capacidade restante) em cada um.

*(não precisa escrever código, a ideia em alto nível será suficiente)*

::: Gabarito
1. **Não incluir**: transforma o problema em um subproblema com mochila de mesma capacidade e um número menor de itens, 
    - `c knapsack(pesos, valores, n−1, W)` 
2. **Incluir**: transforma o problema em um subproblema com mochila de capacidade menor, um número menor de itens e um valor agregado, 
    - `c valores[n−1] + knapsack(pesos, valores, n−1, W−pesos[n−1])`
:::
???

Dessa forma, montamos o algoritmo **recursivo** que resolve o problema da mochila binária: 

``` c
int knapsack(int pesos[], int valores[], int n, int W) {
    if (n == 0 || W == 0) {
        return 0;
    }
    
    // Verifica se o item caberia na mochila
    if (pesos[n-1] > W) {
        return knapsack(pesos, valores, n-1, W);
    }
    
    int valor_sem_incluir = knapsack(pesos, valores, n-1, W);
    int valor_incluindo = valores[n-1] + knapsack(pesos, valores, n-1, W - pesos[n-1]);

    return (valor_incluindo > valor_sem_incluir) ? valor_incluindo : valor_sem_incluir;
}
```


## VVV ISSO DEVE SER QUEBRADO EM MAIS PARTES VVV

Apesar desse algoritmo funcionar para o problema da mochila binária, ele não eficiente. Vamos pensar: para cada item, fazemos duas escolhas, incluir ou não incluir, isso cria uma árvore de recursão binária completa com altura *n* (número de items) e uma árvore binária com altura *n* tem $2^{n-1}$ nós no pior caso.

```bash
knapsack(n, W)
    knapsack(n-1, W-w4)
        knapsack(n-2, W-w4-w3)
            ...
        knapsack(n-1, W-w4)
            ...
    knapsack(n-1, W)
        knapsack(n-2, W-w3)
            ...
        knapsack(n-2, W)
            ...
```

Vamos entender com um exemplo numérico:

* Com 10 itens: até ~1000 chamadas recursivas ($2^{10}$)
* Com 20 itens: até ~1000000 chamadas recursivas ($2^{20}$)
* Com 30 itens: até ~1000000000 chamadas recursivas ($2^{30}$)

<br>


**Mas pera aí, nós saímos de um algoritmo que funcionava, era mais fácil de entender e tinha mesma complexidade que esse aqui. Por que eu iria querer usar a versão recursiva?**

Vamos nos acalmar que tudo o que foi apresentado até agora servirá de alicerce para uma otimização tremenda para esse algoritmo.


## O algoritmo melhorado

Para superar o problema da complexidade crescer muito rapidamente na abordagem recursiva pura, utilizaremos **Programação Dinâmica**. 

A ideia central do algoritmo é construirmos uma **tabela** que armazena as soluções ótimas de subproblemas menores, evitando recalcular recursivamente o mesmo estado várias vezes. Essa estratégia é conhecida como [memoização](https://en.wikipedia.org/wiki/Memoization).

Começaremos entendendo o que é a tabela:

- Vamos criar uma matriz `c memo` de dimensões `c (n+1) x (W+1)`, onde:
    - `c n` é o número de itens.
    - `c W` é a capacidade máxima da mochila.
    
- Cada célula `c memo[i][w]` irá guardar o **valor máximo** que podemos obter considerando apenas os primeiros `c i` itens e uma capacidade de mochila `c w`.
- A tabela deve ser inicializada com todas as suas células `c memo[i][w] = -1`.

<br>

??? Atividade
Considerando a função `c knapsack` definida anteriormente, como deveria ser a nova assinatura da função `c knapsack_pd` para que a tabela de memoização seja incluída?

::: Gabarito
``` c
int knapsack_pd(int pesos[], int valores[], int n, int W, int memo[][W+1]);
```
:::
???

??? Atividade
Escreva o conteúdo da função `c knapsack_pd`.

::: Gabarito
``` c
int knapsack_pd(int pesos[], int valores[], int n, int W, int memo[][W+1]) {
    if (n == 0 || W == 0) {
        return 0;
    }
    
    if (memo[n][W] != -1) {
        return memo[n][W];
    }

    int resultado;
    if (pesos[n-1] > W) {
        resultado = knapsack_pd(pesos, valores, n-1, W, memo);
    } else {
        int sem = knapsack_pd(pesos, valores, n-1, W, memo);
        int com = valores[n-1] + knapsack_pd(pesos, valores, n-1, W - pesos[n-1], memo);
        resultado = (com > sem) ? com : sem;
    }

    memo[n][W] = resultado;

    return resultado;
}
```

:::
???

Esta implementação reduz drasticamente o tempo de execução ao evitar resolver subproblemas que já foram resolvidos. A complexidade temporal passa de $O(2^n)$ para $O(n×W)$, onde `c n` é o número de itens e `c W` é a capacidade da mochila.

Veja abaixo, uma animação do preenchimento da tabela utilizando o algoritmo melhorado para o seguinte exemplo:

```c
int pesos[] = {8, 7, 9, 4, 5, 2};
int valores[] = {120, 65, 210, 300, 40, 175};
int n = 6;
int W = 10;
```

:knapsack

Para dar uma ideia da magnitude dessa otimização, pense no exercício abaixo:

??? Atividade
Considerando as condições abaixo:

`c int pesos[] = [12,  7, 19, 24,  5, 17, 10, 34, 2,  27, 14,  8, 30, 21,  3, 16, 25, 11, 28,`
`c 9, 20,  6, 18, 29, 13, 22,  4, 26, 15, 31]`

`c int valores = [120,  65, 210, 300,  40, 175, 100, 400, 15, 350, 155,  80, 420, 260,  25, 180,`
`c 310, 135, 375,  95, 240,  55, 195, 390, 145, 270,  35, 360, 160, 430]`

`c W = 80`

`c n = 30`

Quantas vezes você imagina que o algoritmo recursivo ingênuo será invocado e quantas vezes o algoritmo de programação dinâmica será invocado?
::: Gabarito
O algoritmo ingênuo foi chamado um total de `c 1099019` vezes;

Já o algoritmo de programação dinâmica foi chamado apenas `c 3198` vezes.

Agora tente alterar o valor de `c W` para um número maior que 80.
:::
???
