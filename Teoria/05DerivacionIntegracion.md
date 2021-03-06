---
title: "Tema 5 - Derivación e Integración numérica"
author: "Juan Gabriel Gomila y Arnau Mir"
date: ''
output: 
  ioslides_presentation: 
    css: Mery_style.css
    fig_caption: yes
    keep_md: yes
    logo: Images/matrix.gif
    widescreen: yes
---



# Introducción

## Introducción

Las técnicas vistas en un curso clásico de [**cálculo diferencial e integral**](https://www.udemy.com/course/calculo-1/?referralCode=1267D9E121C9168B607B) para hallar la derivada o la integral de una función, no son válidas en general cuando nos enfrentamos en un problema complicado de **análisis numérico**.

La mayoría piensa que una función $f(x)$ es una expresión de la forma $f(x)=\ldots$ donde $\ldots$ es una expresión que contiene términos de funciones conocidas aplicadas a la variable $x$. 
En estos casos, los problemas suelen ser fáciles de tratar ya que conocemos explícitamente la expresión de $f(x)$.

Sin embargo, cuando lidiamos con un problema complejo de análisis numérico, la expresión de $f(x)$ no es conocida. Podemos pensar que $f(x)$ es un programa informático de un número determinado de líneas que tiene la variable $x$ como `input` y nos da un valor $f(x)$ como `output`.

## Introducción
En casos como los descritos anteriormente, no podemos hallar $f'(x)$ ni $\int f(x)\, dx$ usando las técnicas vistas en [el curso de **cálculo**](https://www.udemy.com/course/calculo-1/?referralCode=1267D9E121C9168B607B) ya que dichas técnicas presuponen que conocemos la expresión explícita de $f(x)$.

En estos casos, para hallar la derivada de $f(x)$ en un valor determinado $x$, $f'(x)$ o para hallar la integral $\int_a^b f(x)\, dx$ de $f(x)$ entre dos valores concretos $a$ y $b$, necesitamos conocer otro tipo de técnicas que vamos a aprender en este capítulo.

## Introducción

Además, el problema es incluso más grave de lo que uno podría pensar: 

* en primer lugar, el coste computacional de las técnicas de cálculo para hallar $f'(x)$ o $\int f(x)\, dx$ es elevado y 
* en segundo lugar,conocer la expresión explícita de $f(x)$ no garantiza que podamos hallar la derivada o la integral de $f(x)$. No sólo eso, la mayoría de funciones que uno podría pensar no se pueden integrar usando técnicas de **cálculo**.

Como ejemplo, consideremos la función *campana de Gauss* $f(x)=\frac{1}{\sqrt{2\pi}}\mathrm{e}^{-\frac{x^2}{2}}$. Para dicha función, no es posible hallar una expresión de una primitiva en términos de funciones conocidas y hay que integrarla usando técnicas numéricas.

# Diferenciación numérica

## Introducción
Recordemos la definición de **derivada** de una función $f(x)$ en un valor $x_0$:
$$
f'(x_0)=\lim_{x\to x_0}\frac{f(x)-f(x_0)}{x-x_0}=\lim_{h\to 0}\frac{f(x_0+h)-f(x_0)}{h}.
$$
Las dos expresiones anteriores son equivalentes, basta considerar $h=x-x_0$ para pasar de la primera a la segunda.

Una manera sencilla de **aproximar** $f'(x_0)$ sería considerar el **cociente incremental**
$$
f'(x_0)\approx \frac{f(x_0+h)-f(x_0)}{h},
$$
donde $h$ sería un valor pequeño de cara a que $x_0+h$ esté cerca de $x_0$.

## Diferencias hacia adelante y hacia atrás
El problema de la fórmula anterior es que no tenemos ninguna expresión del **error cometido.**

De cara a solventar dicho problema, vamos a **interpolar la función $f(x)$** en un conjunto de puntos "cercanos" a $x_0$ y derivar la expresión obtenida de cara a obtener una expresión del **error**.

Consideremos en primer lugar los puntos $x_0$ y $x_0+h$. Si $f\in {\cal C}^2[a,b]$ es de clase ${\cal C}^2$ en un intervalo que contenga los puntos anteriores, podemos usar la **fórmula de error de interpolación** y escribir que:
$$
f(x)=P_{0,1}(x)+\frac{(x-x_0)(x-(x_0+h))}{2}\cdot f''(\xi(x)),
$$
donde $P_{0,1}(x)$ es el **polinomio de interpolación** en los puntos $(x_0,f(x_0))$ y $(x_0+h,f(x_0+h))$ y $\xi(x)\in <x,x_0,x_0+h>$ (mínimo intervalo que contiene los puntos $x$, $x_0$ y $x_0+h$)

## Diferencias hacia adelante y hacia atrás
El polinomio $P_{0,1}(x)$ vale usando los **polinomios de Lagrange**:
$$
P_{0,1}(x)=f(x_0)\cdot \frac{(x-x_0-h)}{(-h)}+f(x_0+h)\frac{(x-x_0)}{h}.
$$

Por tanto,
$$
\begin{align*}
f(x)= & f(x_0)\cdot \frac{(x-x_0-h)}{(-h)}+f(x_0+h)\frac{(x-x_0)}{h}\\ & +\frac{(x-x_0)(x-(x_0+h))}{2}\cdot f''(\xi(x)).
\end{align*}
$$


## Diferencias hacia adelante y hacia atrás
Derivando la expresión anterior, obtenemos:
$$
\begin{align*}
f'(x)= &\frac{f(x_0+h)-f(x_0)}{h} +D_x\left[\frac{(x-x_0)(x-(x_0+h))}{2}\cdot f''(\xi(x))\right]\\ = & \frac{f(x_0+h)-f(x_0)}{h} + \frac{2(x-x_0)-h}{2}f''(\xi(x))\\ & + \frac{(x-x_0)(x-(x_0+h))}{2}\cdot D_x\left[f''(\xi(x))\right].
\end{align*}
$$
El problema de la expresión anterior es el término $D_x\left[f''(\xi(x))\right]$ del que no sabemos calcular al no conocer el valor de $\xi(x)$. 

Sin embargo, como nos interesa $f'(x_0)$, este término desaparece para $x=x_0$:
$$
f'(x_0)= \frac{f(x_0+h)-f(x_0)}{h} -\frac{h}{2}f''(\xi(x)).
$$

## Diferencias hacia adelante y hacia atrás
Entonces podemos aproximar $f'(x_0)$ por $\displaystyle\frac{f(x_0+h)-f(x_0)}{h}$ con un error acotado por $\frac{M|h|}{2}$ donde 
$$
M=\max_{x\in <x_0,x_0+h>}|f''(x)|.
$$

Si $h>0$, la fórmula anterior se conoce como **fórmula de diferencias hacia adelante** y si $h<0$, **fórmula de diferencias hacia atrás**.

## Ejemplo


<div class="example">
Consideremos la función $f(x)=\mathrm{e}^{\sin x}$ con $x_0=0$.

Tomando $h=0.05$ la aproximación de $f'(x_0)=f'(0)$ es la siguiente:
$$
f'(0)\approx \frac{f(h)-f(0)}{h}=\frac{1.0512492-1}{0.05}=1.024984.
$$

El valor "real" de $f'(x)$ vale:
$$
f'(x)=\cos x\cdot\mathrm{e}^{\sin x},\ f'(0)=\cos 0\cdot \mathrm{e}^{\sin 0}=1.
$$
El error "real" cometido vale: 
$$
|1.024984-1|=0.024984.
$$
</div>

## Ejemplo
<div class="example">
Hallemos la cota del error. El valor de $f''(x)$ es:
$$
f''(x)=\mathrm{e}^{\sin (x)} (\cos ^2(x)- \sin (x)).
$$
Dicha función está acotada por $2\cdot \mathrm{e}^{\sin 0.05}=2.1024984$ para $x\in [0,0.05]$. Por tanto el valor de $M$ será $M=2.1024984$ y la cota del error será:
$$
\frac{M|h|}{2}=\frac{2.1024984\cdot 0.05}{2}=0.0525625.
$$
Vemos que la cota es aproximadamente el doble del error exacto. Podemos considerarla una buena cota ya que los dos errores son del mismo **orden de magnitud.**

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=_VoOSlCbAy05)
</div>
</div>

## Fórmula general
Vamos a generalizar el procedimiento anterior.

Sean $x_0,x_1,\ldots, x_n$, $n+1$ números distintos en algún intervalo $[a,b]$ y sea $f\in {\cal C}^{n+1}[a,b]$ una función de clase ${\cal C}^{n+1}$ en dicho intervalo. Usando la **fórmula del error en la interpolación** podemos escribir:
$$
f(x)=\sum_{k=0}^n f(x_k)L_k(x)+\frac{(x-x_0)\cdots (x-x_n)}{(n+1)!}f^{(n+1)}(\xi(x)),
$$
donde $L_k(x)$ son los **polinomios de Lagrange** para $k=0,1,\ldots,n$ y $\xi(x)\in <x_0,\ldots,x_n,x>$.

## Fórmula general
Si derivamos la expresión anterior, obtenemos:
$$
\begin{align*}
f'(x)=& \sum_{k=0}^n f(x_k)L_k'(x)+D_x\left[\frac{(x-x_0)\cdots (x-x_n)}{(n+1)!}f^{(n+1)}(\xi(x))\right]\\ = & \sum_{k=0}^n f(x_k)L_k'(x)+D_x\left[\frac{(x-x_0)\cdots (x-x_n)}{(n+1)!}\right]f^{(n+1)}(\xi(x))\\ & + \frac{(x-x_0)\cdots (x-x_n)}{(n+1)!}\cdot D_x\left[f^{(n+1)}(\xi(x))\right].
\end{align*}
$$

## Fórmula general
Si el punto donde aproximamos la derivada es uno de los nodos $x_i$, nos "desaparece" el término que "más molesta" y nos queda:
$$
\begin{align*}
f'(x_i)=& \sum_{k=0}^n f(x_k)L_k'(x_i)\\ & +\frac{1}{(n+1)!}D_x\left[(x-x_0)\cdots (x-x_n)\right]_{|x=x_i}f^{(n+1)}(\xi(x)).
\end{align*}
$$

## Fórmula general

El valor de $D_x\left[(x-x_0)\cdots (x-x_n)\right]|_{x=x_i}$ vale:
$$
\begin{align*}
D_x\left[\prod_{k=0}^n (x-x_k)\right]_{|x=x_i}= & D_x\left[(x-x_i)\prod_{k\neq i}(x-x_k)\right]_{|x=x_i}\\ = & \prod_{k\neq i}(x-x_k)_{|x=x_i}+(x_i-x_i)\cdot \prod_{k\neq i}(x-x_k)_{|x=x_i}\\ = & \prod_{k\neq i}(x_i-x_k)
\end{align*}
$$


## Fórmula general
La expresión de $f'(x_i)$ queda de la forma siguiente:
$$
f'(x_i)= \sum_{k=0}^n f(x_k)L_k'(x_i)+\frac{f^{(n+1)}(\xi_i)}{(n+1)!}\prod_{k\neq i}(x_i-x_k).
$$
A la expresión anterior se le conoce como **fórmula de $n+1$ puntos para aproximar $f'(x_i)$**.

## Fórmulas de los tres puntos
Consideremos el caso particular en que $n=2$ o tenemos tres puntos $x_0$, $x_1$ y $x_2$.

Los **polinomios de Lagrange** y sus derivadas son los siguientes:
$$
\begin{align*}
L_0(x)=&\frac{(x-x_1)(x-x_2)}{(x_0-x_1)(x_0-x_2)},\ \Rightarrow L_0'(x)=\frac{2x-x_1-x_2}{(x_0-x_1)(x_0-x_2)},\\
L_1(x)=&\frac{(x-x_0)(x-x_2)}{(x_1-x_0)(x_1-x_2)},\ \Rightarrow L_1'(x)=\frac{2x-x_0-x_2}{(x_1-x_0)(x_1-x_2)},\\
L_2(x)=&\frac{(x-x_0)(x-x_1)}{(x_2-x_0)(x_2-x_1)},\ \Rightarrow L_2'(x)=\frac{2x-x_0-x_1}{(x_2-x_0)(x_2-x_1)}.
\end{align*}
$$


## Fórmulas de los tres puntos
La expresión de $f'(x_i)$ para $i=0,1,2$ es:
$$
\begin{align*}
f'(x_i)=&f(x_0)\left(\frac{2x_i-x_1-x_2}{(x_0-x_1)(x_0-x_2)}\right)+f(x_1)\left(\frac{2x_i-x_0-x_2}{(x_1-x_0)(x_1-x_2)}\right)\\ & +f(x_2)\left(\frac{2x_i-x_0-x_1}{(x_2-x_0)(x_2-x_1)}\right)+\frac{1}{6}f'''(\xi_i)\prod_{k=0,k\neq i}^2 (x_i-x_k).
\end{align*}
$$

## Fórmulas de los tres puntos
Supongamos ahora que los tres puntos están equiespaciados, es decir, $x_1=x_0+h$ y $x_2=x_0+2h$ para un cierto $h$.

Aplicando la fórmula anterior, tenemos las expresiones siguientes para $f'(x_0)$, $f'(x_1)$ y $f'(x_2)$:
$$
\begin{align*}
f'(x_0)= & \frac{1}{h}\left(-\frac{3}{2}f(x_0)+2f(x_1)-\frac{1}{2}f(x_2)\right)+\frac{h^2}{3}f'''(\xi_0),\\
f'(x_1)= & \frac{1}{h}\left(-\frac{1}{2}f(x_0)+\frac{1}{2}f(x_2)\right)-\frac{h^2}{6}f'''(\xi_1),\\
f'(x_2)= & \frac{1}{h}\left(\frac{1}{2}f(x_0)-2 f(x_1)+\frac{3}{2}f(x_2)\right)+\frac{h^2}{3}f'''(\xi_2).
\end{align*}
$$

## Fórmulas de los tres puntos
Aunque aparezcan tres fórmulas en realidad sólo tenemos dos ya que la primera y la última son la misma.

En la primera tenemos una aproximación de $f'(x_0)$ usando los valores $x_0$, $x_0+h$ y $x_0+2h$ y en la tercera, una aproximación de $f'(x_0+2h)$ usando los mismos valores. 

Si en la tercera "cambiamos" los papeles de $x_0+2h$ por $x_0$ y consideramos $h<0$, nos sale la primera:
$$
\begin{align*}
\mbox{Primera fórmula} & \leftrightarrow \mbox{Tercera fórmula}\\
x_0 & \leftrightarrow x_0+2h\\
x_0+h & \leftrightarrow x_0+h\\
x_0+2h &\leftrightarrow  x_0
\end{align*}
$$

## Fórmulas de los tres puntos
En resumen, tenemos las dos fórmulas siguientes de tres puntos para la aproximación de la derivada:

* Fórmula de los tres puntos respecto del punto medio:
$$
f'(x_0)=  \frac{1}{2h}(f(x_0+h)-f(x_0-h))-\frac{h^2}{6}f'''(\xi_1).
$$
Basta aplicar la segunda expresión anterior cambiando los papeles de $x_0$, $x_1=x_0+h$ y $x_2=x_0+2h$ por $x_0-h$, $x_0$ y $x_0+h$.

* Fórmula de los tres puntos respecto del punto extremo:
$$
f'(x_0)=\frac{1}{2h}(-3 f(x_0)+4 f(x_0+h)-f(x_0+2h))+\frac{h^2}{3}f'''(\xi_0).
$$
Basta aplicar la primera expresión anterior.

## Fórmulas de los tres puntos
<l class="observ">Observación.</l> 

Siempre que sea posible, hay que aplicar la fórmula respecto del punto medio ya que el error queda reducido a la mitad.

## Ejemplo anterior
<div class="example">

Si aplicamos la fórmula de los tres puntos respecto del punto medio en el ejemplo anterior con $h=0.05$, obtenemos:
$$
\begin{align*}
f'(0)=& \frac{1}{2h}(f(h)-f(-h))-\frac{h^2}{6}f'''(\xi_1)=\frac{1}{0.1}(f(0.05)-f(-0.05)-\frac{0.0025}{6}f'''(\xi_1)\\ = & 0.9999996-0.0004167f'''(\xi_1).
\end{align*}
$$
El error cometido real será:
$$
|0.9999996-1|=0.0000004,
$$
y usando que $f'''(x)=\mathrm{e}^{\sin (x)} (\cos ^3x-\cos x-3 \sin x\cos x)$, y por tanto, $\max_{x\in [-0.05,0.05]} |f'''(x)|\leq \mathrm{e}^{\sin 0.05}\cdot 5=5.256246$, la cota del error para $x\in [-0.05,0.05]$ será:
$$
0.0004167\cdot 5.256246=0.0021901.
$$
<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=NbkKYr9oCF_F)
</div>
</div>

