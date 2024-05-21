Método de Karatsuba para Multiplicações
======


Utilizamos inúmeras multiplicações em nossos programas, mas já parou para se perguntar como essas operações são efetivamente realizadas por trás dos panos? Para números pequenos, você deve ter uma ideia, já que estudou em Elementos de Sistemas que a multiplicação é nada mais do que várias somas. Já para numeros grandes, o matemático russo Anatoly Karatsuba desenvolveu um algoritmo no ano de 1960 que era capaz de reduzir o número de múltiplicações de um dígito entre dois números de n dígitos.

![](mult.png)

Redefinindo nossa operação atômica
----------------------------------------------------

De acordo com nossos conhecimentos de computação, uma operação atômica é qualquer operação do tipo de atribuição (`md =`), aritmética (`md +`, `md -`, `md *`, `md /`, `md %`), comparação (`md ==`, `md !=`, `md <`, `md <=`, `md >`, `md >=`), entre outras operações. No entanto, para a análise de algoritmos de multiplicação, a convenção é que **uma operação atômica é qualquer multiplicação e soma (e seus respectivos derivados) de um dígito**.

Isso é feito porque não faria sentido analisar a complexidade de algoritmos de multiplicação se a própria multiplicação já tem complexidade igual a $O(1)$.

??? Exercício

Conte o número de operações atômicas que você realiza ao fazer as seguintes contas:

$$
\begin{array}{r}
  12 \\
\underline{+ \quad 34} \\
\phantom{0000}
\end{array}
$$

$$
\begin{array}{r}
  12 \\
\underline{\times \quad 34} \\
\phantom{0000}
\end{array}
$$

::: Gabarito

Você deve ter obtido duas operações atômicas para a soma e seis para a multiplicação (quatro multiplicações de um dígito, uma soma normal e uma soma de carry-over).

:::

???

??? Exercício

Conte o número de operações atômicas que você realiza ao fazer as seguintes contas:

$$
\begin{array}{r}
  145 \\
\underline{+ \quad 327} \\
\phantom{0000}
\end{array}
$$

$$
\begin{array}{r}
  111 \\
\underline{\times \quad 32} \\
\phantom{0000}
\end{array}
$$

::: Gabarito

Você deve ter obtido quatro operações atômicas para a soma e oito para a multiplicação (seis multiplicações de um dígito e duas somas de um dígito).

:::

???

??? Pergunta

Para um caso geral, no qual somamos dois números de n dígitos, quantas operações atômicas você acha que faríamos? E se fosse uma multiplicação?

Observação: não precisa fazer nenhuma conta, responda o que fizer sentido para você.

::: Gabarito

Para a soma, faríamos cerca de $n$ operações.

Para a multiplicação, faríamos cerca de $n^{2}$ operações.

Não são exatamente $n$ operações para somas e $n^{2}$ operações para multiplicações porque podemos ter "vai-ums" durante as somas e, no fim da multiplicação, fazemos algumas somas para obter o resultado final. 

Para efeito de complexidade, isso não muda o fato de que somas tem complexidade O(n) e multiplicações tem complexidade O($n^{2}$).

:::

???


Dividindo para conquistar
----------------------------------------------------

Com o objetivo de reduzir a complexidade $O(n^2)$ do algoritmo de multiplicação ordinária, Karatsuba tentou dividir os números que serão multiplicados em dois números com a mesma quantidade de algarismos:

![](quebra.png)

!!!
Recomendamos que usem papel e caneta para resolver os exercícios.
!!!

??? Exercício
Supondo que x e y possuem **n** algarismos cada e que estão na base 10, como poderiamos escrevê-los em função de $x_0$, $x_1$, $y_0$ e $y_1$ ?

::: Gabarito

$$ x = x_1 \cdot 10^{\frac{n}{2}} + x_0$$
$$ y = y_1 \cdot 10^{\frac{n}{2}} + y_0$$

