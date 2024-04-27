Método de Karatsuba para Multiplicações
======


Utilizamos multiplicações em nossos programas quase todos os dias, mas já parou para se perguntar como essas operações são efetivamente realizadas por trás dos panos? Para números pequenos você deve ter uma idéia já que estudou em elementos de sistemas que a multiplicação é nada mais do que várias somas. Já para numeros grandes, existem algoritmos responsáveis por esse trabalho.


Algoritmo de multiplicação ordinária
-----------------------------------------------------

Nosso algoritmo ordinário utilizará a mesma idéia de multiplicação que utilizamos quando calculamos no papel:

??? Exemplo

Considerando os números 1234 e 5678, chamaremos o total de algarismos em cada um de **n** (nesse caso n = 5):

$$
\begin{array}{r}
  1234 \\
\underline{\times \quad 5678} \\
\phantom{0000}
\end{array}
$$

Primeiro multiplicamos o dígito menos significativo do segundo número por cada dígito do primeiro número:

$$
\begin{array}{r}
  1234 \\
\underline{\times \quad 5678} \\
  9872
\end{array}
$$
Definiremos nossas operações atômicas como somas e multiplicações, logo temos nesse primeiro passo n multiplicações, ou seja, **n** operações atômicas.

Repetindo o mesmo processo para os próximos algarismos temos:

$$
\begin{array}{r}
1234\\
\underline{\times \quad 5678} \\
   9872\\
  8632\phantom{0}\\
 7404\phantom{00}\\
6170\phantom{000}\\
\end{array}
$$

Nesse ponto já realizamos pelo menos $n*n = n^2$ operações. Entretanto, além disso, ainda precisamos somar as colunas:

$$
\begin{array}{r}
1234\\
\underline{\times \quad 5678} \\
   9872\\
  8632\phantom{0}\\
 7404\phantom{00}\\
\underline{+ \quad 6170\phantom{000}\\} \\
7006652
\end{array}
$$

???

Com isso, teremos somas entrando na conta do total de operações, mas podemos simplificar que o algoritmo ordinário possui uma complexidade $n^2$, o que não parece muito bom, certo?


Dividindo para conquistar
----------------------------------------------------

Karatsuba resolveu dividir os números que serão multiplicados em dois números com a mesma quantidade de algarismos:

![](quebra.png)

Supondo que cada número possui **n** algarismos:
$$ x = x_1 \cdot 10^{n/2} + x_0$$
$$ y = y_1 \cdot 10^{n/2} + y_0$$

portanto:
$$ x \cdot y = (x_1 \cdot y_1)\cdot 10^n + (x_1 \cdot y_0 + y_1 \cdot x_0) \cdot 10^{n/2} + x_0 \cdot y_0 $$

??? Pergunta
Qual é o número de operações atômicas nessa operação?

::: Gabarito
Teremos 4 multiplicações de números com n/2 algarismos + 3 somas.
Quando calculamos o total de operações resultantes ignorando as somas teremos:
$$ 4(n/2)^2  $$
:::

???

Pela resposta parece que não ganhamos muita coisa não é mesmo? E se tentarmos repetir esses passos recursivamente até que os números possuam apenas 1 algarismo para realizar os produtos?

??? Pergunta
Como seria um esboço em C dessa recursão?
::: Gabarito

``` c
int karatsuba(int x, int y, int n) {
    if (n == 1) {
        return x * y;
    }
    
    int x1 = x / pow(10, n/2);
    int x0 = x % pow(10, n/2);
    int y1 = y / pow(10, n/2);
    int y0 = y % pow(10, n/2);
    
    int x1y1 = karatsuba(x1, y1, n / 2);
    int x0y0 = karatsuba(x0, y0, n / 2);
    int x1y0 = karatsuba(x1, y0, n / 2);
    int y1x0 = karatsuba(y1, x0, n / 2);
    
    return x1y1*pow(10, n) +(x1y0 + y1x0)*pow(10, n/2) + x0y0;
}
```

:::

???

Utilizando essa recorrência, podemos ver que a cada recursão teremos 4 chamadas de karatsuba para n/2 e outras n operações atômicas. A árvore ficaria assim:

![](4.png)

??? Pergunta
Qual é a complexidade desse código utilizando a árvore como base?

::: Gabarito
$$ 2n^2 - n $$
:::
???

Nossa complexidade continuou sendo da ordem de $n^2$...

![](gatotriste.webp)


Calma, o famoso matemático Gauss nos ajudou com esse problema:

Chamando de k o seguinte valor:
$$ k = (x_0 + x_1) \cdot (y_0 + y_1)$$

Podemos dizer que
$$ x \cdot y = (x_1 \cdot y_1) \cdot 10^n + (k - x_1 \cdot y_1 - x_0 \cdot y_0) \cdot 10^{n/2} + x_0 \cdot y_0 $$

Recomendamos abrir os calculos caso não tenha acreditado.

Agora teremos 3 multiplicações de tamanho n/2 ao invés de 4!




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



Aplicações para o método de Karatsuba
-------------------------------------

Certo, já entendemos como funciona o algortimo, mas resta a pergunta: por que usaríamos ele em vez do método tradicional? Comentamos que esse método de multiplicação garante uma complexidade e, portanto, uso de memória menor e uma maior velocidade de operação. 

Essas características podem ser muito valorizadas em áreas como a de criptografia assimétrica, onde frequentemente ocorre a multiplicação de números primos grandes; ou também em processamento de sinais digitais (DSP), que envolve multiplicação de polinômios ou convolução de sequência de dados; também em computação científica, que requer simulações computacionais ou modelagem matemática com números grandes e de alta precisão; e em inteligência artificial, que envolve algoritmos de aprendizado de máquina, por vezes com redes neurais profundas ou outras estruturas complexas que frequentemente envolvem multiplicação de matrizes grandes.

Todas essas (e outras) áreas podem se beneficiar de implementações eficientes de operações de multiplicação, que reduz a quantidade de operações intermediárias (diminuindo a complexidade e velocidade), o que faz com que o método de Karatsuba seja muito utilizado.

Para saber mais: Quem foi Karatsuba?
------------------------------------

[Anatoli Alexeievitch Karatsuba](https://pt.wikipedia.org/wiki/Anatoli_Alexeievitch_Karatsuba) (1937-2008) foi um matemático russo célebre por suas contribuições à teoria dos números analítica, números p-ádicos e séries de Dirichlet. Graduado na Universidade Estatal de Moscou, Karatsuba publicou mais de 160 obras, explorando áreas como a distribuição de números primos, a soma de potências e a teoria das formas modulares. Ele publicou o algoritmo que leva seu nome, o "Método Karatsuba", em 1962, com 23 anos.