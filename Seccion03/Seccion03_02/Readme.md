# Estimación de Modelos VAR
Si se quiere tener en cuenta todas las relaciones posibles entre, digamos, $k$ variables, parece sensato construir un modelo conjunto para todas estas series de tiempo en lugar de construir modelos para todas las series individuales. 

Considere el sistema bivariado simple:

$I$) $y_t=b_{10}-b_{12}z_t+\gamma_{11}y_{t-1}+\gamma_{12}z_{t-1}+\varepsilon_{yt}$  

$II$) $z_t=b_{20}-b_{21}y_t+\gamma_{21}y_{t-1}+\gamma_{22}z_{t-1}+\varepsilon_{zt}$   

donde se supone que 
* tanto $y_t$ como $z_t$ son estacionarios; 
* $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son perturbaciones ruido blanco con desviaciones estándar $\sigma_y$  y $\sigma_z$, respectivamente; y 
* { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco no correlacionadas.

Las ecuaciones $I$ y $II$ constituyen un vector autorregresivo de primer orden ($VAR$) porque la longitud de rezago más larga es $1$ [^1]. La estructura del sistema incorpora retroalimentación porque permite que $y_t$ y $z_t$ se afecten entre sí. Por ejemplo, $-b_{12}$  es el efecto contemporáneo de un cambio unitario de $z_t$ en $y_t$ y $\gamma_{12}$  es el efecto de un cambio de unitario de $z_{t-1}$ en $y_t$. 

[^1]: **Este _VAR_ sencillo es útil para ilustrar los sistemas multivariados de orden superior que se presentaran más adelante.**

Tenga en cuenta que los términos { $\varepsilon_{yt}$ y $\varepsilon_{zt}$  son innovaciones puras (o choques) en $y_t$ y $z_t$, respectivamente. Por supuesto, 
* si $b_{21}≠0$, $\varepsilon_{yt}$  tiene un efecto contemporáneo indirecto en $z_t$, y
* si $b_{12}≠0$, $\varepsilon_{zt}$  tiene un efecto contemporáneo indirecto en $y_t$. 

Las ecuaciones $I$ y $II$ no pueden ser estimadas por MCO ya que $y_t$ tiene un efecto contemporáneo en $z_t$ y $z_t$ tiene un efecto contemporáneo en $y_t$. Las estimaciones de MCO sufrirían un sesgo de simultaneidad ya que los regresores y los términos de error estarían correlacionados.

Es posible transformar el sistema de ecuaciones en una forma compacta:

$$Bx_t= \Gamma_0 + \Gamma_1x_{t-1}+\varepsilon_t$$ 

donde $\eqalign{B = {\left\lbrack \matrix{1 & b_{12} \cr b_{21} & 1} \right\rbrack}}$, $\eqalign{x_t = {\left\lbrack \matrix{y_t \cr z_t} \right\rbrack}}$, $\eqalign{\Gamma_0 = {\left\lbrack \matrix{b_{10} \cr b_{20}} \right\rbrack}}$, $\eqalign{\Gamma_1 = {\left\lbrack \matrix{\gamma_{11} & \gamma_{12} \cr \gamma_{21} & \gamma_{22}} \right\rbrack}}$ y $\eqalign{\varepsilon_t = {\left\lbrack \matrix{\varepsilon_{yt} \cr \varepsilon_{zt}} \right\rbrack}}$

Al premultiplicar por $B^{−1}$ se obtiene el **modelo VAR en forma estándar** $x_t= A_0 + A_1x_{t-1}+e_t$  donde $A_0=B^{−1}\Gamma_0$, $A_1=B^{−1}\Gamma_1$, y $e_t=B^{−1}\varepsilon_t$

Por propósitos de notación, podemos definir
* $a_{i0}$  como el elemento $i$ del vector $A_0$,
* $a_{ij}$ como el elemento en la fila $i$ y la columna $j$ de la matriz $A_1$, y
* $e_{it}$ como el elemento $i$ del vector $e_t$. 

Usando esta nueva notación, podemos reescribir $x_t= A_0 + A_1x_{t-1}+e_t$ en la forma equivalente:

$i$) $y_t=a_{10}+a_{11}y_{t-1}+a_{12}z_{t-1}+e_{1t}$ 

$ii$) $z_t=a_{20}+a_{21}y_{t-1}+a_{22}z_{t-1}+e_{2t}$

