# Problema da Mochila Binária com Programação Dinâmica

## O problema

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

## Força bruta

---

## Algoritmo ingênuo

Como foi possível observar, o algoritmo guloso nem sempre resolve o problema, já que ele usa o critério de "eficiência" (valor/peso), e o algoritmo de força bruta rapidamente se torna impraticável por conta de sua complexidade exponencial.

Agora que percebemos que a questão não é tão simples de resolver, vamos para a abordagem recursiva. Nela vamos dividir o problema em subproblemas menores, com menos items e/ou menor capacidade, seguindo a mesma ideia de [dividir e conquistar](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm).

Estamos interessados, portanto, em uma função que recebe a capacidade da mochila e duas sequências: os pesos e os valores dos items, no final devolvendo qual é o valor máximo dos items que serão adicionados.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade);
```

Para a base da recursão, podemos observar que:

* se não temos mais items para considerar (n = 0), o valor é 0;
* se a capacidade da mochila é 0, não podemos incluir mais nada, então o valor é 0

Além disso, precisamos nos atentar para um caso em que o item atual é muito pesado para a mochila. Nele não é preciso escolher se ele vai ou não vai entrar, só podemos não incluir ele.

Agora para determinar se o item vai ou não entrar na mochila, devemos calcular o valor para cada situação e retornar o maior valor entre as duas opções.

??? Implementando o algoritmo

Escreva uma função recursiva que recebe:

* Um vetor *pesos* de inteiros;
* Um vetor *valores* de inteiros;
* Um inteiro *n* que representa o tamanho de *pesos* e *valores*;
* Um inteiro *capacidade* que representa o tamanho da mochila

e calcula qual é o valor máximo dos items que serão levados na mochila.

Dica: Para implementar o algoritmo descrito acima, pense no passo a passo da construção de uma função recursiva.

::: Gabarito

Passo 1: entenda o que a função recebe e o que deveria fazer.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    // devolve o valor máximo que podemos obter com os primeiros n itens 
    // usando uma mochila com capacidade limitada
}
```

Passo 2: adicione uma chamada recursiva ao código da função.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    knapsack(pesos, valores, ???, ???);
}
```

Passo 3: passe para a chamada recursiva um parâmetro menor.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    knapsack(pesos, valores, n-1, capacidade); // considera um item a menos
}
```

Passo 4: não simularás e terás fé.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    knapsack(pesos, valores, n-1, capacidade); // devolve o valor máximo considerando n-1 itens
}
```

Passo 5: você tem fé na resposta da chamada recursiva, então use-a.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    // Mas espere... temos duas opções: incluir ou não o item n
    
    // Opção 1: não incluir o item n (usar a fé diretamente)
    int valor_sem_incluir = knapsack(pesos, valores, n-1, capacidade);
    
    // Opção 2: incluir o item n (se ele couber)
    int valor_incluindo = valores[n-1] + knapsack(pesos, valores, n-1, capacidade - pesos[n-1]);
    
    // Retornar o melhor dos dois valores
    return (valor_incluindo > valor_sem_incluir) ? valor_incluindo : valor_sem_incluir;
}
```

Passo 6: isole o caso em que o parâmetro é o menor possível.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    // Caso base: se não há mais itens ou capacidade
    if (n == 0 || capacidade == 0) {
    }
    
    // Opção 1: não incluir o item n
    int valor_sem_incluir = knapsack(pesos, valores, n-1, capacidade);
    
    // Opção 2: incluir o item n (se ele couber)
    int valor_incluindo = valores[n-1] + knapsack(pesos, valores, n-1, capacidade - pesos[n-1]);
    
    // Retornar o melhor dos dois valores
    return (valor_incluindo > valor_sem_incluir) ? valor_incluindo : valor_sem_incluir;
}
```

Passo 7: a solução desse caso é trivial, então calcule ela direto.

```c
int knapsack(int pesos[], int valores[], int n, int capacidade) {
    // Caso base: se não há mais itens ou capacidade
    if (n == 0 || capacidade == 0) {
        return 0; // Não podemos obter nenhum valor
    }
    
    // Caso especial: se o item atual não cabe na mochila
    if (pesos[n-1] > capacidade) {
        return knapsack(pesos, valores, n-1, capacidade); // Só podemos não incluí-lo
    }
    
    // Opção 1: não incluir o item n
    int valor_sem_incluir = knapsack(pesos, valores, n-1, capacidade);
    
    // Opção 2: incluir o item n
    int valor_incluindo = valores[n-1] + knapsack(pesos, valores, n-1, capacidade - pesos[n-1]);
    
    // Retornar o melhor dos dois valores
    return (valor_incluindo > valor_sem_incluir) ? valor_incluindo : valor_sem_incluir;
}
```

???

Apesar desse algoritmo funcionar para o problema da mochila binária, ele não é nem um pouco eficiente. Vamos pensar: para cada item, fazemos duas escolhas, incluir ou não incluir ele; isso cria uma árvore de recursão binária completa com altura de *n* (número de items) e uma árvore binária com altura *n* tem 2^n-1 nós no pior caso.

```bash
knapsack(n, W)
    knapsack(n-1, W-w4)
        knapsack(n-2, W-w4-w3)
            ...
        knapsack(n-1, W-w4)
            ...
    knapsack(n, W)
        knapsack(n-1, W-w3)
            ...
        knapsack(n, W)
            ...
```

Vamos entender com um exemplo numérico:

* Com 10 itens: até ~1000 chamadas recursivas (2^10)
* Com 20 itens: até ~1000000 chamadas recursivas (2^20)
* Com 30 itens: até ~1000000000 chamadas recursivas (2^30)

A complexidade exponencial significa que o tempo de execução dobra a cada item adicional que incluímos no problema, como foi visto na solução de força bruta.

## O algoritmo melhorado