Caso tenha se perguntado, e se n for ímpar? Como por exemplo, se x = 345. Nesse caso, completamos o número com 0 à esquerda até completar um número com quantidade par de algarismos, x = 0345, dividindo então em 03 e 45, ou seja, $3\cdot 10^2 + 45$.
:::

???

A partir desse resultado, podemos calcular $x \cdot y$:
$$ x \cdot y = (x_1 \cdot y_1)\cdot 10^n + (x_1 \cdot y_0 + x_0 \cdot y_1) \cdot 10^{\frac{n}{2}} + x_0 \cdot y_0 $$

??? Exercício
Qual é o número de operações atômicas nessa operação? Considere apenas as multiplicações.

::: Gabarito
Teremos 4 multiplicações de números com $\frac{n}{2}$ algarismos. Se quando multiplicamos numeros com n algarismos teremos $n^2$ operações atômicas, ao multiplicarmos números com $\frac{n}{2}$ algarismos teremos $4(\frac{n}{2})^2  = n^2$ operações atômicas.

Observação: as multiplicações por potências de dez não foram consideradas. Isso se deve ao fato de que multiplicações **pela base** são simplesmente deslocamentos de dígitos, que têm complexidade $O(n)$.
:::

???

Pela resposta, concluímos que não tivemos vantagem nessa divisão comparado ao método tradicional, pelo menos não ainda:

??? Exercício

Esquecendo um pouco as operações atômicas, qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ x_0^{2} - x_0 \cdot y_0 $$

Dica: pense em como você pode fatorar essa expressão.

::: Gabarito

Se você chegou em duas multiplicações, pense novamente! Ao fatorar a expressão da seguinte maneira: $$ x_0\cdot(x_0 - y_0) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

??? Exercício

Considerando que sabemos os valores de $x_0, x_1, y_0, y_1$. Qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ x_0 \cdot y_0 + x_1 \cdot y_1 + x_0 \cdot y_1 + x_1 \cdot y_0 $$

Continue sem utilizar as operações atômicas, considere multiplicações normais.

Dica: você precisará fazer três fatorações.

::: Gabarito

Podemos fatorar a expressão da seguinte maneira: $$ (x_0 + x_1)\cdot(y_0 + y_1) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

??? Exercício

Suponha que exista um valor k, tal que $$k = (x_0 + x_1)\cdot(y_0 + y_1)$$ Obtenha o valor de $(x_1 \cdot y_0 + x_0 \cdot y_1)$ em função de k, $x_0\cdot y_0$ e $x_1\cdot y_1$

Dica: abra o produto e isole a expressão que você deseja calcular.

::: Gabarito
Abrindo a expressão de k, temos 
$$k = x_0 \cdot y_0 + x_1 \cdot y_1 + x_0 \cdot y_1 + x_1 \cdot y_0$$ 
Logo, podemos obter $(x_1 \cdot y_0 + x_0 \cdot y_1)$ da seguinte maneira:
$$(x_1 \cdot y_0 + x_0 \cdot y_1) = k - x_0\cdot y_0 - x_1\cdot y_1 $$
:::

???

??? Exercício
Relembrando da multiplicação que obtivemos pelo método da divisão: $$ x \cdot y = (x_1 \cdot y_1)\cdot 10^n + (x_1 \cdot y_0 + x_0 \cdot y_1) \cdot 10^{\frac{n}{2}} + x_0 \cdot y_0 $$
Como poderíamos obter $x \cdot y$ a partir dos valores de k, $x_0\cdot y_0$ e $x_1\cdot y_1$?

Dica: use o resultado do exercício anterior!

::: Gabarito

Do exercício anterior, sabemos que: $$(x_1 \cdot y_0 + x_0 \cdot y_1) = k - x_0\cdot y_0 - x_1\cdot y_1 $$

Substituindo então na expressão, temos que: $$ x \cdot y = (x_1 \cdot y_1) \cdot 10^n + (k - x_0 \cdot y_0 - x_1 \cdot y_1) \cdot 10^{\frac{n}{2}} + x_0 \cdot y_0 $$

