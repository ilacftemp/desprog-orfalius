Método de Karatsuba para Multiplicações
======


Utilizamos inúmeras multiplicações em nossos programas, mas já parou para se perguntar como essas operações são efetivamente realizadas por trás dos panos? Para números pequenos, você deve ter uma ideia, já que estudou em Elementos de Sistemas que a multiplicação é nada mais do que várias somas. Já para numeros grandes, o matemático russo Anatoly Karatsuba desenvolveu um algoritmo no ano de 1960 que ultrapassou o que acreditava-se ser naquela época o algoritmo de multiplicação para números grandes mais eficiente.

![](mult.png)

Redefinindo nossa operação atômica
----------------------------------------------------

De acordo com nossos conhecimentos de computação, uma operação atômica é qualquer operação do tipo de atribuição (`md =`), aritmética (`md +`, `md -`, `md *`, `md /`, `md %`), comparação (`md ==`, `md !=`, `md <`, `md <=`, `md >`, `md >=`), entre outras operações. No entanto, para a análise de algoritmos de multiplicação, a convenção é que **uma operação atômica é qualquer multiplicação e soma (e seus respectivos derivados) de um dígito**.

Isso é feito porque não faria sentido analisar a complexidade de algoritmos de multiplicação se toda e qualquer multiplicação tem complexidade igual a $O(1)$.

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

Você deve ter obtido duas operações atômicas para a soma e seis para a multiplicação (quatro multiplicações de um dígito, uma soma de um dígito e uma soma de vai-um).

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

Você deve ter obtido quatro operações atômicas para a soma (três somas de um dígito e uma soma de vai-um) e oito para a multiplicação (seis multiplicações de um dígito e duas somas de um dígito).

:::

???

??? Pergunta

Para um caso geral, no qual somamos dois números de n dígitos, quantas operações atômicas você acha que faríamos? E se fosse uma multiplicação?

Observação: não precisa fazer nenhuma conta, responda o que fizer sentido para você.

::: Gabarito

Para a soma, faríamos cerca de $n$ operações.

Para a multiplicação, faríamos cerca de $n^{2}$ operações.

Não são exatamente $n$ operações para somas e $n^{2}$ operações para multiplicações porque podemos ter "vai-ums" durante as somas e, no fim da multiplicação, fazemos algumas somas para obter o resultado final. 

Para efeito de complexidade, isso não muda o fato de que somas têm complexidade O(n) e multiplicações têm complexidade O($n^{2}$).

:::

???


Dividindo para conquistar
----------------------------------------------------

Com o objetivo de reduzir a complexidade $O(n^2)$ do algoritmo de multiplicação ordinária, Karatsuba tentou dividir cada um dos números que serão multiplicados em dois números com a mesma quantidade de algarismos:

![](quebra.png)

??? Exemplo Guiado

Vamos fazer a partição de $x$ e $y$ da imagem acima em $x_0$, $x_1$, $y_0$ e $y_1$.

Como $x$ e $y$ têm 8 dígitos, $x_0$, $x_1$, $y_0$ e $y_1$ terão ${\frac{8}{2}}=4$ dígitos.

Como trabalhamos na base 10, podemos escrever $x$ e $y$ da seguinte maneira:

$$ x = 1111 \cdot 10^{4} + 2222$$
$$ y = 3333 \cdot 10^{4} + 4444$$
???

!!!
Recomendamos que usem papel e caneta para resolver os exercícios.
!!!

??? Exercício
Supondo que x e y possuem **n** algarismos cada e que estão na base 10, como poderiamos escrevê-los em função de $x_0$, $x_1$, $y_0$ e $y_1$?

Dica: é uma generalização do exemplo anterior

::: Gabarito

$$ x = x_1 \cdot 10^{\frac{n}{2}} + x_0$$
$$ y = y_1 \cdot 10^{\frac{n}{2}} + y_0$$