## Fórmulas de los cinco puntos
Si en lugar de usar tres puntos equiespaciados, usamos cinco puntos equiespaciados de la forma $x_0,x_0+h,x_0+2h,x_0+3h,x_0+4h$, y razonamos de la misma manera, es decir, calculamos el polinomio de interpolación en los puntos anteriores usando **polinomios de Lagrange**, usamos la **fórmula de error de interpolación** y la derivamos, obtenemos las siguientes fórmulas de cinco puntos:

* Fórmula de los cinco puntos respecto del punto medio:
$$
\begin{align*}
f'(x_0)=& \frac{1}{12h}(f(x_0-2h)-8f(x_0-h)+8f(x_0+h)-f(x_0+2h))\\ & +\frac{h^4}{30}f^{(5)}(\xi),
\end{align*}
$$
donde $\xi\in (x_0-2h,x_0+2h)$.


## Fórmulas de los cinco puntos
* Formula de los cinco punto respecto del valor extremo:
$$
\begin{align*}
f'(x_0)=& \frac{1}{12h}(-25f(x_0)+48f(x_0+h)-36f(x_0+2h)\\ & +16f(x_0+3h) -3f(x_0+4h))+\frac{h^4}{5}f^{(5)}(\xi),
\end{align*}
$$
donde $\xi\in (x_0,x_0+4h)$.

## Ejemplo anterior
<div class="example">
Si aplicamos la fórmula de los cinco puntos respecto del punto medio en el ejemplo anterior con $h=0.05$, obtenemos:
$$
\begin{align*}
f'(0)=& \frac{1}{12h}(f(-2h)-8f(-h)+8f(h)-f(2h))-\frac{h^4}{30}f^{(5)}(\xi)\\ = & \frac{1}{0.6}(f(-0.1)-8f(-0.05)+8f(0.05)-f(0.1))+\frac{0.0000063}{30}f^{(5)}(\xi)\\ = & 1.0000017-0.0000002f^{(5)}(\xi_1).
\end{align*}
$$
El error cometido real será:
$$
|1.0000017-1|=0.0000017,
$$
y usando que $f^{(5)}(x)=\mathrm{e}^{\sin (x)}(\cos ^5 x-10 \cos ^3 x-10 \sin x \cos ^3 x+15 \sin ^2 x \cos x+\cos x+15 \sin x \cos x)$, y por tanto, $\max_{x\in [-0.1,0.1]} |f^{(5)}(x)|\leq \mathrm{e}^{\sin 0.1}\cdot 52=57.4593152$, la cota del error para $x\in [-0.1,0.1]$ será:
$$
0.0000002\cdot 57.4593152=0.000012.
$$
El ejemplo desarrollado es peculiar ya que la fórmula de los tres puntos tiene menos error que la fórmula de cinco puntos con menos **coste computacional**. Este comportamiento no suele darse en el análisis numérico. Es decir, para conseguir menos error, en general, se requiere más **coste computacional**.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=IYXBRce7GR3P)
</div>
</div>

## Derivación numérica usando la fórmula de Taylor
Otra manera de deducir fórmulas de derivación numérica es usar la fórmula de Taylor.

Vamos a ver cómo se obtiene la **fórmula de los cinco puntos respecto del punto medio** usando desarrollos de Taylor.

Si desarrollamos por Taylor la función $f$ alrededor de $x_0$ en los puntos $x_0-2h$, $x_0-h$, $x_0+h$ y $x_0+2h$, obtenemos:
$$
\begin{align*}
f(x_0-2h)=& f(x_0)-f'(x_0)2h+f''(x_0) \frac{4h^2}{2}-f'''(x_0)\frac{8 h^3}{6}\\ & +f^{(4)}(x_0)\frac{16 h^4}{24}-f^{(5)}(\xi_1)\frac{32 h^5}{120},\\
f(x_0-h)=&  f(x_0)-f'(x_0)h+f''(x_0) \frac{h^2}{2}-f'''(x_0)\frac{h^3}{6}\\ & +f^{(4)}(x_0)\frac{ h^4}{24}-f^{(5)}(\xi_2)\frac{h^5}{120},
\end{align*}
$$

## Derivación numérica usando la fórmula de Taylor
$$
\begin{align*}
f(x_0+h)=&  f(x_0)+f'(x_0)h+f''(x_0) \frac{h^2}{2}+f'''(x_0)\frac{h^3}{6}\\ & +f^{(4)}(x_0)\frac{ h^4}{24}+f^{(5)}(\xi_3)\frac{h^5}{120},\\
f(x_0+2h)=& f(x_0)+f'(x_0)2h+f''(x_0) \frac{4h^2}{2}+f'''(x_0)\frac{8 h^3}{6}\\ & +f^{(4)}(x_0)\frac{16 h^4}{24}+f^{(5)}(\xi_4)\frac{32 h^5}{120},
\end{align*}
$$

## Derivación numérica usando la fórmula de Taylor
donde $\xi_1,\xi_2,\xi_3,\xi_4\in (x_0-2h,x_0+2h)$.

El siguiente paso es hallar unos coeficientes $A_1$, $A_2$, $A_3$, $A_4$ y $A_5$ por los que hay que multiplicar las expresiones de $f(x_0-2h), f(x_0-h), f(x_0), f(x_0+h)$ y $f(x_0+2h)$, respectivamente, de cara a eliminar los términos en $h^0$, $h^2$, $h^3$ y $h^4$ de cara a que quede el término en $h f'(x_0)$ para obtener una fórmula aproximada de $f'(x_0)$:

