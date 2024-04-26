Método de Karatsuba
======

Projeto de Desafios da Programação
---------

Todos conhecemos o método tradicional de multiplicação, que aprendemos ainda quando crianças. O método de Karatsuba é uma nova forma de multiplicar que diminui a quantidade total de operações e a complexidade durante a multiplicação de número grandes.


Analisando operações de uma multiplicação tradicional
-----------------------------------------------------

Para multiplicar dois números grandes um pelo outro, sempre utilizamos o método tradicional:

??? Exemplo

Considerando os números 1234 e 5678:

Primeiro multiplicamos o dígito menos significativo do segundo número por cada dígito do primeiro número, multiplicando cada um por sua dezena:

$$8*4=32$$
$$8*30=240$$
$$8*200=1600$$
$$8*1000=8000$$
Soma de todas as multiplicações:
$$32 + 240 + 1600 + 8000 = 9872$$

Agora multiplicamos o penúltimo dígito menos significativo com seu valor de dezena pelo primeiro número da mesma forma que fizemos com o menos significativo:

$$70*4=280$$
$$70*30=2100$$
$$70*200=14000$$
$$70*1000=70000$$
Soma de todas as multiplicações:
$$280+2100+14000+70000=86380$$

E assim por diante com cada dígito do segundo valor. No final, somamos os resultados das somas de cada grupo de multiplicações e obtemos o resultado final.

$$9872+86380+740400+510170000=511006572$$

