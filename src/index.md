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

*Dica: lembre-se que para todo e qualquer item temos apenas duas escolhas, levá-lo ou não.*

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

Por se tratar de um algoritmo recursivo, vamos precisar implementar a sua base. Para isso, vamos às atividades abaixo:

??? Atividade
Considere um cenário onde não há itens para colocar na mochila. Qual seria o valor máximo que podemos obter?

::: Gabarito

- Se não temos mais items para considerar (`c n = 0`), o valor é 0;

Portanto:

``` c
if (n == 0) {
    return 0;
}
```
:::
???


??? Atividade
Agora, imagine uma mochila com capacidade zero. Mesmo que existam itens, o que podemos colocar nela? Qual seria o valor máximo nesse caso?

::: Gabarito

- Se a capacidade da mochila é 0, não podemos incluir mais nada, então o valor é 0

Portanto:

``` c
if (W == 0) {
    return 0;
}
```
:::
???

Portanto, se combinarmos as duas condições de parada da nossa função recursiva, chegaremos ao seguinte resultado:

``` c
if (n == 0 || W == 0) {
    return 0;
}
```

Após termos a base da recursão definida, precisaremos nos atentar aos casos em que a recursão será invocada.

??? Atividade
Liste, de forma objetiva,  

1. O subproblema criado ao escolher **NÃO INCLUIR** o item na mochila,
2. como mudam os parâmetros `c n` (número de itens restantes) e `c W` (capacidade restante) nesse caso.

*(não precisa escrever código, a ideia em alto nível será suficiente)*

::: Gabarito
**Não incluir**: transforma o problema em um subproblema com mochila de mesma capacidade e um número menor de itens, 

- `c knapsack(pesos, valores, n−1, W)`
:::
???

??? Atividade
Liste, de forma objetiva,  

1. O subproblema criado ao escolher **INCLUIR** o item na mochila,
2. como mudam os parâmetros `c n` (número de itens restantes) e `c W` (capacidade restante) nesse caso.

*(não precisa escrever código, a ideia em alto nível será suficiente)*

:::
**Incluir**: transforma o problema em um subproblema com mochila de capacidade menor, um número menor de itens e um valor agregado, 

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

??? Atividade
Baseado no algoritmo que construímos, desenhe como ficaria a árvore de chamadas com *n* items e identifique quantas chamadas são feitas em função de *n*

::: Gabarito

```bash
knapsack(n, W)
    knapsack(n-1, W-w3)
        knapsack(n-2, W-w3-w2)
            knapsack(n-3, W-w3-w2-w1)
                ...
            knapsack(n-3, W-w3-w2)
                ...
        knapsack(n-2, W-w3)
            knapsack(n-3, W-w3-w1)
                ...
            knapsack(n-3, W-w3)
                ...
    knapsack(n-1, W)
        knapsack(n-2, W-w2)
            knapsack(n-3, W-w2-w1)
                ...
            knapsack(n-3, W-w2)
                ...
        knapsack(n-2, W)
            knapsack(n-3, W-w1)
                ...
            knapsack(n-3, W)
                ...
```

Perceba que por conta das duas escolhas: incluir ou não incluir, é criado uma árvore binária de chamadas com altura n, onde cada nível representa uma decisão (incluir ou não o item `c i`). Então, o número total de chamadas recursivas feitas é $2^{n}$

Vamos entender com um exemplo numérico:

- Com 10 itens: até ~1000 chamadas recursivas ($2^{10}$)
- Com 20 itens: até ~1000000 chamadas recursivas ($2^{20}$)
- Com 30 itens: até ~1000000000 chamadas recursivas ($2^{30}$)

???

??? Atividade
Agora com base na árvore que fizemos na última atividade, substitua os valores dos pesos pelos dos seguintes items:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Item 1 | 20 | 100 |
| Item 2 | 30 | 120 |
| Item 3 | 10 | 60 |

Alguma chamada se repete?

::: Gabarito

