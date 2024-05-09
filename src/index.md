Método de Karatsuba para Multiplicações
======


Utilizamos inúmeras multiplicações em nossos programas, mas já parou para se perguntar como essas operações são efetivamente realizadas por trás dos panos? Para números pequenos você deve ter uma idéia já que estudou em elementos de sistemas que a multiplicação é nada mais do que várias somas. Já para numeros grandes, até o ano de 1960, acreditava-se que só era possível ter algoritmos de multiplicação de ordem de complexidade $n^2$, mas Karatsuba trouxe o primeiro algoritmo que contrariou esse fato.

![](mult.png)

??? Pergunta
Faça no papel a seguinte conta:

$$
\begin{array}{r}
  2 \\
\underline{\times \quad 3} \\
\phantom{0000}
\end{array}
$$

Quantas multiplicações de **um dígito** e somas de **um dígito** foram feitas? a partir desses resultados, quantas operações atômicas foram realizadas?
::: Gabarito
O resultado da multiplicação é 6. Foi feita uma multiplicação de um dígito e zero somas de um dígito. Logo, foi realizada uma operação atômica 
:::

???

??? Pergunta
Faça no papel a seguinte conta:

$$
\begin{array}{r}
  31 \\
\underline{\times \quad 22} \\
\phantom{0000}
\end{array}
$$

Quantas multiplicações de **um dígito** e somas de **um dígito** foram feitas?
::: Gabarito
O resultado da multiplicação é 682. Foram feitas quatro multiplicação de um dígito e três somas de um dígito. Logo, foram realizadas cinco operações atômicas
:::

???

Redefinindo nossa operação atômica
----------------------------------------------------

Apesar dos resultados que acabamos de obter, eles estão errados para o contexto dos algoritmos de multiplicação.

Para desenvolvermos a ideia dos algoritmos de multiplicação, é fundamental compreender o custo de cada tipo de operação para o computador. Quando realizamos multiplicações simples entre números de um único algarismo, como, por exemplo, $5 \cdot 6$, estamos essencialmente somando cinco vezes o número seis ($6 + 6 + 6 + 6 + 6$). Assim, se considerarmos que cada soma tem um custo unitário, para calcular o produto de um número n com um algarismo por outro número m, também com um único algarismo, o custo será igual a n ou m, dependendo de qual dos dois números é o menor, para minimizarmos o número de somas necessárias.

Dessa forma, é evidente que os produtos são consideravelmente mais custosos do que as simples somas. Portanto, a partir deste ponto, adotaremos a premissa de que **nossa operação atômica é a multiplicação de números de um único algarismo**. No caso de nos depararmos com uma multiplicação por um valor $B^x$, sendo B a base em que os números estão sendo adotados, não a consideraremos como uma operação atômica, pois é possível realizar a multiplicação por potências da base através de deslocamentos (shifts), como por exemplo, ao calcular $3 \cdot 2^4$, estando os números na base 2, bastaria realizar um deslocamento de 4 casas para a esquerda, ou, para nós humanos que usamos base 10, basta adicionar um zero no fim do número.

Neste handout, usaremos a base igual a 10, pois esse valor é o que estamos acostumados a usar. No entanto, nos computadores, o algoritmo de Karatsuba é aplicado com bases na forma de potências de 2.

Algoritmo de multiplicação ordinária
-----------------------------------------------------

Começaremos entendendo o único algoritmo de multiplicação para números grandes que existia até 1960. Nesse algoritmo usa-se a mesma idéia de multiplicação que utilizamos quando calculamos no papel:

??? Exemplo

Considerando os números 1234 e 5678, chamaremos o total de algarismos em cada um de **n** (nesse caso, n = 4):

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

Repetindo o mesmo processo para os próximos algarismos temos:

$$
\begin{array}{r}
1234\\
\underline{\times \quad 5678} \\
   9872\\
  8638\phantom{0}\\
 7404\phantom{00}\\
6170\phantom{000}\\
\end{array}
$$

Realizando as somas:

$$
\begin{array}{r}
1234\\
\underline{\times \quad 5678} \\
   9872\\
  8638\phantom{0}\\
 7404\phantom{00}\\