## Derivación numérica usando la fórmula de Taylor
$$
\begin{align*}
\color{red}{A_1}\cdot \Biggl( f(x_0-2h) = & f(x_0)-f'(x_0)2h+f''(x_0) \frac{4h^2}{2}-\cdots,\Biggr)\\
\color{red}{A_2}\cdot \Biggl(f(x_0-h)=&  f(x_0)-f'(x_0)h+f''(x_0) \frac{h^2}{2}-\cdots\Biggr),\\
\color{red}{A_3}\cdot (f(x_0)= & f(x_0)),\\
\color{red}{A_4}\cdot \Biggl(f(x_0+h)=&  f(x_0)+f'(x_0)h+f''(x_0) \frac{h^2}{2}+\cdots \Biggr),\\
\color{red}{A_5}\cdot \Biggl( f(x_0+2h) = & f(x_0)+f'(x_0)2h+f''(x_0) \frac{4h^2}{2}+\cdots,\Biggr)\\
\end{align*}
$$


## Derivación numérica usando la fórmula de Taylor
* El coeficiente correspondiente a $f(x_0)$ es: $A_1+A_2+A_3+A_4+A_5$.
* El coeficiente correspondiente a $f''(x_0)h^2$ es: $2A_1+\frac{1}{2}A_2+\frac{1}{2}A_4+2A_5$.
* El coeficiente correspondiente a $f'''(x_0)h^3$ es: $-\frac{4}{3} A_1-\frac{1}{6}A_2+\frac{1}{6}A_4+\frac{4}{3}A_5$.
* El coeficiente correspondiente a $f^{(4)}(x_0)h^4$ es: $\frac{2}{3} A_1+\frac{1}{24}A_2+\frac{1}{24}A_4+\frac{2}{3}A_5$.


## Derivación numérica usando la fórmula de Taylor

Entonces de cara a anular los coeficientes anteriores, tenemos que resolver el sistema siguiente:
$$
\left.
\begin{align*}
A_1+A_2+A_3+A_4+A_5= & 0,\\
2A_1+\frac{1}{2}A_2+\frac{1}{2}A_4+2A_5 =& 0,\\
-\frac{4}{3} A_1-\frac{1}{6}A_2+\frac{1}{6}A_4+\frac{4}{3}A_5 = & 0,\\
\frac{2}{3} A_1+\frac{1}{24}A_2+\frac{1}{24}A_4+\frac{2}{3}A_5 = &0.
\end{align*}
\right\}
$$

Es un sistema **compatible indeterminado** ya que tiene 4 ecuaciones con 5 incógnitas. Podemos añadir una ecuación extra imponiendo que el coeficiente correspondiente a $hf'(x_0)$ sea $1$:
$$
-2A_1-A_2+A_4+2A_5=1.
$$

## Derivación numérica usando la fórmula de Taylor

Las soluciones del sistema anterior son las siguientes:
$$
A_1=\frac{1}{12},\ A_2=-\frac{2}{3},\ A_3=0,\ A_4=\frac{2}{3},\ A_5=-\frac{1}{12}.
$$
Entonces $A_1\cdot f(x_0-2h)+A_2 f(x_0-h)+A_3 f(x_0)+A_4 f(x_0+h)+A_5 f(x_0+2h)$ vale:
$$
\begin{align*}
 & \frac{1}{12} f(x_0-2h)-\frac{2}{3}f(x_0-h)+\frac{2}{3}f(x_0+h)-\frac{1}{12} f(x_0-2h)\\ & =f'(x_0)h+\frac{h^5}{120}\left(-\frac{32}{12}f^{(5)}(\xi_1)+\frac{2}{3}f^{(5)}(\xi_2)+\frac{2}{3}f^{(5)}(\xi_3)-\frac{32}{12}f^{(5)}(\xi_4)\right).
\end{align*}
$$

## Derivación numérica usando la fórmula de Taylor
Aplicando el **Teorema de Bolzano generalizado** podemos reducir el término del error de la forma siguiente:

* Existe un $\xi_{1,4}$ tal que $-\frac{32}{12}f^{(5)}(\xi_1)-\frac{32}{12}f^{(5)}(\xi_4)=-\frac{16}{3}f^{(5)}(\xi_{1,4})$.
* Existe un $\xi_{2,3}$ tal que
$\frac{2}{3}f^{(5)}(\xi_2)+\frac{2}{3}f^{(5)}(\xi_3)=\frac{4}{3}f^{(5)}(\xi_{2,3})$.

El término del error queda pues:
$$
\frac{h^5}{120}\left(-\frac{16}{3}f^{(5)}(\xi_{1,4})+\frac{4}{3}f^{(5)}(\xi_{2,3})\right).
$$
Si hubiésemos usado la fórmula del **error de interpolación** y los **polinomios de Lagrange**, hubiésemos visto que el término del error se podría escribir como:
$$
-\frac{16}{3}f^{(5)}(\xi_{1,4})+\frac{4}{3}f^{(5)}(\xi_{2,3}) =\left(-\frac{16}{3}+\frac{4}{3}\right)f^{(5)}(\xi)=-4f^{(5)}(\xi).
$$

## Derivación numérica usando la fórmula de Taylor

En resumen,
$$
\begin{align*}
 & \frac{1}{12} f(x_0-2h)-\frac{2}{3}f(x_0-h)+\frac{2}{3}f(x_0+h)-\frac{1}{12} f(x_0+2h)\\ & =f'(x_0)h-\frac{h^5}{120}4f^{(5)}(\xi),
\end{align*}
$$
de donde despejando $f'(x_0)$ obtenemos la fórmula de derivación de los cinco puntos:
$$
\begin{align*}
f'(x_0)=& \frac{1}{h}\left(\frac{1}{12} f(x_0-2h)-\frac{2}{3}f(x_0-h)+\frac{2}{3}f(x_0+h)-\frac{1}{12} f(x_0+2h)\right)\\ & +\frac{h^4}{30}f^{(5)}(\xi),\\
= & \frac{f(x_0-2h)-8f(x_0-h)+8f(x_0+h)-f(x_0+2h)}{12h}+\frac{h^4}{30}f^{(5)}(\xi)
\end{align*}
$$


## Derivadas de orden superior
La técnica anterior nos permite usar fórmulas numéricas aproximadas para hallar derivadas de orden superior.

Calculemos como ejemplo una aproximación de $f''(x_0)$. 

Considerando los mismos puntos anteriores, $x_0\pm h$ y $x_0\pm 2h$ y los mismos desarrollos de Taylor anteriores, hemos de calcular unos coeficientes $B_1,B_2,B_3,B_4$ y $B_5$ por los que hay que multiplicar las expresiones de $f(x_0-2h), f(x_0-h), f(x_0), f(x_0+h)$ y $f(x_0+2h)$, respectivamente, de cara a eliminar los términos en $h^0$, $h$, $h^3$ y $h^4$ de cara a que quede el término en $h^2 f''(x_0)$ para obtener una fórmula aproximada de $f''(x_0)$.

## Derivadas de orden superior

En primer lugar, desarrollamos por Taylor la función $f$ alrededor de $x_0$ en los puntos $x_0-2h$, $x_0-h$, $x_0+h$ y $x_0+2h$ hasta llegar a términos del orden $h^6$:
$$
\begin{align*}
f(x_0-2h)=& f(x_0)-f'(x_0)2h+f''(x_0) \frac{4h^2}{2}-f'''(x_0)\frac{8 h^3}{6}\\ & +f^{(4)}(x_0)\frac{16 h^4}{24}-f^{(5)}(x_0)\frac{32 h^5}{120}+f^{(6)}(\xi_1) \frac{64 h^6}{720},\\
f(x_0-h)=&  f(x_0)-f'(x_0)h+f''(x_0) \frac{h^2}{2}-f'''(x_0)\frac{h^3}{6}\\ & +f^{(4)}(x_0)\frac{ h^4}{24}-f^{(5)}(x_0)\frac{h^5}{120}+f^{(6)}(\xi_2) \frac{h^6}{720},
\end{align*}
$$

## Derivadas de orden superior

$$
\begin{align*}
f(x_0+h)=&  f(x_0)+f'(x_0)h+f''(x_0) \frac{h^2}{2}+f'''(x_0)\frac{h^3}{6}\\ & +f^{(4)}(x_0)\frac{ h^4}{24}+f^{(5)}(x_0)\frac{h^5}{120}+f^{(6)}(\xi_3) \frac{h^6}{720},\\
f(x_0+2h)=& f(x_0)+f'(x_0)2h+f''(x_0) \frac{4h^2}{2}+f'''(x_0)\frac{8 h^3}{6}\\ & +f^{(4)}(x_0)\frac{16 h^4}{24}+f^{(5)}(x_0)\frac{32 h^5}{120}+f^{(6)}(\xi_4) \frac{64 h^6}{720},
\end{align*}
$$


## Derivadas de orden superior

$$
\begin{align*}
\color{red}{B_1}\cdot \Biggl( f(x_0-2h) = & f(x_0)-f'(x_0)2h+f''(x_0) \frac{4h^2}{2}-\cdots,\Biggr)\\
\color{red}{B_2}\cdot \Biggl(f(x_0-h)=&  f(x_0)-f'(x_0)h+f''(x_0) \frac{h^2}{2}-\cdots\Biggr),\\
\color{red}{B_3}\cdot (f(x_0)= & f(x_0)),\\
\color{red}{B_4}\cdot \Biggl(f(x_0+h)=&  f(x_0)+f'(x_0)h+f''(x_0) \frac{h^2}{2}+\cdots \Biggr),\\
\color{red}{B_5}\cdot \Biggl( f(x_0+2h) = & f(x_0)+f'(x_0)2h+f''(x_0) \frac{4h^2}{2}+\cdots,\Biggr)\\
\end{align*}
$$


## Derivadas de orden superior

* El coeficiente correspondiente a $f(x_0)$ es: $B_1+B_2+B_3+B_4+B_5$.
* El coeficiente correspondiente a $f'(x_0)h$ es: $-2B_1-B_2+B_4+2B_5$.
* El coeficiente correspondiente a $f'''(x_0)h^3$ es: $-\frac{4}{3} B_1-\frac{1}{6}B_2+\frac{1}{6}B_4+\frac{4}{3}B_5$.
* El coeficiente correspondiente a $f^{(4)}(x_0)h^4$ es: $\frac{2}{3} B_1+\frac{1}{24}B_2+\frac{1}{24}B_4+\frac{2}{3}B_5$.


## Derivadas de orden superior

Entonces de cara a anular los coeficientes anteriores, tenemos que resolver el sistema siguiente:
$$
\left.
\begin{align*}
B_1+B_2+B_3+B_4+B_5= & 0,\\
-2B_1-B_2+B_4+2B_5 =& 0,\\
-\frac{4}{3} B_1-\frac{1}{6}B_2+\frac{1}{6}B_4+\frac{4}{3}B_5 = & 0,\\
\frac{2}{3} B_1+\frac{1}{24}B_2+\frac{1}{24}B_4+\frac{2}{3}B_5 = &0.
\end{align*}
\right\}
$$

Es un sistema **compatible indeterminado** ya que tiene 4 ecuaciones con 5 incógnitas. Podemos añadir una ecuación extra imponiendo que el coeficiente correspondiente a $h^2f''(x_0)$ sea $1$:
$$
2B_1+\frac{1}{2}B_2+\frac{1}{2}B_4+2B_5=1.
$$


## Derivadas de orden superior

Las soluciones del sistema anterior son las siguientes:
$$
B_1=-\frac{1}{12},\ B_2=\frac{4}{3},\ B_3=-\frac{5}{2},\ B_4=\frac{4}{3},\ B_5=-\frac{1}{12}.
$$
En este caso, observamos que el coeficiente de $h^5$ también se anula:
$$
-\frac{32B_1}{120}-\frac{B_2}{120}+\frac{B_4}{120}+\frac{32B_5}{120}=\frac{1}{1440}(32-48+48-32)=0.
$$

## Derivadas de orden superior

Entonces $B_1\cdot f(x_0-2h)+B_2 f(x_0-h)+B_3 f(x_0)+B_4 f(x_0+h)+B_5 f(x_0+2h)$ vale:
$$
\begin{align*}
 & -\frac{1}{12} f(x_0-2h)+\frac{4}{3}f(x_0-h)-\frac{5}{2}f(x_0)+\frac{4}{3}f(x_0+h)-\frac{1}{12} f(x_0-2h)\\ & =f''(x_0)h^2+\frac{h^6}{720}\left(-\frac{64}{12}f^{(6)}(\xi_1)+\frac{4}{3}f^{(6)}(\xi_2)+\frac{4}{3}f^{(6)}(\xi_3)-\frac{64}{12}f^{(6)}(\xi_4)\right).
\end{align*}
$$


## Derivadas de orden superior

Aplicando el **Teorema de Bolzano generalizado** podemos reducir el término del error de la forma siguiente:

* Existe un $\xi_{1,4}$ tal que $\frac{64}{12}f^{(6)}(\xi_1)+\frac{64}{12}f^{(6)}(\xi_4)=\frac{32}{3} f^{(6)}(\xi_{1,4})$.
* Existe un $\xi_{2,3}$ tal que
$\frac{4}{3}f^{(6)}(\xi_2)+\frac{4}{3}f^{(6)}(\xi_3)=\frac{8}{3} f^{(6)}(\xi_{2,3})$.

El término del error queda pues:
$$
\frac{h^6}{720}\left(\frac{8}{3}f^{(6)}(\xi_{2,3})-\frac{32}{3}f^{(6)}(\xi_{1,4})\right)=\frac{h^6}{270}\left(f^{(6)}(\xi_{2,3})-4f^{(6)}(\xi_{1,4})\right)
$$


## Derivadas de orden superior

En resumen,
$$
\begin{align*}
 & -\frac{1}{12} f(x_0-2h)+\frac{4}{3}f(x_0-h)-\frac{5}{2}f(x_0)+\frac{4}{3}f(x_0+h)-\frac{1}{12} f(x_0+2h)\\ & =f''(x_0)h^2-\frac{h^6}{270}\left(4f^{(6)}(\xi_{1,4})-f^{(6)}(\xi_{2,3})\right),
\end{align*}
$$

## Derivadas de orden superior
de donde despejando $f''(x_0)$ obtenemos la fórmula de derivación de los cinco puntos:
$$
\begin{align*}
f''(x_0)=& \frac{1}{h^2}\Biggl( -\frac{1}{12} f(x_0-2h)+\frac{4}{3}f(x_0-h)-\frac{5}{2}f(x_0)+\frac{4}{3}f(x_0+h)\\ &-\frac{1}{12} f(x_0+2h)\Biggr) +\frac{h^4}{270}\left(4f^{(6)}(\xi_{1,4})-f^{(6)}(\xi_{2,3})\right),\\
= & \frac{1}{12h^2}\Bigl(-f(x_0-2h)+16f(x_0-h)-30 f(x_0)+16f(x_0+h)\\ & -f(x_0+2h)\Bigr)+\frac{h^4}{270}\left(4f^{(6)}(\xi_{1,4})-f^{(6)}(\xi_{2,3})\right).
\end{align*}
$$

## Ejemplo
<div class="example">


Vamos a aproximar $f''(0)$ en el ejemplo anterior donde recordemos que $f(x)=\mathrm{e}^{\sin x}$. Si aplicamos la fórmula anterior con $h=0.05$, obtenemos:
$$
\begin{align*}
f''(0)=&\frac{1}{12h^2}(-f(-2h)+16 f(-h)-30 f(0)+16 f(h)-f(2h))+\frac{h^4}{270}(4f^{(6)}(\xi_1)-f^{(6)}(\xi_2))\\ 
= & \frac{1}{0.03}(-\mathrm{e}^{\sin(-0.1)}+16\mathrm{e}^{\sin (-0.05)}-30\mathrm{e}^{\sin 0}+16\mathrm{e}^{\sin 0.05}-\mathrm{e}^{\sin 0.1})\\ & +0.00000002314815(4f^{(6)}(\xi_1)-f^{(6)}(\xi_2))\\ 
=&  1.0000002+0.00000002314815(4f^{(6)}(\xi_1)-f^{(6)}(\xi_2))
\end{align*}
$$


El valor de $f''(x)$ vale $f''(x)=\mathrm{e}^{\sin (x)} \left(\cos ^2(x)-\sin (x)\right)$. Por tanto, $f''(0)=1$. Vemos que hemos aproximado $f''(0)$ con un error real de:
$$
|1.0000002-1|=0.0000002.
$$

Nuestra cota del error es del orden $O(h^4)\approx K\cdot 0.0000063$, donde $K$ es una constante que depende de la derivada sexta.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=iTdwwFEqGyg0)
</div>
</div>

## Inestabilidad del error de redondeo
En todas las aproximaciones anteriores, sólo hemos tenido en cuenta el error de **truncamiento** o el error que cometemos cuando aproximamos $f'(x_0)$, $f''(x_0)$ o incluso derivadas superiores a partir de **interpolar** o de desarrollar $f$ usando la **fórmula de Taylor** pero no hemos tenido en cuenta el **error de redondeo**.

Es decir, no hemos tenido en cuenta que cuando **evaluamos** la función $f$ en un cierto punto $x$, en realidad no obtenemos el **valor exacto** $f(x)$, sino una **aproximación** $\tilde{f}(x)$.



## Inestabilidad del error de redondeo
Por ejemplo, en la **fórmula de los tres puntos respecto del punto medio,** para aproximar $f'(x_0)$:
$$
f'(x_0)=  \frac{1}{2h}(f(x_0+h)-f(x_0-h))-\frac{h^2}{6}f'''(\xi_1),
$$
donde $\xi_1\in <x_0-h,x_0+h>$, en realidad, cuando evaluamos la aproximación, $\frac{1}{2h}(f(x_0+h)-f(x_0-h))$, hacemos lo siguiente:
$$
\frac{1}{2h}(\tilde{f}(x_0+h)-\tilde{f}(x_0-h)),
$$
donde $\tilde{f}(x_0+h)$, $\tilde{f}(x_0-h)$ son aproximaciones de $f(x_0+h)$ y $f(x_0-h)$, respectivamente.

## Inestabilidad del error de redondeo
Por tanto, podemos escribir,
$$
\tilde{f}(x_0+h)=f(x_0+h)+e(x_0+h),\quad \tilde{f}(x_0-h)=f(x_0-h)+e(x_0-h),
$$
donde $e(x_0+h)$ y $e(x_0-h)$ son los **errores de redondeo** que cometemos al **evaluar** $f(x_0+h)$ y $f(x_0-h)$, respectivamente.

## Error global cometido
El **error global** cometido en la **fórmula de los tres puntos respecto del punto medio** seria el siguiente:
$$
\begin{align*}
& f'(x_0)-\frac{1}{2h}(\tilde{f}(x_0+h)-\tilde{f}(x_0-h))=\frac{1}{2h}(e(x_0+h)+e(x_0-h))\\ & -\frac{h^2}{6}f'''(\xi_1)
\end{align*}
$$
Sea $\epsilon >0$ el error máximo cometido al evaluar $f(x_0\pm h)$ con $|h|\leq h_0$ para un cierto $h_0$.

Sea $M=\max_{x\in (x_0-h_0,x_0+h_0)}|f'''(x)|$.

Podemos acotar el **error global** por:
$$
E=\frac{1}{2h}\cdot 2\epsilon +\frac{h^2}{6}\cdot M=\frac{\epsilon}{h}+\frac{M h^2}{6}.
$$

## Error global cometido
A partir de la expresión anterior, vemos que no tiene sentido considerar $h$ arbitrariamente pequeños ya que, aunque el término $\frac{M h^2}{6}$ tiende a cero, el término $\frac{\epsilon}{h}$ se hace arbitrariamente grande.

Si tuviésemos conocimiento del comportamiento de la tercera derivada, podríamos calcular el $h$ óptimo para el cual el error sería mínimo pero como trabajamos con cotas, dicho $h$ óptimo sería aproximado y nunca lo podríamos hallar.


# Extrapolación de Richardson

## Introducción
Vamos a aprender una técnica que nos permite transformar aproximaciones numéricas de algoritmos con **errores de truncamiento** relativamente bajos, es decir, de orden $O(h^k)$ con $k$ pequeños en algoritmos con **errores de truncamiento** altos, es decir, de orden $O(h^\hat{k})$ con $\hat{k}>k$.

Dicha técnica se denomina **extrapolación de Richardson**. Veamos cómo funciona.

Sea $F(h)$ un algoritmo que nos da una aproximación numérica de cierta constante $C$ con **error de truncamiento** $h$. Es decir:
$$
F(h)=C+k_1 h+k_2 h^2+k_3 h^3+\cdots,
$$
donde $k_1,k_2,k_3,\ldots$ son constantes en principio desconocidas donde sí sabemos que $k_1\neq 0$ ya que como hemos indicado el algoritmo tiene **error de truncamiento** $O(h)$.

## Introducción
Si aplicamos el algoritmo a $\frac{h}{2}$, obtenemos:
$$
F\left(\frac{h}{2}\right)=C+k_1\cdot\frac{h}{2}+k_2\cdot\frac{h^2}{4}+k_3\cdot\frac{h^3}{8}+\cdots,
$$
Consideremos ahora $2F\left(\frac{h}{2}\right)-F(h)$ como nueva aproximación de la constante $C$,
$$
2F\left(\frac{h}{2}\right)-F(h)=C-k_2\cdot\frac{h^2}{2}-k_3\cdot \frac{3h^3}{4}+\cdots
$$
Vemos que el "nuevo" algoritmo $2F\left(\frac{h}{2}\right)-F(h)$ tiene **error de truncamiento** $O(h^2)$ y por tanto es mejor que el algoritmo inicial $F(h)$ de cara a aproximar la constante $C$.

## Descripción del algoritmo
Sea 
$$
\begin{align*}
F_0(h)= &F(h)=C+k_1\cdot h+\cdots,\\ F_1(h)= & 2F\left(\frac{h}{2}\right)-F(h)=C-k_2\cdot \frac{h^2}{2}+\cdots
\end{align*}
$$
En general, escribimos $F_n(h)=C+k_{n+1}\frac{p_n}{q_n} h^{n+1}+\cdots$, donde $k_{n+1}\frac{p_n}{q_n}$ representaría el coeficiente de término principal. 

Para calcular $F_{n+1}(h)$, tenemos que eliminar el término $k_{n+1}\frac{p_n}{q_n} h^{n+1}$. Por tanto, hacemos lo siguiente:
$$
2^{n+1}F_n\left(\frac{h}{2}\right)-F_n(h)=(2^{n+1}-1)C+k_{n+2}\frac{\tilde{p}_{n+1}}{\tilde{q}_{n+1}}h^{n+2}+\cdots
$$

## Descripción del algoritmo
Por tanto,
$$
F_{n+1}(h)=\frac{2^{n+1}F_n\left(\frac{h}{2}\right)-F_n(h)}{2^{n+1}-1}=C+k_{n+2}\frac{p_{n+1}}{q_{n+1}}h^{n+2}+\cdots
$$

Para calcular aproximaciones de $C$ con **errores de truncamiento** cada vez más pequeños, hacemos una tabla como la que sigue:


## Descripción del algoritmo

<div class="center">
|$O(h)$|$O(h^2)$|$O(h^3)$|$O(h^4)$|
|:---|:---|:---|:---|
$F_0(h)$||||
$F_0\left(\frac{h}{2}\right)$|$F_1(h)=2F_0\left(\frac{h}{2}\right)-F_0(h)$|||
$F_0\left(\frac{h}{4}\right)$|$F_1\left(\frac{h}{2}\right)=2F_0\left(\frac{h}{4}\right)-F_0\left(\frac{h}{2}\right)$|$F_2(h)=\frac{2^2 F_1\left(\frac{h}{2}\right)-F_1(h)}{2^2-1}$||
$F_0\left(\frac{h}{8}\right)$|$F_1\left(\frac{h}{4}\right)=2F_0\left(\frac{h}{8}\right)-F_0\left(\frac{h}{4}\right)$|$F_2\left(\frac{h}{2}\right)=\frac{2^2 F_1\left(\frac{h}{4}\right)-F_1\left(\frac{h}{2}\right)}{2^2-1}$|$F_3(h)=\frac{2^3 F_2\left(\frac{h}{2}\right)-F_2(h)}{2^3-1}$|
</div>



## Ejemplo
<div class="example">
Consideremos la función $f(x)=\frac{1}{1+x^2}$. Vamos a hallar una aproximación de $f'(1)$ usando **extrapolación de Richardson**.

Usaremos como aproximación de $f'(x_0)$ el cociente incremental $F_0(h)=\frac{f(x_0+h)-f(x_0)}{h}$ que tiene error de truncamiento $O(h)$, ya que si consideramos el desarrollo de Taylor de $f(x_0+h)$, obtenemos:
$$
F_0(h)=\frac{f(x_0)+hf'(x_0)+\frac{h^2}{2}f''(x_0)+\cdots -f(x_0)}{h}=f'(x_0)+\frac{h}{2}f''(x_0)+\cdots
$$

Consideramos, pues, $x_0=1$ y $h=0.1$, la tabla de las aproximaciones de la **extrapolación de Richardson** vale:
</div>

## Ejemplo
<div class="example">

<div class="center">
|$O(h)$|$O(h^2)$|$O(h^3)$|$O(h^4)$|
|:---|:---|:---|:---|
$F_0(h)=-0.4751131$||||
$F_0\left(\frac{h}{2}\right)=-0.4875149$|$F_1(h)=-0.4999166$|||
$F_0\left(\frac{h}{4}\right)=-0.4937519$|$F_1\left(\frac{h}{2}\right)=-0.4999889$|$F_2(h)=-0.5000131$||
$F_0\left(\frac{h}{8}\right)=-0.4968752$|$F_1\left(\frac{h}{4}\right)=-0.4999986$|$F_2\left(\frac{h}{2}\right)=-0.5000018$|$F_3(h)=-0.5000002$|
</div>

</div>

## Ejemplo
<div class="example">
El valor exacto de $f'(1)$ vale $-0.5$ ya que:
$$
f'(x)=\frac{-2x}{(1+x^2)^2},\ \Rightarrow f'(1)=\frac{-2}{(1+1)^2}=-0.5.
$$
Los errores cometidos en las diferentes aproximaciones $F_i(h)$, con $i=0,1,2,3$ valen:
$$
0.0248869, 0.0000834, 0.0000131, 0.0000002.
$$
Observamos que cada vez los errores son más pequeños, lo que indica que las aproximaciones $F_i(h)$ son cada vez mejores a medida que $i$ aumenta.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=Au8qEn5zHe3D)
</div>
</div>

## Caso en que sólo aparecen exponentes pares
Supongamos que la aproximación $F(h)$ para calcular $C$ tiene una expresión donde sólo aparecen **exponentes pares** en el término del **error**:
$$
F_0(h)=F(h)=C+k_1 h^2+k_2 h^4+\cdots +h_n k^{2n}+\cdots
$$
En este caso, las aproximaciones sucesivas usando la **extrapolación de Richardson** son las siguientes:
$$
F_{n+1}(h)=\frac{4^{n+1}F_n\left(\frac{h}{2}\right)-F_n(h)}{4^{n+1}-1}=C+k_{n+2}\frac{p_{n+1}}{q_{n+1}}h^{2(n+2)}+\cdots
$$

<div class="exercise">
**Ejercicio**

Demostrar la expresión anterior.
</div>

La tabla de las aproximaciones sería en este caso:

## Caso en que sólo aparecen exponentes pares
<div class="center">
|$O(h^2)$|$O(h^4)$|$O(h^6)$|$O(h^8)$|
|:---|:---|:---|:---|
$F_0(h)$||||
$F_0\left(\frac{h}{2}\right)$|$F_1(h)=\frac{4F_0\left(\frac{h}{2}\right)-F_0(h)}{4-1}$|||
$F_0\left(\frac{h}{4}\right)$|$F_1\left(\frac{h}{2}\right)=\frac{4F_0\left(\frac{h}{4}\right)-F_0\left(\frac{h}{2}\right)}{4-1}$|$F_2(h)=\frac{4^2 F_1\left(\frac{h}{2}\right)-F_1(h)}{4^2-1}$||
$F_0\left(\frac{h}{8}\right)$|$F_1\left(\frac{h}{4}\right)=\frac{4F_0\left(\frac{h}{8}\right)-F_0\left(\frac{h}{4}\right)}{4-1}$|$F_2\left(\frac{h}{2}\right)=\frac{4^2 F_1\left(\frac{h}{4}\right)-F_1\left(\frac{h}{2}\right)}{4^2-1}$|$F_3(h)=\frac{4^3 F_2\left(\frac{h}{2}\right)-F_2(h)}{4^3-1}$|
</div>


## Ejemplo anterior
<div class="example">
Recordemos que la función era $f(x)=\frac{1}{1+x^2}$.
Vamos a hallar una aproximación de $f'(1)$ usando **extrapolación de Richardson** pero usando como aproximación de $f'(x_0)$ la fórmula de los tres puntos: $F_0(h)=\frac{f(x_0+h)-f(x_0-h)}{2h}$

En el término del error de dicha fórmula sólo aparecen exponentes pares $O(h^{2k})$ ya que si consideramos el desarrollo de Taylor de $f(x_0+h)$ y $f(x_0-h)$, obtenemos:
$$
\begin{align*}
F_0(h)= & \frac{1}{2h}\Biggl(f(x_0)+hf'(x_0)+\frac{h^2}{2}f''(x_0)+\frac{h^3}{6}f'''(x_0)+\cdots \\ & -(f(x_0) -hf'(x_0)+\frac{h^2}{2}f''(x_0)-\frac{h^3}{6}f'''(x_0)+\cdots)\Biggr)\\ = & f'(x_0)+\frac{h^2}{3}f'''(x_0)+\cdots
\end{align*}
$$
Dejamos los detalles de ver que sólo aparecen términos pares en la parte del error como ejercicio.

Si consideramos $x_0=1$ y $h=0.1$, la tabla de las aproximaciones de la **extrapolación de Richardson** vale:
</div>



## Ejemplo
<div class="example">

<div class="center">
|$O(h^2)$|$O(h^4)$|$O(h^6)$|$O(h^8)$|
|:---|:---|:---|:---|
$F_0(h)=-0.4999875$||||
$F_0\left(\frac{h}{2}\right)=-0.4999992$|$F_1(h)=-0.5000031$|||
$F_0\left(\frac{h}{4}\right)=-0.5$|$F_1\left(\frac{h}{2}\right)=-0.5000002$|$F_2(h)=-0.5$||
$F_0\left(\frac{h}{8}\right)=-0.5$|$F_1\left(\frac{h}{4}\right)=-0.5$|$F_2\left(\frac{h}{2}\right)=-0.5$|$F_3(h)=-0.5$|
</div>
En este caso, al usar una aproximación $F_0(h)$ mejor vemos que la columna correspondiente a un error $O(h^6)$ prácticamente no tiene error. Por tanto, bastaría con llegar hasta la tercera columna.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=glfuRRbdISGs)
</div>
</div>


# Integración numérica

## Introducción
En esta sección vamos a dar fórmulas aproximadas para calcular una integral definida $\int_a^b f(x)\,dx$, donde $f(x)$ es una función que pertenece a ${\cal C}^{k}[a,b]$, es decir de clase ${\cal C}^k$ para un cierto $k$ y un intervalo $[a,b]$.

Dichas fórmulas se denominan fórmulas de **cuadratura numérica**. Dichas fórmulas son expresiones del tipo:
$$
\int_a^b f(x)\, dx\approx\sum_{i=0}^n a_i\cdot f(x_i),
$$
donde $a_i$ son unos coeficientes a determinar y $x_i$ son unos nodos a determinar normalmente en el intervalo $[a,b]$.

## Fórmulas de cuadratura numérica
Las fórmulas de **cuadratura numérica** están basadas en el **polinomio de interpolación** en un conjunto de nodos $x_0,x_1,\ldots,x_n\in [a,b]$. 

Por tanto, si escribimos el **polinomio de interpolación** en función de los **polinomios de Lagrange** y usando la **fórmula del error en la interpolación**, podemos escribir:
$$
\begin{align*}
& \int_a^b f(x)\, dx=  \int_a^b \sum_{i=0}^n f(x_i)L_i(x)\, dx+\int_a^b\prod_{i=0}^n (x-x_i)\frac{f^{(n+1)}(\xi(x))}{(n+1)!}\, dx \\ & = \sum_{i=0}^n f(x_i)\int_a^b L_i(x)\,dx+\frac{1}{(n+1)!}\int_a^b \prod_{i=0}^n (x-x_i)f^{(n+1)}(\xi(x))\,dx.
\end{align*}
$$

## Fórmulas de cuadratura numérica
Los coeficientes $a_i$ serían:
$$
a_i=\int_a^b L_i(x)\,dx,
$$
y el error cometido sería:
$$
E(f,a,b)=\frac{1}{(n+1)!}\int_a^b \prod_{i=0}^n (x-x_i)f^{(n+1)}(\xi(x))\,dx.
$$

## La regla del trapecio
Consideremos el caso más simple: $n=1$, $x_0=a$ y $x_1=b$, en este caso aproximamos la integral $\int_a^b f(x)\, dx$ por el área del trapecio de vértices $(a,0)$, $(a,f(a))$, $(b,0)$ y $(b,f(b))$:

![](05DerivacionIntegracion_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## La regla del trapecio
En este caso, el polinomio de interpolación en los nodos $(x_0,f(x_0))$ y $(x_1,f(x_1))$ vale:
$$
P_1(x)=f(x_0)\frac{x-x_1}{(x_0-x_1)}+f(x_1)\frac{x-x_0}{x_1-x_0}.
$$
La **cuadratura** sería:

## La regla del trapecio
$$
\begin{align*}
\int_a^b f(x)\, dx= & f(x_0)\int_{x_0}^{x_1}\frac{x-x_1}{(x_0-x_1)}\, dx+f(x_1)\int_{x_0}^{x_1}\frac{x-x_0}{x_1-x_0}\, dx\\ & +\frac{1}{2}\int_{x_0}^{x_1}f''(\xi(x))(x-x_0)(x-x_1)\, dx\\ = & f(x_0)\left[\frac{(x-x_1)^2}{2(x_0-x_1)}\right]_{x_0}^{x_1}+f(x_1)\left[\frac{(x-x_0)^2}{2(x_1-x_0)}\right]_{x_0}^{x_1}+E(f,a,b)\\ = & -\frac{1}{2}f(x_0)(x_0-x_1)+\frac{1}{2}f(x_1)(x_1-x_0)+E(f,a,b)\\ = & \frac{1}{2}(x_1-x_0)(f(x_0)+f(x_1))+E(f,a,b)\\ = & \frac{1}{2}h(f(x_0)+f(x_1))+E(f,a,b),
\end{align*}
$$
donde $h=x_1-x_0$.

## La regla del trapecio
Analicemos el término del error:
$$
E(f,a,b)=\frac{1}{2}\int_{x_0}^{x_1}f''(\xi(x))(x-x_0)(x-x_1)\, dx.
$$
Teniendo en cuenta que $(x-x_0)(x-x_1)$ no cambia de signo en el intervalo $[x_0,x_1]=[a,b]$ y usando el **Teorema del valor medio para integrales**, podemos escribir que existe un $\xi\in (a,b)$ tal que:

## La regla del trapecio
$$
\begin{align*}
E(f,a,b)=&\frac{f''(\xi)}{2}\int_{x_0}^{x_1}(x-x_0)(x-x_1)\, dx\\ = &
\frac{f''(\xi)}{2}\int_{x_0}^{x_1}(x-x_0)(x-x_0-h)\, dx\\ = & \frac{f''(\xi)}{2}\int_{x_0}^{x_1}(x-x_0)^2-h (x-x_0)\, dx\\ = & \frac{f''(\xi)}{2}\left[\frac{(x-x_0)^3}{3}-h\frac{(x-x_0)^2}{2}\right]_{x_0}^{x_1}=\frac{f''(\xi)}{2}\left(\frac{h^3}{3}-\frac{h^3}{2}\right)\\ = & -\frac{h^3}{12}f''(\xi).
\end{align*}
$$

## La regla del trapecio
En resumen:
$$
\int_a^b f(x)\, dx=\frac{h}{2}(f(a)+f(b))-\frac{h^3}{12}f''(\xi),
$$
donde $h=b-a$ y $\xi\in (a,b)$.

## Ejemplo
<div class="example">

Consideremos la función $f(x)=\sqrt{1+x^2}$, para $x\in [0,2]$. Vamos a aproximar $\int_0^2 f(x)\, dx$ usando la regla del trapecio:
$$
\int_0^2\sqrt{1+x^2}\, dx =\frac{2}{2}(f(0)+f(2))-\frac{2^3}{12}f''(\xi)=\frac{2}{2}(1+\sqrt{5})-\frac{2}{3}f''(\xi)\approx 3.236068-\frac{2}{3}f''(\xi),
$$
donde $\xi\in (0,2)$.

El valor exacto de la integral anterior es: $\sqrt{5}+\frac{1}{2} \ln
   \left(2+\sqrt{5}\right)\approx 2.9578857.$
   
La aproximación es muy mala debido a que el intervalo de integración tiene amplitud grande ($2$) y la regla del trapecio es la más sencilla y con más error.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=YeZdnPhhJIBj)
</div>
</div>

## Regla de Simpson
Consideremos ahora $n=2$, $x_0=a$, $x_1=\frac{a+b}{2}$ y $x_2=b$. 

Si definimos $h$ como $h=\frac{b-a}{2}$, podemos escribir los nodos como $x_0=a$, $x_1=a+h$ y $x_2=b$.

En este caso aproximamos la integral $\int_a^b f(x)\, dx$ por el área de la parábola que pasa por los puntos $(a,f(a))$, $(x_1,f(x_1))$ y $(b,f(b))$:


![](05DerivacionIntegracion_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## La regla de Simpson
Para deducir la regla de Simpson, en lugar de interpolar, vamos a considerar el desarrollo por **Taylor** de la función $f$ alrededor de $x=x_1$ ya que de esta forma se consigue una expresión del error de orden menor:
$$
\begin{align*}
f(x)=& f(x_1)+f'(x_1)(x-x_1)+\frac{1}{2}f''(x_1)(x-x_1)^2+\frac{1}{6}f'''(x_1)(x-x_1)^3\\ & +\frac{1}{24}f^{(4)}(\xi(x))(x-x_1)^4.
\end{align*}
$$

## La regla de Simpson

La cuadratura sería:
$$
\begin{align*}
\int_a^b f(x)\, dx= & f(x_1)(b-a)+f'(x_1)\int_a^b (x-x_1)\, dx\\ & +\frac{1}{2}f''(x_1)\int_a^b (x-x_1)^2\, dx+\frac{1}{6}f'''(x_1)\int_a^b (x-x_1)^3\, dx\\ & +\frac{1}{24} \int_a^b f^{(4)}(\xi(x))(x-x_1)^4\, dx 
\end{align*}
$$


## La regla de Simpson

La cuadratura sería:
$$
\begin{align*}
 = & f(x_1)(b-a)+f'(x_1)\left[\frac{(x-x_1)^2}{2}\right]_{a}^b\\ & + \frac{1}{2}f''(x_1)\left[\frac{(x-x_1)^3}{3}\right]_a^b +\frac{1}{6}f'''(x_1)\left[\frac{(x-x_1)^4}{4}\right]_a^b+E(f,a,b)\\ = & 
 2hf(x_1)+\frac{h^3}{3}f''(x_1)+E(f,a,b)
\end{align*}
$$

## La regla de Simpson
<div class="exercise">
**Ejercicio**

Sea $f\in {\cal C}^4[a,b]$ y $x_1\in (a,b)$ tal que $x_1-h,x_1+h\in [a,b]$. Deducir la fórmula siguiente para aproximar $f''(x_1)$
$$
f''(x_1)=\frac{f(x_1+h)-2f(x_1)+f(x_1-h)}{h^2}-\frac{h^2}{12}f^{(4)}(\xi_1).
$$
Indicación: considerar los desarrollos de Taylor de las funciones $f(x_1\pm h)$, eliminar los coeficientes de $h^0,h^1$ y $h^3$ y arreglando el término del error obtendréis la fórmula pedida.

</div>


## La regla de Simpson
Teniendo en cuenta que $x_0=x_1-h$ y $x_2=x_1+h$, podemos usar la fórmula del ejercicio en la expresión de la cuadratura obteniendo:
$$
\begin{align*}
\int_a^b f(x)\, dx = &
 2hf(x_1)\\ & +\frac{h^3}{3}\left(\frac{f(x_1+h)-2f(x_1)+f(x_1-h)}{h^2}-\frac{h^2}{12}f^{(4)}(\xi_1)\right) \\ & +E(f,a,b) \\ = &
 \frac{h}{3} (f(x_1+h)+4 f(x_1)+f(x_1-h))-\frac{h^5}{36}f^{(4)}(\xi_1)\\ & +E(f,a,b)\\ = & \frac{h}{3}\left(f(a)+4f\left(\frac{a+b}{2}\right)+f(b)\right)-\frac{h^5}{36}f^{(4)}(\xi_1)\\ & +E(f,a,b)
\end{align*}
$$


## La regla de Simpson
Analicemos el error cometido:
$$
E(f,a,b)=\frac{1}{24} \int_a^b f^{(4)}(\xi(x))(x-x_1)^4\, dx.
$$

Teniendo en cuenta que $(x-x_1)^4$ no cambia de signo en el intervalo $[a,b]$ y usando el **Teorema del valor medio para integrales**, podemos escribir que existe un $\xi_2\in (a,b)$ tal que:
$$
\begin{align*}
E(f,a,b)=&\frac{f^{(4)}(\xi_2)}{24}\int_a^b (x-x_1)^4\, dx=\frac{f^{(4)}(\xi_2)}{24}\left[\frac{(x-x_1)^5}{5}\right]_a^b \\ = & \frac{f^{(4)}(\xi_2)}{60}h^5.
\end{align*}
$$

## La regla de Simpson
En resumen:
$$
\begin{align*}
\int_a^b f(x)\, dx= &  \frac{h}{3} (f(x_1+h)+4 f(x_1)+f(x_1-h))\\ & +\frac{h^5}{12}\left(\frac{f^{(4)}(\xi_2)}{5}-\frac{f^{(4)}(\xi_1)}{3}\right),
\end{align*}
$$
donde $h=\frac{b-a}{2}$.

Intentando deducir la **regla de Simpson** como aquella regla para la cual la integral es exacta para polinomios de grado menor o igual que $3$, puede deducirse que existe un valor $\xi$ tal que:
$$
\frac{f^{(4)}(\xi_2)}{5}-\frac{f^{(4)}(\xi_1)}{3}=-\frac{2 f^{(4)}(\xi)}{15}.
$$

## La regla de Simpson
La regla de Simpson sería, pues:
$$
\int_a^b f(x)\, dx=   \frac{h}{3} (f(x_1+h)+4 f(x_1)+f(x_1-h))-\frac{h^5}{90}f^{(4)}(\xi).
$$

<l class="observ">
**Observación**

Recordemos que el error en el **método de los trapecios** era de orden $O(h^3)$.

Por tanto, en la **regla de Simpson** el orden del error esperable era $O(h^4)$ ya que añadimos un  nodo más en la interpolación pero el error nos ha salido de orden mucho menor, $O(h^5)$. Por dicha razón, es uno de los métodos de integración numérica más usados.

## Ejemplo anterior
<div class="example">
**Ejemplo**

Recordemos que considerábamos la función $f(x)=\sqrt{1+x^2}$, para $x\in [0,2]$. Vamos a aproximar $\int_0^2 f(x)\, dx$ usando la regla de Simpson donde el valor de $h$ será: $h=\frac{2-0}{2}=1$:
$$
\begin{align*}
\int_0^2\sqrt{1+x^2}\, dx = & \frac{1}{3}(f(0)+4f(1)+f(2))-\frac{1}{90}f^{(4)}(\xi)=\frac{1}{3}(1+4\sqrt{2}+\sqrt{5})-\frac{1}{90}f^{(4)}(\xi)\\ \approx & 2.9643074-\frac{1}{90}f^{(4)}(\xi),
\end{align*}
$$
donde $\xi\in (0,2)$.

El valor exacto de la integral anterior es: $\sqrt{5}+\frac{1}{2} \ln
   \left(2+\sqrt{5}\right)\approx 2.9578857.$

La aproximación es mucho mejor que en la regla de los trapecios ya que tenemos un error de sólo $0.0064217$.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=D3RoZNdBB3jO)
</div>
</div>

# Precisión de una cuadratura

## Introducción
<l class="definition">Definición de precisión o grado de exactitud de una cuadratura.</l>

Diremos que una **cuadratura** tiene precisión o grado de exactitud $k$ si la fórmula es exacta para todos los polinomios de grado menor o igual que $k$.

<l class="observ">Observación.</l>

Las fórmulas de **cuadratura** son lineales para las funciones, es decir, dadas dos funciones $f$ y $g$ y dados dos números $\lambda$ y $\mu$, entonces si llamamos $\displaystyle C(f)=\sum_{i=0}^n a_i f(x_i)$ a la cuadratura para calcular $\displaystyle\int_a^b f(x)\, dx$, se cumple:
$$
C(\lambda f+\mu g)=\lambda\cdot C(f)+\mu\cdot C(g).
$$

## Precisión de una cuadratura
Usando la observación anterior, para comprobar que una **cuadratura** tiene precisión $k$, basta ver que es exacta para los monomios $x^i$, para $i=0,1,\ldots,k$.

<l class="prop">Proposición. Precisión de la fórmula de los trapecios y de la fórmula de Simpson.</l>

La fórmula de los trapecios tiene precisión $k=1$ y la fórmula de Simpson, precisión $k=3$.

<div class="dem">
**Demostración**

Veamos que la fórmula de los trapecios $T(f)=\frac{b-a}{2}(f(b)+f(a))$ es exacta para los monomios $1$ y $x$:
$$
\begin{align*}
\int_a^b 1\, dx=& b-a=T(1)=\frac{b-a}{2}(1+1),\\
\int_a^b x\, dx=& \left[\frac{x^2}{2}\right]_a^b =\frac{1}{2}(b^2-a^2)=T(x)=\frac{(b-a)}{2}(b+a).
\end{align*}
$$
</div>

## Precisión de una cuadratura
<div class="dem">
**Demostración** (continuación)

Veamos que la fórmula de Simpson $S(f)=\frac{b-a}{6}\left(f(a)+4f\left(\frac{a+b}{2}\right)+f(b)\right)$ es exacta para los monomios $1$, $x$, $x^2$ y $x^3$:
$$
\begin{align*}
\int_a^b 1\, dx=& b-a=S(1)=\frac{b-a}{6}(1+4+1),\\
\int_a^b x\, dx=& \left[\frac{x^2}{2}\right]_a^b =\frac{1}{2}(b^2-a^2)=S(x)=\frac{(b-a)}{6}\left(a+\frac{4(a+b)}{2}+b\right)\\ =& \frac{(b-a)}{6}(a+2a+2b+b)=\frac{(b-a)}{6} 3(a+b)=\frac{1}{2}(b-a)(a+b)=\frac{1}{2}(b^2-a^2),\\
\int_a^b x^2\, dx=& \left[\frac{x^3}{3}\right]_a^b =\frac{1}{3}(b^3-a^3)=S(x^2)=\frac{(b-a)}{6}\left(a^2+\frac{4(a+b)^2}{4}+b^2\right)\\ =& \frac{(b-a)}{6}(a^2+(a+b)^2+b^2)=\frac{(b-a)}{6} (2a^2+2ab+2b^2)\\ = & \frac{1}{3}(b-a)(a^2+ab+b^2)=\frac{1}{3}(b^3-a^3),\\
\end{align*}
$$

</div>

## Precisión de una cuadratura
<div class="dem">
**Demostración** (continuación)
$$
\begin{align*}
\int_a^b x^3\, dx=& \left[\frac{x^4}{4}\right]_a^b =\frac{1}{4}(b^4-a^4)=S(x^3)=\frac{(b-a)}{6}\left(a^3+\frac{4(a+b)^3}{8}+b^3\right)\\ =& \frac{(b-a)}{6}\left(a^3+\frac{1}{2}(a+b)^3+b^3\right)=\frac{(b-a)}{6}\cdot \frac{3}{2} \cdot (a^3+a^2b+ab^2+b^3)\\ = & \frac{1}{4}(b-a)(a^3+a^2b+ba^2+b^3)=\frac{1}{4}(b^4-a^4).
\end{align*}
$$
</div>

# Fórmulas de Newton-Cotes

## Fórmulas de Newton-Cotes cerradas
Vamos a generalizar el método de los trapecios o la fórmula de Simpson suponiendo que en lugar de considerar $2$ puntos (trapecios) o $3$ (Simpson), consideramos $n+1$ puntos equiespaciados en el intervalo $[a,b]$:
$$
x_i=a+i\cdot h, \ i=0,\ldots,n,\ h=\frac{b-a}{n},
$$
e interpolamos para hallar la **cuadratura** correspondiente:
$$
\int_a^b f(x)\, dx\approx \sum_{i=0}^n a_i f(x_i).
$$

Dichas cuadraturas se llaman **fórmulas de Newton-Cotes** cerradas porque incluyen como nodos los extremos $a$ y $b$ ya que $x_0=a$ y $x_n=b$.

## Fórmulas de Newton-Cotes cerradas
<l class="prop">Teorema.</l>

Consideramos la cuadratura $\displaystyle\sum_{i=0}^n a_i f(x_i)$ como la **fórmula de Newton-Cotes cerrada** considerando $n+1$ puntos equiespaciados en $[a,b]$, con $x_0=a$, $x_n=b$, $h=\frac{b-a}{n}$. Entonces:

* Si $n$ es par y $f\in {\cal C}^{n+2}[a,b]$, existe un $\xi\in (a,b)$ tal que
$$
\int_a^b f(x)\, dx=\sum_{i=0}^n a_i f(x_i)+\frac{h^{n+3}f^{(n+2)}(\xi)}{(n+2)!}\int_0^n t^2(t-1)\cdots (t-n)\,dt.
$$

## Fórmulas de Newton-Cotes cerradas
<l class="prop">Teorema. (continuación) </l>

* Si $n$ es impar y $f\in {\cal C}^{n+1}[a,b]$, existe un $\xi\in (a,b)$ tal que
$$
\int_a^b f(x)\, dx=\sum_{i=0}^n a_i f(x_i)+\frac{h^{n+2}f^{(n+1)}(\xi)}{(n+1)!}\int_0^n t(t-1)\cdots (t-n)\,dt.
$$

## Fórmulas de Newton-Cotes cerradas

<l class="observ">Observación.</l>
Los coeficientes $a_i$ en las **fórmulas de Newton-Cotes** se pueden escribir de la siguiente manera:
$$
a_i=h\int_0^n \prod_{j=0,j\neq i}^n \frac{t-j}{i-j}\, dt,
$$
ya que ($L_i(x)$ son los polinomios de Lagrange) si hacemos el cambio de variable $x=a+t h$, tenemos:
$$
\begin{align*}
a_i= & \int_a^b L_i(x)\, dx=\int_a^b  \prod_{j=0,j\neq i}^n \frac{x-x_j}{x_i-x_j}\, dx\\ = & h\int_0^n\prod_{j=0,j\neq i}^n \frac{a+th-a-jh}{a+ih-a-jh}\, dt=h\int_0^n \prod_{j=0,j\neq i}^n \frac{t-j}{i-j}\, dt.
\end{align*}
$$

## Fórmulas de Newton-Cotes cerradas
En resumen, llamando $\displaystyle\alpha_{i,n}=\int_0^n \prod_{j=0,j\neq i}^n \frac{t-j}{i-j}\, dt$ las **fórmulas de Newton-Cotes** serían:
$$
\int_a^b f(x)\, dx=h \sum_{i=0}^n  \alpha_{i,n} f(x_i)+E(f,a,b),
$$
donde $E(f,a,b)$ es el término del error indicado en el Teorema anterior.

## Fórmulas de Newton-Cotes cerradas
Los valores $\alpha_{i,n}$ para $n=1,2,3,4,5$ son los siguientes:

<div class="center">
|$n$|$\alpha_{0,n}$|$\alpha_{1,n}$|$\alpha_{2,n}$|$\alpha_{3,n}$|$\alpha_{4,n}$|$\alpha_{5,n}$|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|$1$|$\frac{1}{2}$|$\frac{1}{2}$|||||
|$2$|$\frac{1}{3}$|$\frac{4}{3}$|$\frac{1}{3}$||||
|$3$|$\frac{3}{8}$|$\frac{9}{8}$|$\frac{9}{8}$|$\frac{3}{8}$|||
|$4$|$\frac{14}{45}$|$\frac{64}{45}$|$\frac{8}{15}$|$\frac{64}{45}$|$\frac{14}{45}$||
|$5$|$\frac{95}{288}$|$\frac{125}{96}$|$\frac{125}{144}$|$\frac{125}{144}$|$\frac{125}{96}$|$\frac{95}{288}$|
</div>

## Fórmulas de Newton-Cotes cerradas
<l class="observ">
**Observaciones**

* Las dos primeras filas anteriores corresponden al **método de los trapecios** y a la **fórmula de Simpson**, respectivamente.
* Es mejor siempre considerar $n$ par ya que el término del error es de orden $h^{n+3}$, en cambio si $n$ es impar el término del error es de orden sólo $h^{n+2}$. Es decir, "ganamos" un $h$ extra tal como pasa con la **fórmula de Simpson**.

## Ejemplo anterior


<div class="example">

Recordemos que considerábamos la función $f(x)=\sqrt{1+x^2}$, para $x\in [0,2]$.

Vamos a aproximar $\int_0^2 f(x)\, dx$ usando la fórmula de Newton-Cotes cerradas con $n=4$, donde $h$ vale $h=\frac{2-0}{4}=0.5$ y los nodos son:
$$
x_0=0,\ x_1=0.5,\ x_2=1,\ x_3=1.5, \ x_4=2.
$$
$$
\begin{align*}
\int_0^2\sqrt{1+x^2}\, dx = & h(\alpha_{0,4}f(0)+\alpha_{1,4}f(0.5)+\alpha_{2,4}f(1)+\alpha_{3,4}f(1.5)+\alpha_{4,4}f(2))\\ & +\frac{h^7f^{(6)}(\xi)}{6!}\int_0^4 t^2(t-1)(t-2)(t-3)(t-4)\, dt \\ = &
0.5\cdot\left(\frac{14}{45}\cdot 1+\frac{64}{45}\cdot\frac{\sqrt{5}}{2}+\frac{8}{15}\cdot\sqrt{2}+\frac{64}{45}\cdot\frac{\sqrt{13}}{2}+\frac{14}{45}\cdot\sqrt{5}\right)\\ & +\frac{f^{(6)}(\xi)}{92160}\cdot \left(-\frac{128}{21}\right)=2.9575321-0.0000661\cdot f^{(6)}(\xi)
\end{align*}
$$
donde $\xi\in (0,2)$.


</div>

## Ejemplo anterior

<div class="example">

Recordemos que el valor exacto de la integral anterior era: $\sqrt{5}+\frac{1}{2} \ln
   \left(2+\sqrt{5}\right)\approx 2.9578857.$

El error cometido es de sólo $0.0003536$.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=8MvyUoRZJrCe)
</div>
</div>

## Fórmulas de Newton-Cotes abiertas
Vamos a hacer lo mismo que antes pero ahora no incluiremos los extremos del intervalo de integración $a$ y $b$ en los nodos equiespaciados de interpolación.

Los nodos serán pues $x_i=x_0+ih$, donde $i=0,\ldots,n$, $h=\frac{b-a}{n+2}$ y $x_0=a+h$:
$$
\begin{align*}
x_0 & =a+h,\ x_1=a+2h,\ldots, \\ x_n= & a+h+n h=a+h+\frac{(b-a)n}{n+2}=b-h.
\end{align*}
$$


## Fórmulas de Newton-Cotes abiertas

En este caso, etiquetamos los extremos del intervalo como $x_{-1}=a$, $x_{n+1}=b$ y la **cuadratura** será:
$$
\int_a^b f(x)\, dx=\int_{x_{-1}}^{x_{n+1}} f(x)\, dx \approx \sum_{i=0}^n a_i f(x_i),
$$
donde recordemos que los $a_i$ valen $\displaystyle a_i=\int_a^b L_i(x)\, dx$, donde $L_i(x)$ es el $i-èsimo$ polinomio de Lagrange respecto los nodos $x_0,x_1,\ldots, x_n$.


## Fórmulas de Newton-Cotes abiertas
<l class="prop">Teorema.</l>

Consideramos la cuadratura $\displaystyle\sum_{i=0}^n a_i f(x_i)$ como la **fórmula de Newton-Cotes abierta** considerando $n+1$ puntos equiespaciados en $[a,b]$, con $x_{-1}=a$, $x_{n+1}=b$, $h=\frac{b-a}{n+2}$. Entonces:

* Si $n$ es par y $f\in {\cal C}^{n+2}[a,b]$, existe un $\xi\in (a,b)$ tal que
$$
\int_a^b f(x)\, dx=\sum_{i=0}^n a_i f(x_i)+\frac{h^{n+3}f^{(n+2)}(\xi)}{(n+2)!}\int_{-1}^{n+1} t^2(t-1)\cdots (t-n)\,dt.
$$

## Fórmulas de Newton-Cotes abiertas
<l class="prop">Teorema. (continuación) </l>

* Si $n$ es impar y $f\in {\cal C}^{n+1}[a,b]$, existe un $\xi\in (a,b)$ tal que
$$
\int_a^b f(x)\, dx=\sum_{i=0}^n a_i f(x_i)+\frac{h^{n+2}f^{(n+1)}(\xi)}{(n+1)!}\int_{-1}^{n+1} t(t-1)\cdots (t-n)\,dt.
$$

<l class="observ">Observaciones.</l>

* Tenemos una expresión del error muy parecida a las **fórmulas de Newton-Cotes** cerradas, lo único que cambia en el término del error es que ahora integramos entre $-1$ y $n+1$ en vez de entre $0$ y $n$.
* Vemos que si $n$ es par la precisión vuelve a ser mejor que si $n$ es impar tal como pasaba en las **fórmulas de Newton-Cotes cerradas**.

## Fórmulas de Newton-Cotes abiertas
* Caso $n=0$, **fórmula del punto medio**. En este caso sólo tenemos un punto $x_0=\frac{a+b}{2}$ y $h=\frac{b-a}{2}$:
$$
\begin{align*}
\int_a^b f(x)\, dx= & 2hf(x_0)+\frac{h^3}{2}\int_{-1}^1 t^2\, dt=  2hf(x_0)+\frac{h^3}{2}\cdot\frac{2}{3}\cdot f''(\xi)\\ =  &  2hf(x_0)+\frac{h^3}{3}\cdot f''(\xi)
\end{align*}
$$
con $\xi\in (a,b)$.

## Fórmulas de Newton-Cotes abiertas
* $n=1$. En este caso, $h=\frac{b-a}{3}$, $x_0=a+h=\frac{2a+b}{3}$, $x_1=a+2h=\frac{a+2b}{3}$:
$$
\begin{align*}
& \int_a^b f(x)\, dx=  \frac{3h}{2}(f(x_0)+f(x_1))+\frac{h^3  f''(\xi)}{2}\int_{-1}^2 t(t-1)\, dt\\ = &
 \frac{3h}{2}(f(x_0)+f(x_1))+\frac{h^3  f''(\xi)}{2}\cdot \frac{3}{2}\\ = &
 \frac{3h}{2}(f(x_0)+f(x_1))+\frac{3h^3  f''(\xi)}{4}.
\end{align*}
$$
con $\xi\in (a,b)$.

## Fórmulas de Newton-Cotes abiertas
* $n=2$. En este caso, $h=\frac{b-a}{4}$, $x_0=a+h=\frac{3a+b}{4}$, $x_1=a+2h=\frac{a+b}{2}$, $x_2=a+3h=\frac{a+3b}{4}$:
$$
\begin{align*}
& \int_a^b f(x)\, dx=  \frac{4h}{3}(2f(x_0)-f(x_1)+2f(x_2))\\ & +\frac{h^5  f^{(4)}(\xi)}{4!}\int_{-1}^3 t^2(t-1)(t-2)\, dt\\ = &
 \frac{4h}{3}(2f(x_0)-f(x_1)+2f(x_2)) +\frac{h^5  f^{(4)}(\xi)}{24}\cdot \frac{112}{15}\\ = &
 \frac{4h}{3}(2f(x_0)-f(x_1)+2f(x_2)) +\frac{14h^5  f^{(4)}(\xi)}{45}.
\end{align*}
$$
con $\xi\in (a,b)$.

## Fórmulas de Newton-Cotes abiertas

<div class="exercise">
**Ejercicio** 

Deducir las fórmulas de Newton-Cotes abiertas para $n=0,1,2$ y deducir y desarrollar la fórmula para $n=3$ dando el valor de $h$, $x_0,x_1,x_2$ y $x_3$:
$$
\begin{align*}
\int_a^b f(x)\, dx = & \frac{5h}{24}(11 f(x_0)+f(x_1)+f(x_2)+11 f(x_3))+\frac{95}{144}h^5f^{(4)}(\xi),
\end{align*}
$$
con $\xi\in (a,b)$.

</div>

## Ejemplo anterior
<div class="example">
Recordemos que considerábamos la función $f(x)=\sqrt{1+x^2}$, para $x\in [0,2]$.

Vamos a aproximar $\int_0^2 f(x)\, dx$ usando la fórmula de Newton-Cotes abiertas con $n=2$, donde $h$ vale $h=\frac{2-0}{4}=0.5$ y los nodos son:
$$
x_0=0.5,\ x_1=1,\ x_2=1.5.
$$
$$
\begin{align*}
\int_0^2\sqrt{1+x^2}\, dx = & \frac{4h}{3}(2f(0.5)-f(1)+2f(1.5)) +\frac{h^5 f^{(4)}(\xi)}{4!}\int_{-1}^3 t^2(t-1)(t-2)\, dt \\ = &
\frac{2}{3}\cdot\left(2\frac{\sqrt{5}}{2}-\sqrt{2}+2\frac{\sqrt{13}}{2}\right) +\frac{f^{(4)}(\xi)}{768}\cdot \left(\frac{112}{15}\right)\\ = & 2.9516038+0.0097222\cdot f^{(4)}(\xi),
\end{align*}
$$
donde $\xi\in (0,2)$.

Recordemos que el valor exacto de la integral anterior era: $\sqrt{5}+\frac{1}{2} \ln
   \left(2+\sqrt{5}\right)\approx 2.9578857.$

El error cometido es de sólo $0.0062819$.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=8E-Y9IlTKSx6)
</div>
</div>

# Integración numérica compuesta

## Introducción
Las fórmulas de Newton-Cotes introducidas en la sección anterior tienen los siguientes inconvenientes:

* Se requiere mucho **cálculo** para obtener los **coeficientes** $a_i$ para valores de $n$ grandes.
* Están basadas en interpolar polinomios de **grado cada vez más alto** en **nodos equiespaciados**. Ya comentamos en el capítulo de interpolación que aproximar una función con polinomios de grado cada vez más alto en nodos equiespaciados no siempre produce los efectos deseados, es decir, un polinomio que tenga poco error en todos los valores del intervalo en cuestión. Recordar el **fenómeno de Runge**, por ejemplo.


## Introducción

Por los motivos anteriormente expuestos, la fórmulas de Newton-Cotes se suelen usar para valores de $n$ bajos.

Ahora bien, si el intervalo de integración tiene una longitud grande, tendremos un error grande ya que el valor de $h$ será también grande.

Para resolver este problema, dividiremos el **intervalo de integración en pequeños subintervalos** y aplicaremos la fórmula de Newton-Cotes a cada uno de los **subintervalos** usando que la integral sobre el intervalo grande es la suma de las integrales sobre cada uno de los subintervalos.

Dicha técnica se denomina **integración compuesta**.


## Ejemplo ilustrativo


<div class="example">
Consideremos la función logística $f(x)=\frac{1}{1+\mathrm{e}^{-3x}}$, para $x\in [-2,2]$. 
Queremos calcular:
$$
\int_{-2}^2 f(x)\, dx=\int_{-2}^2 \frac{1}{1+\mathrm{e}^{-3x}}\, dx.
$$

Consideremos el método de los trapecios donde en general para calcular $\displaystyle\int_c^d f(x)\, dx$ usábamos la fórmula:
$$
\int_c^d f(x)\, dx=\frac{h}{2}(f(c)+f(d))-\frac{h^3}{12}f''(\xi).
$$
Vamos a subdividir el intervalo de integración $[-2,2]$ en $n$ subintervalos de la misma longitud de la forma:
$$
[-2,2]=\bigcup_{i=0}^{n-1}\left[-2+i\cdot\frac{4}{n},-2+(i+1)\cdot\frac{4}{n}\right].
$$
Por ejemplo, para $n=5$ obtenemos los subintervalos siguientes:
$$
[-2,2]=[-2,-1.2]\cup [-1.2,-0.4]\cup [-0.4,0.4]\cup [0.4,1.2]\cup [1.2,2].
$$


</div>

## Ejemplo ilustrativo

<div class="example">
Entonces escribimos la integral a calcular de la forma siguiente:
$$
\int_{-2}^2 f(x)\, dx =\int_{-2}^{-1.2}f(x)\, dx+\int_{-1.2}^{-0.4}f(x)\, dx+\int_{-0.4}^{0.4}f(x)\, dx+\int_{0.4}^{1.2}f(x)\, dx+\int_{1.2}^{2}f(x)\, dx.
$$
A continuación aplicamos la fórmula de los trapecios a cada una de las integrales anteriores:
$$
\begin{align*}
\int_{-2}^2 f(x)\, dx= & \frac{h}{2}(f(-2)+f(-1.2))-\frac{h^3}{12}f''(\xi_1)+\frac{h}{2}(f(-1.2)+f(-0.4))-\frac{h^3}{12}f''(\xi_2)\\ & +\frac{h}{2}(f(-0.4)+f(0.4))-\frac{h^3}{12}f''(\xi_3)+\frac{h}{2}(f(0.4)+f(1.2))-\frac{h^3}{12}f''(\xi_4)\\ & +\frac{h}{2}(f(1.2)+f(2))-\frac{h^3}{12}f''(\xi_5)\\ = &
\frac{0.8}{2}(f(-2)+2f(-1.2)+2f(-0.4)+2f(0.4)+2f(1.2)+f(2))\\ & -\frac{0.8^3}{12}(f''(\xi_1)+f''(\xi_2)+f''(\xi_3)+f''(\xi_4)+f''(\xi_5)) \\ = & 0.4(f(-2)+2f(-1.2)+2f(-0.4)+2f(0.4)+2f(1.2)+f(2))\\ &
- 0.0426667(f''(\xi_1)+f''(\xi_2)+f''(\xi_3)+f''(\xi_4)+f''(\xi_5))
\end{align*}
$$

</div>

## Ejemplo ilustrativo

<div class="example">
Aplicando el **Teorema generalizado de Bolzano**, podemos decir que existe un valor $\xi\in [-2,2]$ tal que:
$$
5f''(\xi)=f''(\xi_1)+f''(\xi_2)+f''(\xi_3)+f''(\xi_4)+f''(\xi_5).
$$
En resumen, la expresión de la integral aproximada aplicando la **fórmula de los trapecios compuesta** es:
$$
\begin{align*}
\int_{-2}^{2} f(x)\, dx= & 0.4(f(-2)+2f(-1.2)+2f(-0.4)+2f(0.4)+2f(1.2)+f(2))
- 5\cdot 0.0426667 f''(\xi) \\ = & 0.4 (0.0024726+2\cdot 0.026597+2\cdot 0.2314752+2\cdot 0.7685248\\ & +2\cdot 0.973403+0.9975274)- 0.2133333 f''(\xi) = 2- 0.2133333 f''(\xi),
\end{align*}
$$
donde $\xi\in [-2,2]$.

El valor exacto de la integral es $2$. Observamos que la aproximación nos ha dado el valor exacto pero es debido a la forma de la función, ver la figura siguiente.

En general, obtendremos una aproximación de la integral.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=4ajKqODCKhY7)
</div>

</div>

## Ejemplo ilustrativo
![](05DerivacionIntegracion_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

## Fórmula de trapecios compuesta
Vamos a generalizar el ejemplo anterior.

Sea $f\in {\cal C}^2[a,b]$ de clase ${\cal C}^2$ en un cierto intervalo $[a,b]$. Sea $n$ un número natural (número de subintervalos en los que subdividiremos el intervalo $[a,b]$) y $h=\frac{b-a}{n}$. Sean $x_i=a+ih$, $i=0,1,\ldots,n$, los extremos de dichos subintervalos, con $x_0=a$ y $x_n=b$. Entonces podemos aproximar $\displaystyle\int_a^b f(x)\,dx$ de la forma siguiente:
$$
\int_a^b f(x)\, dx=\sum_{i=0}^{n-1}\int_{x_{i}}^{x_{i+1}}f(x)\, dx=\sum_{i=0}^{n-1}\left(\frac{h}{2}\left(f(x_{i})+f(x_{i+1})\right)-\frac{h^3}{12}f''(\xi_i)\right),
$$
con $\xi_i\in (x_{i},x_{i+1})$.

## Fórmula de trapecios compuesta
La expresión anterior puede simplificarse de la forma siguiente:
$$
\int_a^b f(x)\, dx =\frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right)-\frac{h^3}{12}\sum_{i=0}^{n-1}f''(\xi_i).
$$
Usando el **Teorema de Bolzano generalizado**, podemos afirmar que existe un $\xi\in (a,b)$ tal que 
$$
\sum_{i=0}^{n-1}f''(\xi_i)=n f''(\xi).
$$

## Fórmula de trapecios compuesta
La fórmula de trapecios compuesta queda de la forma siguiente:
$$
\begin{align*}
\int_a^b f(x)\, dx = & \frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right)-\frac{h^3}{12} n f''(\xi)\\ = & \frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right)-\frac{(b-a)}{12}h^2 f''(\xi),
\end{align*}
$$
donde hemos usado que $h\cdot n=b-a$.


## Fórmula de trapecios compuesta

La aproximación de la **fórmula de trapecios compuesta** es:
$$
T(h)=\frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right),
$$
con error:
$$
E(f,a,b)=-\frac{(b-a)}{12}h^2 f''(\xi).
$$