```bash
knapsack(3, 50)
    knapsack(2, 40)
        knapsack(1, 10)
            # knapsack(0, -10) Inválido
            knapsack(0, 10)
        knapsack(1, 40)
            knapsack(0, 20) # Repete
            knapsack(0, 40)
    knapsack(2, 50)
        knapsack(1, 20)
            knapsack(0, 0)
            knapsack(0, 20) # Repete
        knapsack(1, 50)
            knapsack(0, 30)
            knapsack(0, 50)
```

Como é possível perceber, existe uma chamada que se repete com 3 items, agora imagine um problema com vários items... mais chamadas serão repetidas gastando recursos desnecessários.

???

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

??? Atividade
Considerando os valores abaixo:

```c
int pesos[] = {1, 7, 3, 4, 2, 5};
int valores[] = {120, 65, 160, 200, 175, 40};
int n = 6;
int W = 8;
```

Quais são os valores que preenchem corretamente as células `c memo[i][0]` *(todas as células da primeira linha)* e `c memo[0][w]` *(todas as células da primeira coluna)*.

::: Gabarito
Dizer que `c w = 0` é o mesmo que dizer que a mochila não possui nenhum espaço restante, portanto, o valor possível de ser obtido nesse caso é 0.

O mesmo se aplica para `c i = 0`. Não é possível obter valor se não há itens a serem colocados na mochila.

Dessa forma, tanto a primeira linha quanto a primeira coluna devem ter seus valores zerados:

![](linha_coluna_0.png)
:::
???

Mas ao invés de adicionarmos essa tabela diretamente ao algoritmo recursivo, vamos fazer essa adição à sua versão iterativa *(qualquer problema resolvido com recursão pode ser resolvido com loops)*.

Dessa forma, o código final e adaptado ficará como abaixo:

```c
int knapsack(int pesos[], int valores[], int n, int W) {
    int memo[n+1][W+1];
    
    // inicializa casos base: primeira coluna e primeira linha = 0
    for (int i = 0; i <= n; i++) {
        memo[i][0] = 0;
    }
    for (int w = 0; w <= W; w++) {
        memo[0][w] = 0;
    }
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= W; w++) {
            int sem = memo[i-1][w];
            int com = 0;
            if (pesos[i-1] <= w) {
                com = valores[i-1] + memo[i-1][w - pesos[i-1]];
            }
            memo[i][w] = (sem > com) ? sem : com;
        }
    }
    
    return memo[n][W];
}
```


??? Atividade
Esta implementação reduz drasticamente o tempo de execução ao evitar resolver subproblemas que já foram resolvidos.

Tente fazer o **cálculo da nova complexidade** do algoritmo com memoização.

::: Gabarito

Como cada uma das iterações dos dois laços aninhados (`c i = 1..n`, `c w = 1..W`) faz um número constante de operações *(comparar, somar, indexar)*.

Podemos chegar a conclusão que a **complexidade de tempo total é $O(nW)$**.

Além disso, como é criada a tabela de memoização, a complexidade espacial é $O(nW)$


:::
???

Veja abaixo, uma animação do preenchimento da tabela utilizando o algoritmo melhorado para o seguinte exemplo:

```c
int pesos[] = {1, 7, 3, 4, 2, 5};
int valores[] = {120, 65, 160, 200, 175, 40};
int n = 6;
int W = 8;
```

:knapsack


??? Atividade
A partir da tabela preenchida, como podemos fazer para descobrir os itens levados?

::: Gabarito
Para saber se o item `c i` foi incluído no valor ótimo em `c memo[i][w]`, basta comparar esse número com `c memo[i–1][w]` *(a célula imediatamente acima)*:


Se `c memo[i][w] == memo[i-1][w]`, a solução ótima **não** inclui o item `c i` e a análise deve seguir na mesma coluna.

Se `c memo[i][w] > memo[i-1][w]`, a solução ótima **inclui** o item `c i` e a próxima análise deverá ser feita na coluna referente ao peso atual menos o peso do item.
:::
???

??? Exercício
Identifique os itens que foram levados na mochila para a solução ótima.

![](tabela_completa.png)

::: Gabarito
:solucao

Ao fim, foram escolhidos os itens:

- Item 1: peso=1, valor=120
- Item 4: peso=4, valor=200
- Item 5: peso=2, valor=175
:::
???