Caso tenha se perguntado, e se n for ímpar? Como por exemplo, se x = 345. Nesse caso, completamos o número com 0 à esquerda até completar um número com quantidade par de algarismos, x = 0345, dividindo então em 03 e 45, ou seja, $3\cdot 10^2 + 45$.
:::

???

A partir desse resultado, podemos calcular $x \cdot y$:
$$ x \cdot y = ({\color{red}x_1 \cdot y_1}) \cdot 10^n + ({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1}) \cdot 10^{\frac{n}{2}} + {\color{orange}x_0 \cdot y_0} $$

??? Exercício
Qual é o número de operações atômicas nessa operação? Considere apenas as multiplicações coloridas.

::: Gabarito
Teremos 4 multiplicações de números com $\frac{n}{2}$ algarismos. Se quando multiplicamos numeros com n algarismos teremos $n^2$ operações atômicas, ao multiplicarmos números com $\frac{n}{2}$ algarismos teremos $4(\frac{n}{2})^2  = n^2$ operações atômicas.

Observação: as multiplicações por potências de dez não foram consideradas. Isso se deve ao fato de que multiplicações **pela base** são simplesmente deslocamentos de dígitos, que têm complexidade $O(n)$.
:::

???

Pela resposta, observamos que não tivemos vantagem nessa divisão comparado ao método tradicional, que também tinha $n^2$ operações, pelo menos não ainda:

??? Exercício

Esquecendo um pouco as operações atômicas, qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ x_0^{2} - x_0 \cdot y_0 $$

Dica: pense em como você pode fatorar essa expressão.

::: Gabarito

Ao fatorar a expressão da seguinte maneira: $$ x_0\cdot(x_0 - y_0) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

??? Exercício

Considerando que sabemos os valores de $x_0, x_1, y_0, y_1$. Qual é a quantidade mínima de multiplicações necessárias para calcular 
$$ {\color{orange}x_0 \cdot y_0} + {\color{red}x_1 \cdot y_1} + {\color{magenta}x_0 \cdot y_1} + {\color{blue}x_1 \cdot y_0} $$

Continue sem utilizar as operações atômicas, considere apenas multiplicações.

Dica: você precisará fazer três fatorações.

::: Gabarito

Podemos fatorar a expressão da seguinte maneira: $$ (x_0 + x_1)\cdot(y_0 + y_1) $$
Nós reduzimos a equação a apenas uma multiplicação!

:::

???

??? Exercício

Suponha que exista um valor k, tal que 
$$k = (x_0 + x_1)\cdot(y_0 + y_1)$$ 
Obtenha o valor de $({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1})$ em função de k, ${\color{orange}x_0 \cdot y_0}$ e ${\color{red}x_1 \cdot y_1}$

Dica: abra o produto e isole a expressão que você deseja calcular.

::: Gabarito
Abrindo a expressão de k, temos 
$$k = {\color{orange}x_0 \cdot y_0} + {\color{red}x_1 \cdot y_1} + {\color{magenta}x_0 \cdot y_1} + {\color{blue}x_1 \cdot y_0}$$ 
Logo, podemos obter $({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1})$ da seguinte maneira:
$$({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1}) = k - {\color{orange}x_0 \cdot y_0} - {\color{red}x_1 \cdot y_1} $$
:::

???

Relembrando da multiplicação que obtivemos pelo método da divisão: $$ x \cdot y = ({\color{red}x_1 \cdot y_1})\cdot 10^n + ({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1}) \cdot 10^{\frac{n}{2}} + {\color{orange}x_0 \cdot y_0} $$
Note que poderíamos obter $x \cdot y$ a partir de apenas os valores de k, ${\color{orange}x_0 \cdot y_0}$ e ${\color{red}x_1 \cdot y_1}$!

Do exercício anterior, sabemos que: $$({\color{blue}x_1 \cdot y_0} + {\color{magenta}x_0 \cdot y_1}) = k - {\color{orange}x_0 \cdot y_0} - {\color{red}x_1 \cdot y_1} $$