<l class="observ">Observación.</l>

La **fórmula de trapecios compuesta** tiene error $O(h^2)$.

## Fórmula de trapecios compuesta. Pseudocódigo
* `INPUT a,b,n`. (damos los extremos del intervalo de integración y el número de subintervalos)
* `Set h=(b-a)/n`. (calculamos el valor de $h$)
* `Set S0=f(a)+f(b)`. (calculamos $f(a)+f(b)$)
* `Set S=0` (en la variable $S$ vamos a guardar la suma $\displaystyle\sum_{i=1}^{n-1}f(x_i)$)
* `For i=1,...,n-1` (calculamos la suma anterior)
  * `Set X=a+i*h`.
  * `Set S=S+f(X)`.
* `Set Iaprox = (h/2)*(S0+2*S)`. (calculamos el valor de la integral aproximada)
* `Print (Iaprox)`. (damos dicho valor)
* `STOP`.

## Fórmula de Simpson compuesta
Vamos a aplicar la técnica anterior pero en lugar de considerar la **fórmula de los trapecios**, consideraremos la **fórmula de Simpson**.

Recordemos que para aplicar la fórmula de Simpson en un intervalo $[c,d]$ necesitábamos tres puntos: $c$, $d$ y $\frac{c+d}{2}$:
$$
\int_c^d f(x)\, dx=\frac{h}{3}\left(f(c)+4f\left(\frac{c+d}{2}\right)+f(d)\right)-\frac{h^5}{90}f^{(4)}(\xi),
$$
donde $\xi\in (c,d)$ y $h=\frac{d-c}{2}$.

