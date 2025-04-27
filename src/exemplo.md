# Problema da Mochila binária com programação dinâmica

Esse handout te como objetivo explicar como funciona o algoritmo de programação dinâmica para o problema da mochila binária, mas também propor atividades e fazer vocês construírem a solução para o problema.

## Mochila?

Imagine que você foi convidado pelos seus amigos para fazer uma trilha durante 2 dias. E agora você precisa fazer a sua mala para partir para a aventura. Existem vários items que você gostaria de levar para a trilha mas sua mochila é frágil e não suporta mais do que 15kg. Portanto vai ser necessário avaliar os items que você tem e atribuir valores de importância para eles, como por exemplo:

| Item | Peso (kg) | Valor |
|------|-----------|-------|
| Barraca | 5 | 10 |
| Saco de dormir | 3 | 9 |
| Kit primeiros socorros | 1 | 7 |
| Cantil | 2 | 8 |
| Nintendo Switch | 4 | 6 |
| Lanterna | 1 | 5 |

Seu objetivo é escolher a melhor combinação de items visando maximizar o valor total, sem ultrapassar 15kg. Porém não é possível levar meia Garrafa d'água, por isso o problema da mochila binária: ou leva ou não leva.

O problema tem uma proposta até que simples e intuitiva, mas a solução não chega nem perto disso. Vamos colocar a mão na massa agora!