Para distinguir entre los sistemas representados por $I$ y $II$ versus $i$ y $ii$, el primero se llama un **VAR estructural o sistema primitivo** y el segundo se llama un **VAR en forma estándar**. 

Es importante tener en cuenta que los términos de error (es decir, $e_{1t}$ y $e_{2t}$) son compuestos de los dos choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$. Y se puede demostrar, aunque aquí no lo desarrollaremos, que dado que como $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son procesos de ruido blanco, entonces $e_{1t}$ y $e_{2t}$ son ruido blanco

Un objetivo explícito del enfoque de Box-Jenkins es proporcionar una metodología que conduzca a modelos parsimoniosos. El objetivo final de hacer pronósticos precisos a corto plazo se logra mejor eliminando las estimaciones de parámetros insignificantes del modelo. 

Sims (1980) aboga por una estrategia de estimación alternativa. 

Considere la siguiente generalización multivariada de un proceso autorregresivo $x_t= A_0 + A_1x_{t-1}+...+A_px_{t-p}+e_t$ donde 
* $x_t$ es un vector ($n×1$) que reúne las 𝑛 variables incluidas en el $VAR$
* $A_0$ es un vector ($n×1$) de interceptos
* $A_i$ son las matrices ($n×n$) de coeficientes
* $e_t$ es un vector ($n×1$) de los términos de error

La metodología de Sims implica únicamente:
1) la determinación de las variables apropiadas para incluir en el $VAR$: Las variables que se incluirán en el $VAR$ se seleccionan de acuerdo con el modelo económico relevante.
2) la determinación de la longitud de rezagos apropiada: Existen una serie de pruebas (las cuales se explicarán más adelante) para hacer esto. No se hace un intento explícito de "reducir" el número de parámetros estimados. La matriz $A_0$ contiene $n$ parámetros, y cada matriz $A_i$ contiene $n^2$ parámetros; por lo tanto, $n+pn^2$ coeficientes deben ser estimados. 

Sin lugar a dudas, el $VAR$ esta sobreparameterizado ya que muchas de las estimaciones de los coeficientes serán no significativas. Sin embargo, el objetivo es encontrar las interrelaciones importantes entre las variables. Imponer incorrectamente restricciones nulas puede desperdiciar información importante. Además, es probable que los regresores sean altamente colineales, de modo que las pruebas $t$ sobre los coeficientes individuales no son guías confiables para reducir el modelo.

## Pronósticos
Una vez que se ha estimado el $VAR$, se puede utilizar como un modelo de pronóstico de múltiples ecuaciones. 

Suponga que estima el modelo de primer orden $x_t=A_0+A_1x_{t-1}+e_t$ para obtener los valores de los coeficientes en $A_0$ y $A_1$. Si sus datos van hasta el período $T$, es sencillo obtener los pronósticos de un periodo hacia adelante de sus variables utilizando la relación $E_Tx_{T+1}=A_0+A_1x_T$. De manera similar, se puede obtener un pronóstico de dos periodos hacia adelante utilizando la relación $E_Tx_{T+2}=A_0+A_1x_{T+1}=A_0+A_1(A_0+A_1x_T)$. Sin embargo, en un modelo de orden superior, puede haber un gran número de coeficientes estimados. Dado que los 𝑉𝐴𝑅 no restringidos están sobreparametrizados, los pronósticos pueden ser poco confiables. Con el fin de obtener un modelo parsimonioso, muchos pronosticadores eliminarían los coeficientes no significativos del $VAR$. Para después volver a estimar el modelo llamado $near-VAR$ mediante el uso de un $SUR$, para utilizar con fines de pronóstico. 

## La Función Impulso-Respuesta
Al igual que un proceso autorregresivo tiene una representación de media móvil, un $VAR$ puede escribirse como un vector de media móvil ($VMA$). 
$\eqalign{x_t=\mu+\sum_{i=1}^p A_1^i e_{t-i}}$ es la representación $VMA$ de $x_t=A_0+A_1x_{t-1}+e_t$  en donde las variables (es decir, $y_t$ y $z_t$) se expresan en términos de los valores actuales y pasados de los dos tipos de choques (es decir, $e_{1t}$ y $e_{2t}$).[^2] 
[^2]: **La representación $VMA$ es una característica esencial de la metodología de Sims (1980), ya que permite rastrear la trayectoria en el tiempo de los diversos choques de las variables contenidas en el sistema _VAR_**. 