## Fórmula de Simpson compuesta

Sea $f\in {\cal C}^4[a,b]$ de clase ${\cal C}^4$ en un cierto intervalo $[a,b]$. 
Sea $n$ un número par, $h=\frac{b-a}{n}$ y $x_i=a+ih$, con $i=0,1,\ldots,n$. Dividimos el intervalo $[a,b]$ en los subintervalos siguientes:
$$
[a,b]=[x_0,x_2]\cup [x_2,x_4]\cup\cdots\cup [x_{n-2},x_n],
$$
es decir, aplicaremos la fórmula de Simpson a cada subintervalo de la forma $[x_{2i},x_{2i+2}]$, $i=0,1,\ldots,\frac{n-2}{2}$:
$$
\int_{x_{2i}}^{x_{2i+2}} f(x)\, dx=\frac{h_i}{3}\left(f(x_{2i})+4f(x_{2i+1})+f(x_{2i+2})\right)-\frac{h_i^5}{90}f^{(4)}(\xi_i),
$$
donde $h_i=\frac{x_{2i+2}-x_{2i}}{2}=\frac{a+(2i+2)h-a-2ih}{2}=\frac{2h}{2}=h$ y $\xi_i\in (x_{2i},x_{2i+2})$.

## Fórmula de Simpson compuesta
La fórmula resultante será la siguiente:
$$
\begin{align*}
\int_a^b f(x)\, dx=& \sum_{i=0}^{\frac{n-2}{2}}\int_{x_{2i}}^{x_{2i+2}}f(x)\, dx \\ =& \sum_{i=0}^{\frac{n-2}{2}}\left(\frac{h}{3}\left(f(x_{2i})+4f(x_{2i+1})+f(x_{2i+2})\right)-\frac{h^5}{90}f^{(4)}(\xi_i)\right)\\ = & \frac{h}{3}\left(f(a)+2\sum_{i=1}^{\frac{n-2}{2}}f(x_{2i})+4\sum_{i=0}^{\frac{n-2}{2}}f(x_{2i+1})+f(b)\right)\\ & -\frac{h^5}{90}\sum_{i=0}^{\frac{n-2}{2}}f^{(4)}(\xi_i).
\end{align*}
$$