**TA TUDO ERRADO :(( DPS A GNT DESCOBRE ONDE EU FIZ UM ERRO ESTÚPIDO**

Deu **tal** quantidade de operações (todas somas)

???

Partindo para o funcionamento do método de Karatsuba por divisão e conquista
----------------------------------------------------

$$x=x_1*10+x_0$$
$$y=y_1*10+y_0$$

Usando recursão, o método de Karatsuba divide os números na metade, separando seus dígitos diversas vezes, até que cada dígito esteja sozinho (base da recursão) e possa ser multiplicado individualmente com o dígito correspondente (por posição) do segundo valor.

??? Exemplo

O número 12345678 seria dividido em 1234 e 5678.

O número 48347 seria dividido em **48 e 347.(?)**

___

Considerando a multiplicação entre os números 1234 e 5678:

Primeira recursão:

|    primeiro valor   |
|:-------------------:|
|12      \|        34 |


|    segundo valor    |
|:-------------------:|
|    56    \|    78    |


Segunda recursão:

|              primeiro valor               |
|:--------:|:--------:|:--------:|:--------:|
|    1    \|    2    \|    3    \|    4     |


|              segundo valor                |
|:--------:|:--------:|:--------:|:--------:|
|    5    \|    6    \|    7    \|    8     |

___

Agora que os dígitos estão separados individualmente, multiplicamos os correspondentes um pelo outro (imagine que cada multiplição das abaixo ocorre quando uma recursão chega à base, portanto elas não são simultâneas):

Primeira multiplicação (a): $1*5 = 5$

Segunda multiplicação (b): $2*6 = 12$

Terceira multiplicação \(c\): $3*7 = 21$

Quarta multiplicação (d): $4*8 = 32$

Utilizando os nomes acima, atribuídos a cada resultado, o comportamento do método ao começar a retornar da base segue o modelo

$$a*c*10^{2m}+[y - (a*c) - (b*d)]*10^m+b*d$$

onde $m=n/2$, sendo n a quantidade de dígitos do número (a qual varia dependendo da altura em que estamos na árvore binária) e **y é um resultado que veremos um pouco mais para frente.**

Dessa forma, teríamos nesse caso
$$5*21*10^{2*(1/2)}+[y-(5*21)-(12*32)]*10^{1/2}+12*32$$

???

!!! Observação

Sempre supomos que n é par para fazer a divisão (embora o algoritmo já lida com valores ímpares e faz a divisão corretamente). Quando n é ímpar, podemos adicionar um 0 à esquerda, assim temos uma quantidade par de dígitos e já podemos fazer a divisão na metade sem problemas.

??? Exemplos
* Para 371: 0371 pode ser divido em 03 e 71
* Para 5493845: 05493845 pode ser dividido em 0549 e 3845
???

Depois de realizar os cálculos recursivos, os zeros adicionados não afetarão o resultado final.
!!!

Voltando ao y
-------------

No exemplo 3, mencionamos um y misterioso e garantimos que voltaríamos para ele no futuro. Esse y representa o resultado da função recursiva que vem após todas as iniciais (que dividem os número em dígitos até chegar a base em que n (quantidade de dígitos) é 1), em que, em vez de entregar os dois número e a quantidade de dígitos, como feito nas outras chamadas, enviamos como primeiro argumento $a+b$, segundo argumento $c+d$ e terceiro argumento $m+1$. Dessa forma, **blablabla completar aqui**

Implementação em código
-----------------------

``` py
def karatsuba(u, v, n):
    if n <= 3:
        return u*v
    else:
        m = n//2
        a = u//(10**m)
        b = u % (10**m)
        c = v//(10**m)
        d = v % (10**m)
        ac = karatsuba(a, c, m)
        bd = karatsuba(b, d, m)
        y = karatsuba(a+b, c+d, m+1)
        x = ac*10**(2*m) + (y-ac-bd)*10**m + bd
        return x
```

??? Exemplos de chamada da funçâo

``` py
x = 1937
y = 2008
result = karatsuba(x, y, 4)
print(f"The product of {x} and {y} is: {result}")
```

``` py
x = 123
y = 12345
result = karatsuba(x, y, 5)
print(f"The product of {x} and {y} is: {result}")
```

!!! Atenção

Sempre enviamos, na chamada inicial da função, o maior $n$ entre os dois valores a serem multiplicados. O p´roprio código já lida naturalmente com os casos em que os números têm quantidades diferentes de dígitos.

!!!

???

??? Pergunta
Você deve ter notado que a condição de base (``` py if n<= 3 ```) não deixa que o código chegue até a base (dividir o número original até terem apenas 1 dígito). Tente entender o por quê desse valor ser 3 em vez de 1.

::: Gabarito
Essa diferença ocorre pois números com três dígitos já são pequenos o suficiente para não gastar uma quantidade significativa de tempo ou memória ao serem multiplicados. Porém, esse valor pode ser modificado sem problemas e, se quisermos chegar até a base, é só substituí-lo por 1.
:::

???

Calculando a complexidade
-------------------------

Ok, já falamos um monte sobre como a complexidade muda da multiplicação tradicional para a multiplicação por método de Karatsuba e isso é o que faz desse método mais eficiente para números grandes. Mas então, qual é a complexidade desse método?

Para conseguir analisar a complexidade, temos que definir o que é uma operação atômica (básica). No caso estudado, vamos considerar operações básicas adições e multiplicações de dígitos.

Sabemos que se u e v têm n dígitos cada então
* $u × v$ tem no máximo $2n$ dígitos
* $u + v$ tem no máximo $n + 1$ dígitos

Portanto, em uma multiplicação do método tradicional, em que temos que multiplicar cada dígito do segundo número por cada dígito do segundo e depois somar os resultados obtidos de cada grupo de multiplicações, teremos $2n^{2}+n$ operações elementares, que é $n$ vezes mais lenta que adição de dois números (tem $n$ operações elementares).



Aplicações para o método de Karatsuba
-------------------------------------

Certo, já entendemos como funciona o algortimo, mas resta a pergunta: por que usaríamos ele em vez do método tradicional? Comentamos que esse método de multiplicação garante uma complexidade e, portanto, uso de memória menor e uma maior velocidade de operação. 

Essas características podem ser muito valorizadas em áreas como a de criptografia assimétrica, onde frequentemente ocorre a multiplicação de números primos grandes; ou também em processamento de sinais digitais (DSP), que envolve multiplicação de polinômios ou convolução de sequência de dados; também em computação científica, que requer simulações computacionais ou modelagem matemática com números grandes e de alta precisão; e em inteligência artificial, que envolve algoritmos de aprendizado de máquina, por vezes com redes neurais profundas ou outras estruturas complexas que frequentemente envolvem multiplicação de matrizes grandes.

Todas essas (e outras) áreas podem se beneficiar de implementações eficientes de operações de multiplicação, que reduz a quantidade de operações intermediárias (diminuindo a complexidade e velocidade), o que faz com que o método de Karatsuba seja muito utilizado.

Para saber mais: Quem foi Karatsuba?
------------------------------------

[Anatoli Alexeievitch Karatsuba](https://pt.wikipedia.org/wiki/Anatoli_Alexeievitch_Karatsuba) (1937-2008) foi um matemático russo célebre por suas contribuições à teoria dos números analítica, números p-ádicos e séries de Dirichlet. Graduado na Universidade Estatal de Moscou, Karatsuba publicou mais de 160 obras, explorando áreas como a distribuição de números primos, a soma de potências e a teoria das formas modulares. Ele publicou o algoritmo que leva seu nome, o "Método Karatsuba", em 1962, com 23 anos.