:::

???

Dessa forma, conseguimos reduzir o cálculo de $x \cdot y$ a apenas 3 multiplicações em vez de 4!

??? Exercício
Qual seria o total de operações atômicas da equação nessa nova fórmula, considerando novamente que $x_0$ e $y_0$ possuem $\frac{n}{2}$ dígitos e que produtos como $x_0 \cdot y_0$ não precisam ser realizados duas vezes quando já se sabe o valor?

::: Gabarito

Como $x_0 + x_1$ e $y_0 + y_1$ podem ter $n+1$ algarismos em vez de $n$ (pior caso), ficaríamos com apenas 2 multiplicações de $\frac{n}{2}$ algarismos e 1 multiplicação de $\frac{n}{2} + 1$ algarismos:

$$x_1\cdot y_1$$
$$x_0 \cdot y_0 $$
$$(x_0 + x_1)\cdot(y_0 + y_1)$$

Sendo assim, ficaríamos com $2(\frac{n}{2})^2 + (\frac{n}{2} + 1)^2 = \frac{3n^2}{4} + \frac{n}{2} + 1$ operações atômicas! Já é melhor que $n^2$, não?

:::

???

Cálculo da complexidade
-----------------------

Como vimos logo acima, conseguimos um método que usa apenas 3 multiplicações, que podem ser resolvidas recursivamente, além das outras operações (somas e shifts), que serão de ordem $n$. Podemos usar recursão até chegar na base, que ocorre quando $n = 1$. Portanto, nossa nova árvore binária seria:

![](3.png)

Na base, chegamos em números de apenas 1 algarismo, onde basta devolver o produto (só uma operação atômica).

Considerando que a cada andar o valor de n é dividido por 2, sabemos que a altura da árvore é $log_{2}^{n}$. Durante o processo inteiro de multiplicação, a quantidade de operações atômicas pode ser representada por 
$$(n+3\cdot\frac{n}{2}+9\cdot\frac{n}{4}+...+3^{log_{2}^{n}-1}\cdot2+3^{log_{2}^{n}}\cdot1)$$

??? Exercício

Aplicando a fórmula das progressões geométricas
$$S_{n} = \frac{a_{1}\cdot(q^{n}-1)}{(q-1)}$$

Sendo $a_{1}=n$, $q=\frac{3}{2}$ e $n=log_{2}^{n}+1$, encontre o valor da soma e determine a complexidade do algoritmo de Karatsuba.

!!! Dica
Se precisar, pode relembrar as propriedades logarítmicas no site da matéria usando [este link](https://ensino.hashi.pro.br/desprog/resumo/analise/caixa.html).
!!!

::: Gabarito

A equação pode ser resolvida com algumas manipulações algébricas, como a seguir:

$$ $$

$$S=\frac{n\cdot((\frac{3}{2})^{log_{2}^{n}+1}-1)}{\frac{3}{2}-1}$$

$$ $$

$$S=2\cdot n\cdot(\frac{3^{log_{2}^{n}}}{2^{log_{2}^{n}}}\cdot\frac{3}{2}-1)$$

$$ $$

$$S=n\cdot\frac{3^{log_{2}^{n}}}{n}\cdot3-2\cdot n$$

$$ $$

$$S=3\cdot3^{\frac{log_{3}^{n}}{log_{3}^{2}}}-2\cdot n$$

$$ $$

$$S=3\cdot n^{\frac{1}{log_{3}^{2}}}-2\cdot n$$

$$ $$

$$S=3\cdot n^{log_{2}^{3}}-2\cdot n$$

$$ $$

Considerando que $2\cdot n$ é de menor relevância que $n^{log_{2}^{3}}$, pode-se concluir que a complexidade é $$O(n^{log_{2}^{3}}) ≈ O(n^{1.58})$$

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