## Fórmula de Simpson compuesta
Usando el **Teorema de Bolzano generalizado**, podemos afirmar que existe un $\xi\in (a,b)$ tal que:
$$
\sum_{i=0}^{\frac{n-2}{2}}f^{(4)}(\xi_i)=\left(1+\frac{n-2}{2}\right)f^{(4)}(\xi)=\frac{n}{2}f^{(4)}(\xi).
$$


## Fórmula de Simpson compuesta

La **fórmula de Simpson compuesta** será, pues:
$$
\begin{align*}
\int_a^b f(x)\, dx=& \frac{h}{3}\left(f(a)+2\sum_{i=1}^{\frac{n-2}{2}}f(x_{2i})+4\sum_{i=0}^{\frac{n-2}{2}}f(x_{2i+1})+f(b)\right)\\ & -\frac{h^5}{90}\cdot\frac{n}{2}f^{(4)}(\xi) \\ =& \frac{h}{3}\left(f(a)+2\sum_{i=1}^{\frac{n-2}{2}}f(x_{2i})+4\sum_{i=0}^{\frac{n-2}{2}}f(x_{2i+1})+f(b)\right)\\ & -\frac{(b-a)}{180}\cdot h^4 f^{(4)}(\xi).
\end{align*}
$$

## Fórmula de Simpson compuesta
<l class="observ">Observación.</l>

La **fórmula de Simpson compuesta** tiene error $O(h^4)$.



<div class="example">
**Ejemplo**

Consideremos la misma función logística $f(x)=\frac{1}{1+\mathrm{e}^{-5x}}$ anterior pero ahora para $x\in [0,3]$. 
Queremos calcular:
$$
\int_{0}^3 f(x)\, dx=\int_{0}^3 \frac{1}{1+\mathrm{e}^{-5x}}\, dx.
$$

Sea $n=6$. Entonces, dividimos el intervalo de integración $[0,3]$ de la forma siguiente:
$$
[0,3]=[0,1]\cup [1,2]\cup [2,3]
$$

El valor de $h$ será $h=\frac{3-0}{6}=0.5$. 
</div>

## Ejemplo
<div class="example">
Si aplicamos la fórmula de Simpson compuesta, obtenemos:
$$
\begin{align*}
\int_0^3 f(x)\,dx=& \frac{0.5}{3}(f(0)+2 (f(1)+f(2))+4 (f(0.5)+f(1.5)+f(2.5))+f(3))\\ & -\frac{3}{180}\cdot 0.0625\cdot f^{(4)}(\xi) \\ = & 2.8634774-0.0010417\cdot f^{(4)}(\xi)
\end{align*}
$$

El valor exacto de la integral es: $\frac{1}{5}\ln\left(\frac{1}{2}\left(1+\mathrm{e}^{15}\right)\right)\approx 2.8613706.$ Hemos cometido un error de $0.0021068$.

En la figura siguiente se observa que el error está concentrado en el subintervalo $[0,1]$ que es donde la función decrece de forma súbita.

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=jDgWqcBYLXzR)
</div>
</div>