Substituindo então na expressão, temos que: $$ x \cdot y = ({\color{red}x_1 \cdot y_1}) \cdot 10^n + (k - {\color{orange}x_0 \cdot y_0} - {\color{red}x_1 \cdot y_1}) \cdot 10^{\frac{n}{2}} + {\color{orange}x_0 \cdot y_0} $$

Dessa forma, conseguimos reduzir o cálculo de $x \cdot y$ a apenas 3 multiplicações em vez de 4!

??? Exercício
Qual seria o total de operações atômicas da equação nessa nova fórmula, considerando novamente que $x_0$, $x_1$, $y_0$ e $y_1$ possuem $\frac{n}{2}$ dígitos e que os produtos ${\color{orange}x_0 \cdot y_0}$ e ${\color{red}x_1 \cdot y_1}$ não precisam ser realizados duas vezes? Considere apenas as multiplicações.

::: Gabarito

Como $x_0 + x_1$ e $y_0 + y_1$ podem ter $n+1$ algarismos em vez de $n$ (pior caso), ficaríamos com apenas 2 multiplicações de $\frac{n}{2}$ algarismos e 1 multiplicação de $\frac{n}{2} + 1$ algarismos:

$${\color{red}x_1 \cdot y_1}$$
$${\color{orange}x_0 \cdot y_0}$$
$$(x_0 + x_1)\cdot(y_0 + y_1)$$

Sendo assim, ficaríamos com $2(\frac{n}{2})^2 + (\frac{n}{2} + 1)^2 = \frac{3n^2}{4} + \frac{n}{2} + 1$ operações atômicas de multiplicação! Já é melhor que $n^2$, não?

:::

???

Cálculo da complexidade
-----------------------

Como vimos logo acima, conseguimos um método que usa apenas 3 multiplicações, que podem ser resolvidas recursivamente, além das outras operações (somas e shifts), que serão de ordem $n$. Podemos usar recursão até chegar na base, que ocorre quando $n = 1$. Portanto, nossa nova árvore binária seria:

![](3.png)

Na base, chegamos em números de apenas 1 algarismo, onde basta devolver o produto (só uma operação atômica).

??? Exercício

Qual a altura da árvore acima?

Dica: pense qual seria a altura de uma árvore cujo n é uma potência de 2.

::: Gabarito

Vamos pensar caso n fosse igual a 1. Já estariamos na base e teríamos h = 1.  
Sendo n = 2. Teríamos h = 2.  
Sendo n = 4. Teríamos h = 3.  
sendo n = 8. Teríamos h = 4.  
...  
Sendo n = n. Teríamos $h = log_{2} n + 1$

:::

???

??? Exercício

No primeiro andar soma-se 1 vez n, no segundo andar 3 vezes n/2, no terceiro 9 vezes n/4, então qual é o termo do quinto andar e quantas vezes somamos ele?

::: Gabarito

No quinto andar teríamos que continuar dividindo por 2 o valor a ser somado, e multiplicar por 3 a quantidade de vezes que o somamos.
Logo teríamos que somar 27 vezes n/8.

:::

???

A partir dessas informações, podemos escrever a soma do número de operações:

$$(n+3\cdot\frac{n}{2}+9\cdot\frac{n}{4}+...+ 3^{h-1} \cdot \frac{n}{2^{h-1}})$$

??? Exercício

Qual é o tipo de progressão dessa soma? Qual é o termo inicial, razão e o total de termos?
Ainda deixe em função de h.

::: Gabarito

Temos uma PG de termo inicial n, razão 3/2 e um total de h termos, lembrando que $h = log_{2} n + 1$
:::

???