Para fines ilustrativos, usaremos el modelo de primer orden de dos variables analizado previamnete. Escribiendo el $VAR$ de dos variables en forma matricial, ${\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{a_{10} \cr a_{20}} \right\rbrack} + {\left\lbrack \matrix{a_{11} & a_{12} \cr a_{21} & a_{22}}\right\rbrack}{\left\lbrack \matrix{y_{t-1} \cr z_{t-1}} \right\rbrack}+{\left\lbrack \matrix{e_{1t} \cr e_{2t}} \right\rbrack}$ o usando $\eqalign{x_t=\mu+\sum_{i=1}^p A_1^i e_{t-i}}$ tenemos $\eqalign{{\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{\overline{y} \cr \overline{z}} \right\rbrack} + \sum_{i=0}^\infty{\left\lbrack \matrix{a_{11} & a_{12} \cr a_{21} & a_{22}}\right\rbrack}^i+{\left\lbrack \matrix{e_{1(t-i)} \cr e_{2(t-i)}} \right\rbrack}}$.

Esta ecuación expresa $y_t$ y $z_t$ en términos de las secuencias $e_{1t}$ y $e_{2t}$. Sin embargo, si se reescriben en términos de las secuencias $\varepsilon_{yt}$ y $\varepsilon_{zt}$, se obtiene: $\eqalign{{\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{\overline{y} \cr \overline{z}} \right\rbrack} + \sum_{i=0}^\infty{\left\lbrack \matrix{\phi_{11}(i) & \phi_{12}(i) \cr \phi_{21}(i) & \phi_{22}(i)}\right\rbrack}^i+{\left\lbrack \matrix{\varepsilon_{y(t-i)} \cr \varepsilon_{z(t-i)}} \right\rbrack}}$ donde $\phi_i=\frac{A_1^i}{1-b_{12}b_{21}}{\left\lbrack \matrix{1 & -b_{12} \cr -b_{21} & 1}\right\rbrack}$ o en forma más compacta, $\eqalign{x_t=\mu+\sum_{i=1}^\infty \phi_i \varepsilon_{t-i}}$

La representación de la media móvil es una herramienta especialmente útil para examinar la interacción entre las secuencias { $y_t$ } y { $z_t$ }. 
Los coeficientes de $\phi_i$  se pueden usar para generar los efectos de los choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$  en las trayectorias temporales de las secuencias { $y_t$ } y { $z_t$ }: 

1) Los cuatro elementos $\phi_{jk}(0)$ son **multiplicadores de impacto**. Por ejemplo, el coeficiente $\phi_{12}(0)$ es el impacto instantáneo de un cambio en una unidad de $\varepsilon_{zt}$ en $y_t$. 
2) De la misma manera, los elementos $\phi_{12}(1)$ y $\phi_{12}(1)$ son las respuestas en el siguiente período de los cambios unitarios de $\varepsilon_{y(t-1)}$ y $\varepsilon_{z(t-1)}$ en $y_t$, respectivamente. La actualización un período hacía adelante también indica que $\phi_{12}(1)$ y $\phi_{12}(1)$ representan los efectos de los cambios unitarios de $\varepsilon_{yt}$ y $\varepsilon_{zt}$ en $y_{t+1}$.

Los efectos acumulados de los impulsos unitarios en $\varepsilon_{yt}$  y/o $\varepsilon_{zt}$  se pueden obtener mediante la suma apropiada de los coeficientes de las funciones de impulso-respuesta. Por ejemplo, tenga en cuenta que, después de $n$ periodos, el efecto de $\varepsilon_{zt}$  en el valor de $y_{t+n}$ es $\phi_{12}(n)$. Por lo tanto, después de $n$ periodos, la suma acumulada de los efectos de $\varepsilon_{zt}$  en la secuencia { $y_t$ } es $\eqalign{\sum_{i=0}^n \phi_{12}(i)}$

Cuando $n$ se aproxima al infinito se produce el efecto total acumulado. Si se supone que las secuencias { $y_t$ } y { $z_t$ } son estacionarias, debe darse el caso de que para todos los $j$ y $k$, los valores de $\phi_{12}(i)$ convergen a cero a medida que $i$ se hace grande. Esto ocurre porque los choques no pueden tener un efecto permanente en una serie estacionaria. 


. 