\underline{+ \quad 6170\phantom{000}\\} \\
7006652
\end{array}
$$

???


??? Pergunta
Qual é o número de operações atômicas nessa operação?

::: Gabarito
Relembrando que nossa operação atômica é a multiplicação de algarismos de 1 dígito: Multiplicando o algarismo menos significativo do número de baixo por cada um dos 4 algarismos de cima temos 4 multiplicações, como temos 4 números que realizam esse processo, teremos 4x4 = 16 operações atômicas.
:::

???

??? Pergunta
Para um caso geral em que multiplicamos dois números de n dígitos cada, qual será o número operações atômicas nessa operação?

::: Gabarito
Nesse caso, teremos $n \cdot n = n^2$ operações atômicas.
:::

???

Dividindo para conquistar
----------------------------------------------------

Com o objetivo de reduzir a complexidade $O(n^2)$ do algoritmo de multiplicação ordinária, Karatsuba tentou dividir os números que serão multiplicados em dois números com a mesma quantidade de algarismos:

![](quebra.png)

??? Pergunta
Supondo que x e y possuem **n** algarismos cada e que estão na base 10, como poderiamos escrevê-los em função de $x_0$, $x_1$, $y_0$ e $y_1$ ?

::: Gabarito

$$ x = x_1 \cdot 10^{n/2} + x_0$$
$$ y = y_1 \cdot 10^{n/2} + y_0$$

Caso tenha se perguntado, e se n for ímpar? Como por exemplo, se x = 345. Nesse caso, completamos o número com 0 à esquerda até completar um número com quantidade par de algarismos, x = 0345, dividindo então em 03 e 45, ou seja, $3\cdot 10^2 + 45$.
:::

???

A partir desse resultado, podemos calcular $x \cdot y$:
$$ x \cdot y = (x_1 \cdot y_1)\cdot 10^n + (x_1 \cdot y_0 + y_1 \cdot x_0) \cdot 10^{n/2} + x_0 \cdot y_0 $$

??? Pergunta
Qual é o número de operações atômicas nessa operação?

::: Gabarito
Teremos 4 multiplicações de números com n/2 algarismos. Se quando multiplicamos numeros com n algarismos teremos $n^2$ operações atômicas, ao multiplicarmos números com n/2 algarismos teremos $4(n/2)^2  = n^2$ operações atômicas.
:::

???

Pela resposta, concluímos que não tivemos vantagem nessa divisão, pelo menos não ainda:

??? Exercício

Esquecendo um pouco as operações atômicas, qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ x_0^{2} - x_0 \cdot y_0 $$

::: Gabarito

Se você chegou em duas multiplicações, pense novamente! Ao fatorar a expressão da seguinte maneira: $$ x_0\cdot(x_0 - y_0) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

??? Exercício

Considerando que sabemos os valores de $x_0, x_1, y_0, y_1$. Qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ x_0 \cdot y_0 + x_1 \cdot y_1 + x_0 \cdot y_1 + x_1 \cdot y_0 $$

Continue sem utilizar as operações atômicas, considere multiplicações normais.

::: Gabarito

Podemos fatorar a expressão da seguinte maneira: $$ (x_0 + x_1)\cdot(y_0 + y_1) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

Relembrando da multiplicação que obtivemos pelo método da divisão: $$ x \cdot y = (x_1 \cdot y_1)\cdot 10^n + (x_1 \cdot y_0 + y_1 \cdot x_0) \cdot 10^{n/2} + x_0 \cdot y_0 $$

??? Exercício

Suponha que exista um valor k, tal que $$(x_1 \cdot y_0 + y_1 \cdot x_0) =  (k - x_1 \cdot y_1 - x_0 \cdot y_0)$$ como poderiamos obter k com o menor número de multiplicações?


::: Gabarito
Isolando k, temos 
$$k = x_0 \cdot y_0 + x_1 \cdot y_1 + x_0 \cdot y_1 + x_1 \cdot y_0$$ 
mas já sabemos que isso é o mesmo que $k = (x_0 + x_1)\cdot(y_0 + y_1)$

:::

???

Substituindo então na antiga expressão, ficariamos com:

$$ x \cdot y = (x_1 \cdot y_1) \cdot 10^n + (k - x_1 \cdot y_1 - x_0 \cdot y_0) \cdot 10^{n/2} + x_0 \cdot y_0 $$

Agora conseguimos realizar apenas 3 multiplicações distintas ao invés de 4!

??? Pergunta
Qual seria o total de operações atômicas da equação nessa nova fórmula, considerando novamente que $x_0$ e $y_0$ possuem n/2 dígitos e que produtos como $x_0 \cdot y_0$ não precisam ser realizados duas vezes quando já se sabe o valor?

::: Gabarito

Caso $x_0 + x_1$ e $y_0 + y_1$ continuarem possuindo $n/2$ algarismos, ficaríamos com apenas 3 multiplicações de $n/2$ algarismos:

$$x_1\cdot y_1$$
$$(x_0 + x_1)\cdot(y_0 + y_1)$$
$$x_0 \cdot y_0 $$

Sendo assim, ficaríamos com $3(\frac{n}{2})^2 = \frac{3n^2}{4}$ operações atômicas! Já é melhor que $n^2$, não?

:::

???

!!!!!!!!!!!!!
Colocar a partir daqui, a explicação de como funcionaria a recursão e como se chegaria à ordem n elevado a log3 na base 2, sem utilizar tanto o código - RAUL e ILANA
!!!!!!!!!!!!!

??? Pergunta
Como ficaria o código em C atualizado agora?

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
    int k = karatsuba((x1 + x0), (x0 + y0), n/2 + 1);
    
    return x1y1*pow(10, n) +(k - x1y1 - x0y0)*pow(10, n/2) + x0y0;
}
```

:::

???

Nossa nova árvore binária seria:
![](3.png)

A complexidade desse algoritmo é calculada através do [teorema mestre das recorrências de divisão e conquista](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)). Você não precisa entender esse teorema, apenas saber que algoritmos de divisão e conquista cuja recorrência se encaixa no padrão abaixo já tem sua complexidade calculada. $$T(n) = a*T(n/b) + f(n)$$

No nosso caso, observando o primeiro "andar" da árvore binária, notamos que o Algoritmo de Karatsuba tem a seguinte recorrência: $$T(n) = 3*T(n/2) + n$$

Logo, a complexidade é $$O(n^{log2(3)}) ≈ O(n^{1.58})$$



Aplicações para o método de Karatsuba
-------------------------------------

Certo, já entendemos como funciona o algortimo, mas resta a pergunta: por que usaríamos ele em vez do método tradicional? Comentamos que esse método de multiplicação garante uma complexidade e, portanto, uso de memória menor e uma maior velocidade de operação. 

Essas características podem ser muito valorizadas em áreas como a de criptografia assimétrica, onde frequentemente ocorre a multiplicação de números primos grandes; ou também em processamento de sinais digitais (DSP), que envolve multiplicação de polinômios ou convolução de sequência de dados; também em computação científica, que requer simulações computacionais ou modelagem matemática com números grandes e de alta precisão; e em inteligência artificial, que envolve algoritmos de aprendizado de máquina, por vezes com redes neurais profundas ou outras estruturas complexas que frequentemente envolvem multiplicação de matrizes grandes.

Todas essas (e outras) áreas podem se beneficiar de implementações eficientes de operações de multiplicação, que reduz a quantidade de operações intermediárias (diminuindo a complexidade e velocidade), o que faz com que o método de Karatsuba seja muito utilizado.

Para saber mais: Quem foi Karatsuba?
------------------------------------

[Anatoli Alexeievitch Karatsuba](https://pt.wikipedia.org/wiki/Anatoli_Alexeievitch_Karatsuba) (1937-2008) foi um matemático russo célebre por suas contribuições à teoria dos números analítica, números p-ádicos e séries de Dirichlet. Graduado na Universidade Estatal de Moscou, Karatsuba publicou mais de 160 obras, explorando áreas como a distribuição de números primos, a soma de potências e a teoria das formas modulares. Ele publicou o algoritmo que leva seu nome, o "Método Karatsuba", em 1962, com 23 anos.