!!! Dica
Se precisar, pode relembrar as propriedades logarítmicas no site da matéria usando [este link](https://ensino.hashi.pro.br/desprog/resumo/analise/caixa.html).
!!!

??? Exercício

Aplique a soma de progressões geométricas finitas para os valores acima e encontre uma expressão para a soma total das operações atômicas.
$$S_{m} = \frac{a_{1}\cdot(q^{m}-1)}{(q-1)}$$

$a_1$ é o termo inicial, $m$ o total de termos e $q$ a razão.  
OBS: Você deve chegar em algo que possua $3^{log_{2}^{n}}$, simplifique qualquer outra potência de logarítmo que esteja elevando a própria base.

::: Gabarito

$$ $$

$$S=\frac{n\cdot((\frac{3}{2})^{log_{2}^{n}+1}-1)}{\frac{3}{2}-1}$$

$$ $$

$$S=\frac{n\cdot((\frac{3}{2})^{log_{2}^{n}+1}-1)}{\frac{1}{2}}$$

$$ $$

$$S=\frac{n\cdot((\frac{3}{2})^{log_{2}^{n}} \cdot \frac{3}{2}^{1} -1)}{\frac{1}{2}}$$

$$ $$

$$S=2\cdot n\cdot(\frac{3^{log_{2}^{n}}}{2^{log_{2}^{n}}}\cdot\frac{3}{2}-1)$$

$$ $$

$$S=n\cdot\frac{3^{log_{2}^{n}}}{n}\cdot3-2\cdot n$$

$$ $$

$$S=3^{log_{2}^{n}}\cdot3-2\cdot n$$

$$ $$

:::

???

??? Exercício

Queremos que o n saia do expoente, como podemos fazer isso utilizando as propriedades de logaritmos?

Dica: faça uma mudança de base
::: Gabarito

Através da propriedade de mudança de base temos:
$$ $$

$$S=3^{\frac{log_{3}^{n}}{log_{3}^{2}}}\cdot 3-2\cdot n$$

$$ $$

$$S= n^{\frac{1}{log_{3}^{2}}}\cdot 3-2\cdot n$$

$$ $$

$$S=3\cdot n^{log_{2}^{3}}-2\cdot n$$

$$ $$

:::

???
Aplicando as regras para o cálculo de complexidade, podemos concluir que a complexidade é: $$O(n^{log_{2}^{3}}) ≈ O(n^{1.58})$$

Ufa! Fomos de um algoritmo de complexidade $O(n^2)$ para $O(n^{1.58})$!

Aplicações para o método de Karatsuba
-------------------------------------

Certo, já entendemos como funciona o algortimo, mas resta a pergunta: por que usaríamos ele em vez do método tradicional? Comentamos que esse método de multiplicação garante uma complexidade e, portanto, uso de memória menor e uma maior velocidade de operação. 

Essas características podem ser muito valorizadas em áreas como a de criptografia assimétrica, onde frequentemente ocorre a multiplicação de números primos grandes; ou também em processamento de sinais digitais (DSP), que envolve multiplicação de polinômios ou convolução de sequência de dados; também em computação científica, que requer simulações computacionais ou modelagem matemática com números grandes e de alta precisão; e em inteligência artificial, que envolve algoritmos de aprendizado de máquina, por vezes com redes neurais profundas ou outras estruturas complexas que frequentemente envolvem multiplicação de matrizes grandes.

Todas essas (e outras) áreas podem se beneficiar de implementações eficientes de operações de multiplicação, que reduz a quantidade de operações intermediárias (diminuindo a complexidade e velocidade), o que faz com que o método de Karatsuba seja muito utilizado.

Para saber mais: Quem foi Karatsuba?
------------------------------------

[Anatoli Alexeievitch Karatsuba](https://pt.wikipedia.org/wiki/Anatoli_Alexeievitch_Karatsuba) (1937-2008) foi um matemático russo célebre por suas contribuições à teoria dos números analítica, números p-ádicos e séries de Dirichlet. Graduado na Universidade Estatal de Moscou, Karatsuba publicou mais de 160 obras, explorando áreas como a distribuição de números primos, a soma de potências e a teoria das formas modulares. Ele publicou o algoritmo que leva seu nome, o "Método Karatsuba", em 1962, com 23 anos.