## Ejemplo
![](05DerivacionIntegracion_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

## Fórmula Simpson compuesta. Pseudocódigo
* `INPUT a,b,n`. (damos los extremos del intervalo de integración y el valor $n$)
* `Set h=(b-a)/n`. (calculamos el valor de $h$)
* `Set S0=f(a)+f(b)`. (calculamos $f(a)+f(b)$)
* `Set S1=0`. (en la variable $S1$ vamos a guardar la suma $\displaystyle\sum_{i=1}^{\frac{n-2}{2}}f(x_{2i})$
*  `Set S2=0`. (y en $S2$, $\displaystyle \sum_{i=0}^{\frac{n-2}{2}}f(x_{2i+1})$)

## Fórmula Simpson compuesta. Pseudocódigo
* `For i=1,...,n-1` (calculamos las sumas anteriores)
  * `Set X=a+i*h`.
  * `If i par then`
    * `Set S1=S1+f(X)`
  * `Else`
    * `Set S2=S2+f(X)`.
* `Set Iaprox = (h/3)*(S0+2*S1+4*S2)`. (calculamos el valor de la integral aproximada)
* `Print (Iaprox)`. (damos dicho valor)
* `STOP`.


## Ejercicio
<div class="exercise">
**Ejercicio**

Demostrar la fórmula de la **integración compuesta del punto medio**:

<l class="prop">Teorema.</l>

Sea $f\in {\cal C}^2[a,b]$ de clase ${\cal C}^2$ en un cierto intervalo $[a,b]$. Sea $n$ un número par, $h=\frac{b-a}{n+2}$ y $x_i=a+(i+1)h$, $i=-1,0,\ldots,n+1$. Entonces, existe un $\xi\in (a,b)$ tal que:
$$
\int_a^b f(x)\, dx=2h\sum_{i=0}^{\frac{n}{2}}f(x_{2i})+\frac{(b-a)}{6}h^2 f''(\xi).
$$

</div>

# Estabilidad

## Introducción
Los algoritmos para calcular aproximaciones de derivadas son **inestables** como ya estudiamos en su momento, ver la parte de **inestabilidad del error de redondeo**.

En pocas palabras, vimos que no tenía sentido considerar valores de $h$ muy pequeños ya que el **error de redondeo** debido a la evaluación de la función $f$ tenía una parte que tendía a infinito o "explotaba" cuando $h$ tendía a cero.

Nos preguntamos lo mismo con los algoritmos de integración numérica. ¿Son estables o inestables?

Es decir, si consideramos una fórmula de integración compuesta y aumentamos el número de subintervalos considerados, ¿cómo afectará este hecho al error global cometido, teniendo en cuenta aparte del **error de truncamiento del método**, los **errores de redondeo** cometidos al evaluar la función $f$?

## Estudio para el método de trapecios compuesto
Estudiemos la estabilidad para el **método de trapecios compuesto**:
$$
T(h)=\frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right).
$$
Sean $e_i$, $i=0,1,\ldots,n$ los errores de redondeo cometidos al evaluar $f(x_i)$, es decir:
$$
f(x_i)=\tilde{f}(x_i)+e_i,\ i=0,1,\ldots,n,
$$
donde $\tilde{f}(x_i)$ es el valor que calculamos realmente y $f(x_i)$ su valor exacto.

## Estudio para el método de trapecios compuesto

Entonces el **error de redondeo** cometido por el método para un $h$ determinado está acotado por:
$$
e(h)\leq \frac{h}{2}\left(|e_0|+2\sum_{i=1}^{n-1} |e_i|+|e_n|\right).
$$
Si todos los errores $e_i$ están acotados por $e$, tenemos que:
$$
e(h)\leq \frac{h}{2}(1+2(n-1)+1)e=nhe=(b-a)e.
$$
El **error de redondeo** $e(h)$ está acotado por una constante independiente de $h$ y, por tanto, de $n$. Esto significa que si aumentamos $n$ o disminuimos $h$, el número de operaciones requeridas no aumentará el **error de redondeo** global del método lo que significa que el método es **estable** si $h$ tiende a cero.

## Estudio para el método de Simpson compuesto
$$
S(h)=\frac{h}{3}\left(f(a)+2\sum_{i=1}^{\frac{n-2}{2}}f(x_{2i})+4\sum_{i=0}^{\frac{n-2}{2}}f(x_{2i+1})+f(b)\right).
$$
En este caso, el **error global de redondeo** del método estará acotado por:
$$
\begin{align*}
e(h)\leq & \frac{h}{3}\left(e+2\frac{(n-2)}{2} e+4\left(\frac{(n-2)}{2}+1\right)e+e\right) \\ = & \frac{h}{3}\cdot 3ne=hne=(b-a)e.
\end{align*}
$$

Nos pasa lo mismo que en el **método de trapecios compuesto**, lo que deducimos que el **método de Simpson compuesto** también es estable cuando $h$ tiende a cero o si aumentamos $n$.

## Estudio en general
Hemos dado un procedimiento para estudiar la estabilidad de un método numérico de integración.

En general, como los métodos de integración compuesta tendrán expresiones similares a la **fórmula de los trapecios compuesta** o a la **fórmula de Simpson** serán estables. De todas formas, para estar seguros, podemos aplicar el procedimiento anterior y estudiar su **estabilidad**.

# Integración de Romberg

## Introducción
El método de **integración de Romberg** consiste en aplicar la **extrapolación de Richardson** a un método de integración numérica.

En esta sección, para ilustrar su funcionamiento, aplicaremos la **extrapolación de Richardson** al **método de los trapecios compuesto**.


## Introducción
En primer lugar, se puede demostrar el resultado siguiente:

<l class="prop">Teorema. Expresión del error del método de los trapecios compuesto.</l>

Sea $f\in {\cal C}^2[a,b]$ de clase ${\cal C}^2$ en un cierto intervalo $[a,b]$. Sea $n$ un número natural y $h=\frac{b-a}{n}$. Sean $x_i=a+ih$, $i=0,1,\ldots,n$, los extremos de dichos subintervalos, con $x_0=a$ y $x_n=b$. Entonces el **método de los trapecios compuesto** admite una expresión como la que sigue:
$$
\begin{align*}
\int_a^b f(x)\, dx= & \frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right)\\ & +k_1 h^2+k_2 h^4+\cdots + k_n h^{2n}+\cdots
\end{align*}
$$

Entonces sólo aparecen exponentes pares en los exponentes de la $h$ en la expresión del error.

## Método de los trapecios compuesto
Llamamos $T_0=T(h)=\frac{h}{2}\left(f(a)+2\sum_{i=1}^{n-1}f(x_i)+f(b)\right)$.  Para hallar las aproximaciones de orden superior, usábamos la expresión siguiente:
$$
T_{n+1}(h)=\frac{4^{n+1}T_n\left(\frac{h}{2}\right)-T_n(h)}{4^{n+1}-1}=C+k_{n+2}\frac{p_{n+1}}{q_{n+1}}h^{2(n+2)}+\cdots
$$


## Método de los trapecios compuesto

<div class="center"> 
|$O(h^2)$|$O(h^4)$|$O(h^6)$|$O(h^8)$|
|:---|:---|:---|:---|
$T_0(h)$||||
$T_0\left(\frac{h}{2}\right)$|$T_1(h)=\frac{4T_0\left(\frac{h}{2}\right)-T_0(h)}{4-1}$|||
$T_0\left(\frac{h}{4}\right)$|$T_1\left(\frac{h}{2}\right)=\frac{4T_0\left(\frac{h}{4}\right)-T_0\left(\frac{h}{2}\right)}{4-1}$|$T_2(h)=\frac{4^2 T_1\left(\frac{h}{2}\right)-T_1(h)}{4^2-1}$||
$T_0\left(\frac{h}{8}\right)$|$T_1\left(\frac{h}{4}\right)=\frac{4T_0\left(\frac{h}{8}\right)-T_0\left(\frac{h}{4}\right)}{4-1}$|$T_2\left(\frac{h}{2}\right)=\frac{4^2 T_1\left(\frac{h}{4}\right)-T_1\left(\frac{h}{2}\right)}{4^2-1}$|$T_3(h)=\frac{4^3 T_2\left(\frac{h}{2}\right)-T_2(h)}{4^3-1}$|
</div>   

## Pseudocódigo
* `INPUT a,b,n0,n`. (damos los extremos del intervalo de integración, el valor $n_0$ del número de subintervalos inicial a considerar y el orden $h^{2n}$ al que queremos llegar)
* `For i=0,...,n do` (calculamos primero $T_0\left(\frac{h_0}{2^i}\right)$)
  * `Set ni=n0*2^i`. (calculamos el número de subintervalos para el nivel $i$-ésimo)
  * `Set h=(b-a)/(ni)`. (calculamos el $h$ para el nivel $i$-ésimo)
  * `Set T_0[i]=(h/2)*(f(a)+f(b)+Sum(f(a+k*h),k=1,...,ni-1))`. (calculamos $T_0\left(\frac{h_0}{2^i}\right)$)


## Pseudocódigo

* `For k=1,...,n do` (el índice $k$ nos indicará el nivel donde estamos. Guardaremos los valores $T_k\left(\frac{h_0}{2^i}\right)$, $i=0,\ldots,n-k$ en el vector $T_k$)
  * `For i=0,...,n-k do` (bucle para el índice $i$)
    * `Set ni=n0*2^i`. 
    * `Set h=(b-a)/(ni).`
    * `Set T_k[i]=(4^k*T_{k-1}(h/2)-T_{k-1}(h))/(4^k-1).`
* `Print T_n[0]`. (damos la aproximación de orden $h^{2n}$)
* `STOP`.
  

## Ejemplo


<div class="example">

Recordemos la función del ejemplo anterior $f(x)=\frac{1}{1+\mathrm{e}^{-5x}}$ para $x\in [0,3]$. 

Queremos calcular:
$$
\int_{0}^3 f(x)\, dx=\int_{0}^3 \frac{1}{1+\mathrm{e}^{-5x}}\, dx.
$$

Consideramos $h=0.1$. La tabla anterior será la siguiente:

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=7Tekg8kcLnbf)
</div>
</div>

## Ejemplo
<div class="example">

<div class="center"> 
|$O(h^2)$|$O(h^4)$|$O(h^6)$|$O(h^8)$|
|:---|:---|:---|:---|
$T_0(h)=2.8603268$||||
$T_0\left(\frac{h}{2}\right)=2.8611101$|$T_1(h)=2.8613712$|||
$T_0\left(\frac{h}{4}\right)=2.8613055$|$T_1\left(\frac{h}{2}\right)=2.8613707$|$T_2(h)=2.8613706$||
$T_0\left(\frac{h}{8}\right)=2.8613543$|$T_1\left(\frac{h}{4}\right)=2.8613706$|$T_2\left(\frac{h}{2}\right)=2.8613706$|$T_3(h)=2.8613706$|
</div>   

Recordemos que el valor exacto de la integral era $2.8613706$. 

Vemos que las aproximaciones son muy buenas. 
</div>

# Integración gaussiana

## Introducción
Las **fórmulas de cuadratura numérica** vistas hasta el momento tienen la expresión siguiente:
$$
\int_a^b f(x)\, dx\approx \sum_{i=0}^n a_i\cdot f(x_i),
$$
donde $a_i$, $i=0,\ldots,n$ son unos coeficientes a determinar de cara a tener el menor error posible o la mayor **precisión** posible y $x_i$, $i=0,1,\ldots, n$ son los nodos que consideramos **equiespaciados** y que vienen dados en función del intervalo de integración $[a,b]$.

Es decir dado $[a,b]$ y valor $n$, se determinan los nodos equiespaciados $x_i$, $i=0,1,\ldots, n$.

## Introducción
En esta sección, vamos a introducir unas **fórmulas de cuadratura** donde se admite más libertad en el sentido de que los **nodos de integración** $x_i$ no están predeterminados y forman parte de los parámetros a determinar en la **fórmula de cuadratura numérica** correspondiente:
$$
\int_a^b f(x)\, dx\approx \sum_{i=0}^n a_i\cdot f(x_i).
$$
Es decir, no se presupone que los nodos $x_i$ sean equiespaciados sino que se pueden escoger de tal forma que la expresión anterior tenga la **precisión** más alta posible.

## Ilustración del método
Supongamos que $n=0$ y el intervalo $[a,b]$ es $[-1,1]$. Nos planteamos hallar una **fórmula de cuadratura** de la forma: $\displaystyle\int_{-1}^1 f(x)\, dx \approx a_0 f(x_0)$ que tenga la **precisión** más alta posible. 

Como tenemos dos parámetros, $a_0$ y $x_0$, podemos exigir **precisión** igual a $1$:
$$
\begin{align*}
\int_{-1}^1 1\, dx = &  2=a_0,\\
\int_{-1}^1 x\, dx= & \left[\frac{x^2}{2}\right]_{-1}^1 = 0=a_0\cdot x_0.
\end{align*}
$$
Los valores de los parámetros $a_0$ y el nodo $x_0$ son los siguientes: $a_0=2,\  x_0=0$.

## Ilustración del método
Entonces, la **fórmula de cuadratura**:
$$
\int_{-1}^1 f(x)\, dx\approx 2\cdot f\left(0\right),
$$
tiene **precisión** $1$.

Consideremos $n=1$. En este caso, buscamos coeficientes $a_0$ y $a_1$ y nodos $x_0$ y $x_1$ tal que la **fórmula de cuadratura** $\displaystyle\int_{-1}^1 f(x)\, dx\approx a_0 f(x_0)+a_1 f(x_1)$ tenga **precisión** lo más alta posible.

Como tenemos $4$ parámetros, $a_0,a_1,x_0$ y $x_1$ podemos exigir **precisión** $3$:

## Ilustración del método
$$
\begin{align*}
\int_{-1}^1 1\, dx = &  2=a_0+a_1,\\
\int_{-1}^1 x\, dx= &  0=a_0\cdot x_0+a_1\cdot x_1,\\
\int_{-1}^1 x^2\, dx= & \frac{2}{3}=a_0\cdot x_0^2+a_1\cdot x_1^2,\\
\int_{-1}^1 x^3\, dx= &  0=a_0\cdot x_0^3+a_1\cdot x_1^3.
\end{align*}
$$
Las soluciones del sistema anterior son: $a_0=a_1=1$, $x_0=\frac{1}{\sqrt{3}}$ y $x_1=-\frac{1}{\sqrt{3}}$.

## Ilustración del método
Entonces, la **fórmula de cuadratura**:
$$
\int_{-1}^1 f(x)\, dx\approx f\left(-\frac{1}{\sqrt{3}}\right)+f\left(\frac{1}{\sqrt{3}}\right),
$$
tiene **precisión** $3$.

Por último, consideremos $n=2$.  En este caso, buscamos coeficientes $a_0$, $a_1$ y $a_2$ y nodos $x_0$, $x_1$ y $x_2$ tal que la **fórmula de cuadratura** $\displaystyle\int_{-1}^1 f(x)\, dx\approx a_0 f(x_0)+a_1 f(x_1)+a_2 f(x_2)$ tenga **precisión** lo más alta posible.

Como tenemos $6$ parámetros, $a_0,a_1,a_2,x_0,x_1$ y $x_2$ podemos exigir **precisión** $5$:

## Ilustración del método
$$
\begin{align*}
\int_{-1}^1 1\, dx = &  2=a_0+a_1+a_2,\\
\int_{-1}^1 x\, dx= &  0=a_0\cdot x_0+a_1\cdot x_1+a_2 x_2,\\
\int_{-1}^1 x^2\, dx= & \frac{2}{3}=a_0\cdot x_0^2+a_1\cdot x_1^2+a_2\cdot x_2^2,\\
\int_{-1}^1 x^3\, dx= &  0=a_0\cdot x_0^3+a_1\cdot x_1^3+a_2\cdot x_2^3,\\
\int_{-1}^1 x^4\, dx= &  \frac{2}{5}=a_0\cdot x_0^4+a_1\cdot x_1^4+a_2\cdot x_2^4,\\
\int_{-1}^1 x^5\, dx= &  0=a_0\cdot x_0^5+a_1\cdot x_1^5+a_2\cdot x_2^5.
\end{align*}
$$

## Ilustración del método
Las soluciones del sistema anterior son:
$$
a_0=\frac{5}{9},\ a_1=\frac{5}{9},\ a_2=\frac{8}{9},\ x_0=\sqrt{\frac{3}{5}},\ x_1=-\sqrt{\frac{3}{5}},\ x_2=0.
$$

Entonces, la **fórmula de cuadratura**:
$$
\int_{-1}^1 f(x)\, dx\approx \frac{5}{9}\cdot f\left(-\sqrt{\frac{3}{5}}\right)+\frac{8}{9}\cdot f(0)+\frac{5}{9}\cdot f\left(\sqrt{\frac{3}{5}}\right),
$$
tiene **precisión** $5$.

¿Cómo podemos obtener fórmulas para valores de $n$ más grandes y generalizar dichas fórmulas para cualquier intervalo $[a,b]$?

Para poder hacerlo, necesitamos conocer los **polinomios de Legendre**:

## Polinomios de Legendre
<l class="definition">Definición de los polinomios de Legendre.</l>

Las sucesión de polinomios $P_0(x),P_1(x),\ldots,P_n(x),\ldots$ forma la **sucesión de los polinomios de Legendre** si se cumplen las condiciones siguientes:

* el grado de $P_n(x)$ es $n$,
* $\displaystyle\int_{-1}^1 P(x)\cdot P_n(x)\, dx=0$, para cualquier polinomio $P(x)$ de grado menor o igual que $n-1$.

Como los **polinomios de Legendre** están definidos salvo una constante multiplicativa, consideraremos **polinomios de Legendre** mónicos, es decir, polinomios $P_n(x)$ cuyo coeficiente de $x^n$ es $1$.

## Polinomios de Legendre
Los primeros polinomios de Legendre son los siguientes:
$$
\begin{align*}
P_0(x)= & 1,\ P_1(x)=x,\ P_2(x)=x^2-\frac{1}{3},\ P_3(x)=x^3-\frac{3}{5}x,\\ P_4(x)= & x^4-\frac{6}{7}x^2+\frac{3}{35}.
\end{align*}
$$

<div class="exercise">
**Ejercicio**

Demostrar que los $4$ primeros polinomios de Legendre vienen dados por las expresiones anteriores. Es decir hay que ver que 
$$
\int_{-1}^1 P(x)\cdot P_n(x)\, dx=0,
$$
siendo $P(x)$ cualquier polinomio de grado menor o igual que $n-1$, para $n=1,2,3,4$.
</div>

## Polinomios de Legendre
<l class="prop">Proposición. Expresiones de los polinomios de Legendre.</l>

* Los **polinomios de Legendre mónicos** verifican la siguiente relación de recurrencia (*fórmula de Bonet*):
$$
P_{n+1}(x)=x\cdot P_n(x)-\frac{n^2}{(4n^2-1)}P_{n-1}(x),\ n=1,2,\ldots
$$
* Los **polinomios de Legendre** también pueden calcularse de la forma siguiente (*fórmula de Rodrigues*):
$$
P_n(x)=\frac{1}{2^n\cdot (2n-1)!!}\frac{d^n}{dx^n}(x^2-1)^n.
$$


Las expresiones anteriores nos dan una manera para ir calculando todos los **polinomios de Legendre**.

## Polinomios de Legendre
Las raíces de los **polinomios de Legendre** cumplen la condición siguiente:

<l class="prop">Proposición. Raíces de los polinomios de Legendre.</l>
Sea $P_n(x)$ el **polinomio de Legendre** de grado $n$. Entonces sus raíces $x_1,\ldots,x_n$ son reales, todas están en el intervalo $(-1,1)$ y son simétricas respecto al origen, es decir, si $x_i$ es una raíz de $P_n(x)$, $P_n(x_i)=0$, entonces $-x_i$ también lo es, es decir, $P_n(-x_i)=0$.

<l class="observ">Observación.</l> 
Se puede demostrar a partir de la *fórmula de Bonet* que los **polinomios de Legendre** $P_n(x)$ son pares si $n$ es par e impares si $n$ es impar. Es decir:
$$
P_{2k}(-x)=P_{2k}(x),\quad P_{2k+1}(-x)=-P_{2k+1}(x),
$$
para todo valor de $k$.

<div class="exercise">
**Ejercicio**

Demostrar por inducción la observación anterior.
</div>


## Cuadratura gaussiana
El teorema siguiente nos dice que si escogemos como nodos los **ceros de los polinomios de Legengre**, obtenemos una cuadratura de precisión $2n-1$ en el intervalo $[-1,1]$, donde $n$ es el grado del **polinomio de Legendre** considerado. 

<l class="prop">Teorema. Cuadratura gaussiana</l>
Sea $P_n(x)$ el **polinomio de Legendre** de grado $n$. Sean $x_1,x_2,\ldots,x_n$ las raíces del mismo en el intervalo $(-1,1)$. Consideramos los números $c_i$ definidos de la forma siguiente:
$$
c_i=\int_{-1}^1 \prod_{j=1,\\ j\neq i}^n \frac{x-x_j}{x_i-x_j}\, dx.
$$
Entonces si $P(x)$ es un polinomio de grado menor o igual que $2n-1$, 
$$
\int_{-1}^1 P(x)\,dx=\sum_{i=1}^n c_i P(x_i).
$$

## Cuadratura gaussiana
<div class="dem">
**Demostración**

Supongamos primero que $P(x)$ es un polinomio de grado menor o igual que $n-1$.

Escribiendo el polinomio $P(x)$ en función de los **polinomios de Lagrange de interpolación** $L_i(x)$ en los nodos $x_1,\ldots,x_n$, tenemos:
$$
P(x)=\sum_{i=1}^n P(x_i)L_i(x)=\sum_{i=1}^n \prod_{j=1,\\ j\neq i}^n \frac{x-x_j}{x_i-x_j} P(x_i).
$$
Entonces:
$$
\int_{-1}^1 P(x)\, dx=\int_{-1}^1 \sum_{i=1}^n \prod_{j=1,\\ j\neq i}^n \frac{x-x_j}{x_i-x_j} P(x_i) =\sum_{i=1}^n P(x_i)\int_{-1}^1\prod_{j=1,\\ j\neq i}^n \frac{x-x_j}{x_i-x_j}\, dx=\sum_{i=1}^n c_i P(x_i).
$$
En conclusión, acabamos de demostrar que la cuadratura es exacta para polinomios de grado menor o igual que $n-1$.
</div>

## Cuadratura gaussiana
<div class="dem">
**Demostración** (continuación)

Supongamos ahora que $P(x)$ es un polinomio de grado menor o igual que $2n-1$. Sea $P_n(x)$ el **polinomio de Lagrange** de grado $n$. Sean $Q(x)$ y $R(x)$ el cociente y el resto respectivamente del cociente de $P(x)$ entre $P_n(x)$:
$$
P(x)=Q(x)\cdot P_n(x)+R(x).
$$
Como los nodos $x_i$ son ceros del polinomio $P_n(x)$, $P_n(x_i)=0$, $i=1,\ldots, n$, tenemos que:
$$
P(x_i)=Q(x_i)\cdot P_n(x_i)+R(x_i)=R(x_i).
$$
A continuación, usamos la propiedad fundamental de los **polinomios de Legendre**. Como $Q(x)$ es un polinomio de grado menor o igual que $n-1$ ($2n-1-n=n-1$), se verifica:
$$
\int_{-1}^1 Q(x)\cdot P_n(x)\, dx=0.
$$
Como $R(x)$ es un polinomio de grado menor o igual que $n-1$, sabemos que la **cuadratura** anterior es exacta para $R(x)$:
$$
\int_{-1}^1 R(x)\,dx=\sum_{i=1}^n c_i R(x_i).
$$

</div>

## Cuadratura gaussiana
<div class="dem">
**Demostración** (continuación)

Ya estamos en condiciones de probar el teorema:
$$
\begin{align*}
\int_{-1}^1 P(x)\, dx= & \int_{-1}^1 (Q(x)\cdot P_n(x)+R(x))\, dx =\int_{-1}^1 Q(x)\cdot P_n(x)\, dx +\int_{-1}^1 R(x)\, dx=\int_{-1}^1 R(x)\, dx\\ = & \sum_{i=1}^n c_i R(x_i) =\sum_{i=1}^n c_i P(x_i),
\end{align*}
$$
tal como queríamos ver.


</div>

## Cuadratura gaussiana
Los nodos $x_i$, $i=1,\ldots,n$ de los primeros polinomios de la **cuadratura gaussiana** son los siguientes:


<div class="center">
|$n$|$x_i,\ i=1,\ldots,n$|
|:---|:---|
|$1$|$0$|
|$2$|$-0.5773502692,\ 0.5773502692$|
|$3$|$-0.7745966692,\ 0,\ 0.7745966692$|
|$4$|$-0.8611363116,\ -0.3399810436,\ 0.3399810436,\ 0.8611363116$|
|$5$|$-0.9061798459,\ -0.5384693101,\ 0,\ 0.5384693101,\ 0.9061798459$|
|$6$|$-0.9324695142,\ -0.6612093865,\ -0.2386191861,\ 0.2386191861,\ 0.6612093865,\ 0.9324695142$|
</div>

## Cuadratura gaussiana
Los coeficientes $c_i$, $i=1,\ldots,n$ de las primeras expresiones de la cuadratura gaussiana son los siguientes: (están asociados en el mismo orden que los nodos $x_i$)


<div class="center">
|$n$|$c_i,\ i=1,\ldots,n$|
|:---|:---|
|$1$|$2$|
|$2$|$1,\ 1$|
|$3$|$0.5555555556,\ 0.8888888889,\ 0.5555555556$|
|$4$|$0.3478548451,\  0.6521451549,\  0.6521451549,\  0.3478548451$|
|$5$|$0.2369268851,\  0.4786286705,\  0.5688888889,\  0.4786286705,\  0.2369268851$|
|$6$|$0.1713244924,\  0.3607615730,\  0.4679139346,\  0.4679139346,\  0.3607615730, \ 0.1713244924$|
</div>

## Error en la cuadratura gaussiana
<l class="prop"> Error en la cuadratura gaussiana.</l>

Sea $n$ un entero positivo y $f\in {\cal C}^{2n}[-1,1]$ una función de clase $2n$ en el intervalo $[-1,1]$. Entonces existe un valor $\xi\in (-1,1)$ tal que el error de la cuadratura gaussiana viene dado por:
$$
\int_{-1}^1 f(x)\, dx =\sum_{i=1}^n c_i f(x_i)+\frac{f^{(2n)}(\xi)}{(2n)!}\int_{-1}^1 P_n(x)^2\, dx,
$$
donde $P_n(x)$ es el **polinomio de Legendre** de grado $n$.

## Ejemplo


<div class="example">
Consideremos la función $f(x)=\cos \left(\frac{\pi  x}{2}+1\right)$. 

Vamos a aplicar la cuadratura gaussiana a la función anterior para $n=2,3,4$:
$$
\begin{align*}
n=2: \quad \int_{-1}^1 \cos \left(\frac{\pi  x}{2}+1\right)\,dx\approx &  1\cdot f(-0.577)+1\cdot f(0.577)\\ = & 1\cdot 0.996+1\cdot (-0.33)=0.665858,\\
n=3: \quad \int_{-1}^1 \cos \left(\frac{\pi  x}{2}+1\right)\,dx\approx &  0.556\cdot f(-0.775)+0.889\cdot f(0)+0.556\cdot f(0.775)\\ = & 0.556\cdot 0.977+0.889\cdot 0.54+0.556\cdot (-0.602)=0.688412,\\
n=4: \quad \int_{-1}^1\cos \left(\frac{\pi  x}{2}+1\right)\,dx\approx &  0.348\cdot f(-0.861)+0.652\cdot f(-0.34)+0.652\cdot f(0.34)\\ & +0.348\cdot f(0.861)\\ = & 0.348\cdot 0.938+0.652\cdot 0.893+0.652\cdot 0.037+0.348\cdot (-0.705)\\ = & 0.687929.
\end{align*}
$$
</div>

## Ejemplo
<div class="example">
El valor exacto de la integral vale $\displaystyle\int_{-1}^1 \cos \left(\frac{\pi  x}{2}+1\right)\,dx=\frac{4\cdot\cos(1)}{\pi}\approx 0.687934.$

Los errores cometidos en las aproximaciones anteriores son los siguientes:

* $n=2$: $|0.665858-0.687934|=0.022076.$
* $n=3$: $|0.688412-0.687934|=0.000478.$
* $n=4$: $|0.687929-0.687934|=0.000005.$

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=12CrIoU0NJTF)
</div>
</div>

## Cuadratura gaussiana en cualquier intervalo
Si en lugar de integrar en el intervalo $[-1,1]$, tenemos que integrar en un intervalo cualquiera $[a,b]$, podemos considerar el cambio de variable siguiente $t=\varphi(x)=\left(\frac{b-a}{2}\right)x+\frac{a+b}{2}$ que nos pasa del intervalo $[-1,1]$ al intervalo $[a,b]$:
$$
\begin{align*}
\varphi:  [-1,1]\longrightarrow & [a,b]\\
  x \longrightarrow & t=\varphi(x)=\left(\frac{b-a}{2}\right)x+\frac{a+b}{2}.
\end{align*}
$$


## Cuadratura gaussiana en cualquier intervalo

Usando el cambio anterior, transformamos la integral de aproximar una función $f(x)$ en el intervalo $[a,b]$ en una integral en el intervalo $[-1,1]$ de la que sabemos aproximar usando la cuadratura gaussiana:
$$
\begin{align*}
\int_a^b f(t)\, dt =&  \frac{(b-a)}{2}\int_{-1}^1 f\left(\left(\frac{b-a}{2}\right)x+\frac{a+b}{2}\right)\, dx\\ \approx & \frac{(b-a)}{2}\sum_{i=1}^n c_i f\left(\left(\frac{b-a}{2}\right)x_i+\frac{a+b}{2}\right),
\end{align*}
$$
donde $x_i$, $i=1,\ldots,n$ son los $n$ ceros del $n$-ésimo polinomio de Legendre $P_n(x)$.

## Ejemplo


<div class="example">
Vamos a considerar el ejemplo anterior de aproximar la integral de la función $f(x)=\frac{1}{1+\mathrm{e}^{-5x}}$ en el intervalo $[0,3]$ usando integración gaussiana para $n=2,3,4$:
$$
\begin{align*}
n=2: \quad \int_{0}^3 f(t)\,dt= & \frac{3}{2}\int_{-1}^1 f\left(\frac{3}{2}x+\frac{3}{2}\right)\, dx\\ \approx &    \frac{3}{2}\left(1\cdot f\left(\frac{3}{2}(-0.577)+\frac{3}{2}\right)+1\cdot f\left(\frac{3}{2}0.577+\frac{3}{2}\right)\right)\\ = &
 \frac{3}{2}\left(1\cdot f(0.634)+1\cdot f(2.366)\right)\\ = &\frac{3}{2} (1\cdot 0.96+1\cdot (1))=2.939516,
\end{align*}
$$
</div>

## Ejemplo
<div class="example">
$$
\begin{align*}
n=3: \quad \int_{0}^3 f(t)\,dt= & \frac{3}{2}\int_{-1}^1 f\left(\frac{3}{2}x+\frac{3}{2}\right)\, dx\\ \approx &    \frac{3}{2}\Biggl(0.556\cdot f\left(\frac{3}{2}(-0.775)+\frac{3}{2}\right)+0.889\cdot f\left(\frac{3}{2}0 +\frac{3}{2}\right)\\ & +0.556\cdot f\left(\frac{3}{2}0.775+\frac{3}{2}\right)\Biggr)\\ = & 
 \frac{3}{2}\left(0.556\cdot f(0.338)+0.889\cdot f(1.5)+0.556\cdot f(2.662)\right)\\ = &\frac{3}{2} (0.556\cdot 0.844+0.889\cdot 0.999+0.556\cdot 1)=2.869506,
\end{align*}
$$
</div>

## Ejemplo
<div class="example">
$$
\begin{align*}
n=4: \quad \int_{0}^3 f(t)\,dt= & \frac{3}{2}\int_{-1}^1 f\left(\frac{3}{2}x+\frac{3}{2}\right)\, dx\\ \approx &    \frac{3}{2}\Biggl(0.348\cdot f\left(\frac{3}{2}(-0.861)+\frac{3}{2}\right)+0.652\cdot f\left(\frac{3}{2}(-0.34) +\frac{3}{2}\right)\\ & +0.652\cdot f\left(\frac{3}{2}0.34+\frac{3}{2}\right)+0.348\cdot f\left(\frac{3}{2}0.861+\frac{3}{2}\right)\Biggr)\\ = & 
 \frac{3}{2}\left(0.348\cdot f(0.208)+0.652\cdot f(0.99)+0.652\cdot f(2.01)+0.348\cdot f(2.792)\right)\\ = &\frac{3}{2} (0.348\cdot 0.739+0.652\cdot 0.993+0.652\cdot 1+0.348\cdot 1)=2.856963,
\end{align*}
$$

Recordemos que el valor exacto de la integral era $2.8613706$. 

</div>

## Ejemplo
<div class="example">
Los errores cometidos en las aproximaciones anteriores son los siguientes:

* $n=2$: $|2.939516-2.861371|=0.078145.$
* $n=3$: $|2.869506-2.861371|=0.008135.$
* $n=4$: $|2.856963-2.861371|=0.004407.$

<div class="center">
[![](Images/colab.png){width=10%}](https://colab.research.google.com/drive/1zFV_f2Jo8V2_dr56HKP4trthzSbPq4F3#scrollTo=tKjQeqmSN1Ki)
</div>
</div>
