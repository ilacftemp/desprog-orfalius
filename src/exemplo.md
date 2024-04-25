Método de Karatsuba
======

Projeto de Desafios da Programação
---------

Todos conhecemos o método tradicional de multiplicação, que aprendemos ainda quando crianças. O método de Karatsuba é uma nova forma de multiplicar que diminui a quantidade total de operações e a complexidade durante a multiplicação de número grandes.

Operações atômicas
------------------

Antes de tudo, para conseguir analisar a complexidade da operação mais para frente, temos que definir o que é uma operação atômica (básica). Em DesProg, costumamos utilizar qualquer símbolo de operação (?). **blabla explicacao da operacao atomica**

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

Partindo para o funcionamento do método de Karatsuba
----------------------------------------------------

$$x=x_1*10+x_0$$
$$y=y_1*10+y_0$$

Usando recursão, o método de Karatsuba divide os número na metade, separando seus dígitos

??? Exemplo

O número 12345678 seria dividido em 1234 e 5678.

O número 48347 seria dividido em **48 e 347.(?)**

???

Essa divisão é feita diversas vezes ao longo da recursão, até que cada dígito esteja sozinho e possa ser multiplicado individualmente com o dígito correspondente (por posição) do outro valor (o que vai ser multiplicado pelo primeiro).

??? Exemplo

Considerando a multiplicação entre os números 1234 e 5678:

Primeira recursão:

|    primeiro valor   |
|----------|----------|
|    12    |    34    |


|    segundo valor    |
|----------|----------|
|    56    |    78    |


Segunda recursão:

|     primeiro valor   |
|-----------|----------|
|  1  |  2  |  3  |  4 |


|    segundo valor     |
|-----------|----------|
|  5  |  6  |  7  |  8 |


Agora que os dígitos estão separados individualmente, multiplicamos os correspondentes um pelo outro:

Primeira multiplicação: $$1*5 = 5$$
Segunda multiplicação: $$2*6 = 12$$
Terceira multiplicação: $$3*7 = 21$$
Quarta multiplicação: $$4*8 = 32$$

???

A fórmula que define como é feito um cálculo usando esse método 

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| primeiro valor      |
|----------|----------|
|    12    |    34    |






Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
