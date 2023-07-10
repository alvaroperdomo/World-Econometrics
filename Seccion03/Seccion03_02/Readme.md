# Estimación de Modelos de Vectores Autorregresivos $VAR$
Si se quiere tener en cuenta todas las relaciones posibles entre, digamos, $k$ variables, parece sensato construir un modelo conjunto que incluya todas estas series de tiempo en lugar de construir modelos para cada una de las series individuales. 

Considere el sistema bivariado simple[^1]:

[^1]: **Por razones pedagógicas nos centraremos principalemente en el análisis de este sistema _VAR_ dos variables. Este _VAR_ sencillo es útil para ilustrar los sistemas multivariados de orden superior que se presentaran más adelante**

$I$) $y_t=b_{10}-b_{12}z_t+\gamma_{11}y_{t-1}+\gamma_{12}z_{t-1}+\varepsilon_{yt}$  

$II$) $z_t=b_{20}-b_{21}y_t+\gamma_{21}y_{t-1}+\gamma_{22}z_{t-1}+\varepsilon_{zt}$   

donde se supone que 
* tanto { $y_t$ } como { $z_t$ } son series estacionarias; 
* { $\varepsilon_{yt}$ }  y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco con desviaciones estándar $\sigma_y$  y $\sigma_z$, respectivamente; y 
* { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco no correlacionadas.

Las ecuaciones $I$ y $II$ constituyen un vector autorregresivo de primer orden $VAR(1)$ porque la longitud de rezago más larga es $1$. La estructura del sistema incorpora retroalimentación porque permite que $y_t$ y $z_t$ se afecten entre sí. Por ejemplo, $-b_{12}$  es el efecto contemporáneo de un cambio unitario de $z_t$ en $y_t$ y $\gamma_{12}$ es el efecto de un cambio de unitario de $z_{t-1}$ en $y_t$. 

Tenga en cuenta que los términos $\varepsilon_{yt}$ y $\varepsilon_{zt}$ son innovaciones puras (o choques) en $y_t$ y $z_t$, respectivamente. Por supuesto, 
* si $b_{21}≠0$, $\varepsilon_{yt}$  tiene un efecto contemporáneo indirecto en $z_t$, y
* si $b_{12}≠0$, $\varepsilon_{zt}$  tiene un efecto contemporáneo indirecto en $y_t$. 

Las ecuaciones $I$ y $II$ no pueden ser estimadas por Mínimos Cuadrados Ordinarios ( $MCO$ ) ya que 
* $y_t$ tiene un efecto contemporáneo en $z_t$ y
* $z_t$ tiene un efecto contemporáneo en $y_t$.

Por lo tanto, las estimaciones de $MCO$ sufrirían un sesgo de simultaneidad ya que los regresores y los términos de error estarían correlacionados.

Es posible transformar el sistema de ecuaciones en una forma compacta: $\mathbf{B x_t= \Gamma_0 + \Gamma_1 x_{t-1}+\varepsilon_t}$

donde $\eqalign{\mathbf{B} = {\left\lbrack \matrix{1 & b_{12} \cr b_{21} & 1} \right\rbrack}}$, $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{y_t \cr z_t} \right\rbrack}}$, $\eqalign{\mathbf{\Gamma_0}= {\left\lbrack \matrix{b_{10} \cr b_{20}} \right\rbrack}}$, $\eqalign{\mathbf{\Gamma_1} = {\left\lbrack \matrix{\gamma_{11} & \gamma_{12} \cr \gamma_{21} & \gamma_{22}} \right\rbrack}}$ y $\eqalign{\mathbf{\varepsilon_t} = {\left\lbrack \matrix{\varepsilon_{yt} \cr \varepsilon_{zt}} \right\rbrack}}$

Al premultiplicar por $\mathbf{B^{−1}}$ se obtiene el **modelo VAR en forma estándar** $\mathbf{x_t= A_0 + A_1x_{t-1}+e_t}$  donde $\mathbf{A_0=B^{−1}\Gamma_0}$, $\mathbf{A_1=B^{−1}\Gamma_1}$, y $\mathbf{e_t=B^{−1}\varepsilon_t}$

Por propósitos de notación, podemos definir
* $a_{i0}$  como el elemento $i$ del vector $\mathbf{A_0}$,
* $a_{ij}$ como el elemento en la fila $i$ y la columna $j$ de la matriz $\mathbf{A_1}$, y
* $e_{it}$ como el elemento $i$ del vector $\mathbf{e_t}$. 

Usando esta nueva notación, podemos reescribir $\mathbf{x_t= A_0 + A_1x_{t-1}+e_t}$ en la forma equivalente:

$i$) $y_t=a_{10}+a_{11}y_{t-1}+a_{12}z_{t-1}+e_{1t}$ 

$ii$) $z_t=a_{20}+a_{21}y_{t-1}+a_{22}z_{t-1}+e_{2t}$

Para distinguir entre los sistemas representados por $I$ y $II$ versus $i$ y $ii$, el primero se llama un **VAR estructural o sistema primitivo** y el segundo se llama un **VAR en forma estándar**. 

Es importante tener en cuenta que los términos de error (es decir, $e_{1t}$ y $e_{2t}$) son compuestos de los dos choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$. Y se puede demostrar, aunque aquí no lo desarrollaremos, que dado que como $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son procesos de ruido blanco, entonces $e_{1t}$ y $e_{2t}$ son ruido blanco

Un objetivo explícito del enfoque de Box-Jenkins es proporcionar una metodología que conduzca a modelos parsimoniosos. El objetivo final de hacer pronósticos precisos a corto plazo se logra mejor eliminando las estimaciones de parámetros insignificantes del modelo. 

Sims (1980) aboga por una estrategia de estimación alternativa. 

Considere la siguiente generalización multivariada de un proceso autorregresivo $\mathbf{x_t=A_0 +  A_1x_{t-1} + \dots + A_p x_{t-p} + e_t}$ donde 
* $\mathbf{x_t}$ es un vector ($n \times 1$) que reúne las 𝑛 variables incluidas en el $VAR$
* $\mathbf{A_0}$ es un vector ($n \times 1$) de interceptos
* $\mathbf{A_i}$ son las matrices ($n \times n$) de coeficientes
* $\mathbf{e_t}$ es un vector ($n \times 1$) de los términos de error

La metodología de Sims implica únicamente:
1) la determinación de las variables apropiadas para incluir en el $VAR$: Las variables que se incluirán en el $VAR$ se seleccionan de acuerdo con el modelo económico relevante.
2) la determinación de la longitud de rezagos apropiada: Existen una serie de pruebas (las cuales se explicarán más adelante) para hacer esto. No se hace un intento explícito de "reducir" el número de parámetros estimados. La matriz $\mathbf{A_0}$ contiene $n$ parámetros, y cada matriz $\mathbf{A_i}$ contiene $n^2$ parámetros; por lo tanto, $n+pn^2$ coeficientes deben ser estimados. 

Sin lugar a dudas, el $VAR$ esta sobreparameterizado ya que muchas de las estimaciones de los coeficientes serán no significativas. Sin embargo, el objetivo es encontrar las interrelaciones importantes entre las variables. Imponer incorrectamente restricciones nulas puede desperdiciar información importante. Además, es probable que los regresores sean altamente colineales, de modo que las pruebas $t$ sobre los coeficientes individuales no son guías confiables para reducir el modelo.

De todas formas, 

## Pronósticos
Una vez que se ha estimado el $VAR$, se puede utilizar como un modelo de pronóstico de múltiples ecuaciones. 

Suponga que estima el modelo de primer orden $\mathbf{x_t=A_0+A_1x_{t-1}+e_t}$ para obtener los valores de los coeficientes en $\mathbf{A_0}$ y en $\mathbf{A_1}$. Si sus datos van hasta el período $T$, es sencillo obtener los pronósticos de un periodo hacia adelante de sus variables utilizando la relación $E_T\mathbf{x_{T+1}=A_0+A_1x_T}$. De manera similar, se puede obtener un pronóstico de dos periodos hacia adelante utilizando la relación $E_T\mathbf{x_{T+2}=A_0+A_1x_{T+1}=A_0+A_1(A_0+A_1x_T)}$. 

Sin embargo, en un modelo de orden superior, puede haber un gran número de coeficientes estimados. Dado que los 𝑉𝐴𝑅 no restringidos están sobreparametrizados, los pronósticos pueden ser poco confiables. Con el fin de obtener un modelo parsimonioso, muchos pronosticadores eliminarían los coeficientes no significativos del $VAR$. Para después volver a estimar el modelo llamado $near-VAR$ mediante el uso de un $SUR$, para utilizar con fines de pronóstico. 

## La Función Impulso-Respuesta
Al igual que un proceso autorregresivo tiene una representación de media móvil, un $VAR$ puede escribirse como un vector de media móvil ($VMA$). 

$\eqalign{\mathbf{x_t=\mu+\sum_{i=1}^p A_1^i e_{t-i}}}$ es la representación $VMA$ de $\mathbf{x_t=A_0+A_1x_{t-1}+e_t}$  en donde las variables (es decir, $y_t$ y $z_t$) se expresan en términos de los valores actuales y pasados de los dos tipos de choques (es decir, $e_{1t}$ y $e_{2t}$).[^2] 

[^2]: **La representación _VMA_ es una característica esencial de la metodología de Sims (1980), ya que permite rastrear la trayectoria en el tiempo de los diversos choques de las variables contenidas en el sistema _VAR_**. 

Para fines ilustrativos, usaremos el modelo de primer orden de dos variables analizado previamente. Escribiendo el $VAR$ de dos variables en forma matricial, ${\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{a_{10} \cr a_{20}} \right\rbrack} + {\left\lbrack \matrix{a_{11} & a_{12} \cr a_{21} & a_{22}}\right\rbrack}{\left\lbrack \matrix{y_{t-1} \cr z_{t-1}} \right\rbrack}+{\left\lbrack \matrix{e_{1t} \cr e_{2t}} \right\rbrack}$ o usando $\eqalign{\mathbf{x_t=\mu+\sum_{i=1}^p A_1^i e_{t-i}}}$ tenemos $\eqalign{{\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{\overline{y} \cr \overline{z}} \right\rbrack} + \sum_{i=0}^\infty{\left\lbrack \matrix{a_{11} & a_{12} \cr a_{21} & a_{22}}\right\rbrack}^i+{\left\lbrack \matrix{e_{1(t-i)} \cr e_{2(t-i)}} \right\rbrack}}$.

Esta ecuación expresa $y_t$ y $z_t$ en términos de las secuencias $e_{1t}$ y $e_{2t}$. Sin embargo, si se reescriben en términos de las secuencias $\varepsilon_{yt}$ y $\varepsilon_{zt}$, se obtiene: $\eqalign{{\left\lbrack \matrix{y_t \cr z_t} \right\rbrack} = {\left\lbrack \matrix{\overline{y} \cr \overline{z}} \right\rbrack} + \sum_{i=0}^\infty{\left\lbrack \matrix{\phi_{11}(i) & \phi_{12}(i) \cr \phi_{21}(i) & \phi_{22}(i)}\right\rbrack}^i+{\left\lbrack \matrix{\varepsilon_{y(t-i)} \cr \varepsilon_{z(t-i)}} \right\rbrack}}$ donde $\phi_i=\frac{A_1^i}{1-b_{12}b_{21}}{\left\lbrack \matrix{1 & -b_{12} \cr -b_{21} & 1}\right\rbrack}$ o en forma más compacta, $\eqalign{\mathbf{x_t=\mu+\sum_{i=1}^\infty \phi_i \varepsilon_{t-i}}}$

La representación de la media móvil es una herramienta especialmente útil para examinar la interacción entre las secuencias { $y_t$ } y { $z_t$ }. 
Los coeficientes de $\phi_i$  se pueden usar para generar los efectos de los choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$  en las trayectorias temporales de las secuencias { $y_t$ } y { $z_t$ }: 

1) Los cuatro elementos $\phi_{jk}(0)$ son **multiplicadores de impacto**. Por ejemplo, el coeficiente $\phi_{12}(0)$ es el impacto instantáneo de un cambio en una unidad de $\varepsilon_{zt}$ en $y_t$. 
2) De la misma manera, los elementos $\phi_{12}(1)$ y $\phi_{12}(1)$ son las respuestas en el siguiente período de los cambios unitarios de $\varepsilon_{y(t-1)}$ y $\varepsilon_{z(t-1)}$ en $y_t$, respectivamente. La actualización un período hacía adelante también indica que $\phi_{12}(1)$ y $\phi_{12}(1)$ representan los efectos de los cambios unitarios de $\varepsilon_{yt}$ y $\varepsilon_{zt}$ en $y_{t+1}$.

Los efectos acumulados de los impulsos unitarios en $\varepsilon_{yt}$  y/o $\varepsilon_{zt}$  se pueden obtener mediante la suma apropiada de los coeficientes de las funciones de impulso-respuesta. Por ejemplo, tenga en cuenta que, después de $n$ periodos, el efecto de $\varepsilon_{zt}$  en el valor de $y_{t+n}$ es $\phi_{12}(n)$. Por lo tanto, después de $n$ periodos, la suma acumulada de los efectos de $\varepsilon_{zt}$  en la secuencia { $y_t$ } es $\eqalign{\sum_{i=0}^n \phi_{12}(i)}$

Cuando $n$ se aproxima al infinito se produce el efecto total acumulado. Si se supone que las secuencias { $y_t$ } y { $z_t$ } son estacionarias, debe darse el caso de que para todos los $j$ y $k$, los valores de $\phi_{12}(i)$ convergen a cero a medida que $i$ se hace grande. Esto ocurre porque los choques no pueden tener un efecto permanente en una serie estacionaria. 

Los cuatro conjuntos de coeficientes $\phi_{11}(i)$, $\phi_{12}(i)$, $\phi_{21}(i)$ y $\phi_{22}(i)$ se denominan funciones de impulso-respuesta. 
El dibujo de las funciones impulso-respuesta (es decir, el dibujo de los coeficientes de $\phi_{jk}(i)$  en función de $i$) es una forma práctica de representar visualmente el comportamiento de las series { $y_t$ } y { $z_t$ } en respuesta a los diversos choques. 

Suponga que las estimaciones de las ecuaciones $i$ y $ii$ arrojan los valores:
* $a_{10}=a_{20}=0$, $a_{11}=a_{22}=0.7$, y $a_{12}=a_{21}=0.2$.
* los elementos de la matriz $\Sigma$ son tales que $\sigma_1^2=\sigma_2^2$
* el coeficiente de correlación entre $e_{1t}$ y $e_{2t}$ es $\rho_{12}=0.8$.
* Por lo tanto, $e_{1t}=\varepsilon_{yt}-0.8\varepsilon_{zt}$ y $e_{2t}=\varepsilon_{zt}$   

Los paneles (a) y (b) de la figura de abajo muestran los efectos de los choques unitarios de $\varepsilon_{zt}$  y $\varepsilon_{yt}$  en las trayectorias temporales de las secuencias { $y_t$ } y { $z_t$ }. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/804ef3dc-2e0e-4fd7-bbaa-69ced830d223)

Como se muestra en el Panel (a), un choque de una unidad en $\varepsilon_{zt}$ hace que $z_t$  salte una unidad y que $y_t$ salte $0.8$ unidades. En el siguiente período, $\varepsilon_{z(t+1)}$ vuelve a cero, pero la naturaleza autorregresiva del sistema es tal que $y_{t+1}$ y $z_{t+1}$ no regresa inmediatamente a sus valores a largo plazo. 
Como $z_{t+1}=0.2y_t+0.7z_t+\varepsilon_{z(t+1)}$, se deduce que $z_{t+1}=0.2(0.8)+0.7(1)=0.86$. De manera similar, $y_{t+1}=0.7y_t+0.2z_t=0.76$. Como se puede ver en la figura, los valores subsiguientes de las secuencias { $y_t$ } y { $z_t$ } convergen a sus niveles de largo plazo.

Los efectos de un choque unitario de $\varepsilon_{yt}$  se muestran en el Panel (b) de la figura. Puede ver la asimetría de la descomposición inmediatamente comparando los dos gráficos. Un choque unitario en $\varepsilon_{yt}$ hace que el valor de $y_t$ aumente en una unidad; sin embargo, no hay un efecto contemporáneo sobre el valor de $z_t$, de modo que $y_t=1$ y $z_t=0$. En el período subsiguiente, $\varepsilon_{y(t+1)}$ vuelve a cero. La naturaleza autorregresiva del sistema es tal que 
$y_{t+1}=0.7y_t+0.2z_t=0.7$ y $z_{t+1}=0.2y_t+0.7z_t=0.2$. Los puntos restantes en la figura son los impulso-respuesta para los períodos $t+2$ a $t+20$. Como el sistema es estacionario, los impulso-respuesta finalmente decaen.

Un problema clave relacionado con las funciones impulso-respuesta es que se construyen utilizando los coeficientes estimados. Como cada coeficiente se estima de forma imprecisa, los impulso-respuesta también contienen errores. El problema es construir intervalos de confianza alrededor de los impulso-respuesta que permitan la incertidumbre de parámetros inherente al proceso de estimación. 

Para ilustrar la metodología, considere la siguiente estimación de un modelo $AR(1):y_t=0.6y_{t-1}$ donde el estadístico $t$ de $a_1$ es $4$, es decir el coeficiente parece bien estimado. 

Es fácil configurar la función impulso-respuesta: para cualquier nivel dado de $y_{t-1}$, un choque unitario de $\varepsilon_t$  aumentará $y_t$ en una unidad. En períodos subsiguientes, $y_{t+1}$ será $0.6$ y $y_{t+1}$  será $0.6^2$. Como puede verificar fácilmente, la función impulso-respuesta se puede escribir como $\phi(i)=0.6^i$.
Observe que la estimación puntual del coeficiente $AR(1)$ es de $0.6$ con una desvíación estándar de $0.15$ ($0.15=\frac{0.6}{4}$). Si estamos dispuestos a suponer que el coeficiente se distribuye normalmente, hay una probabilidad del 95% de que el valor real se encuentre dentro del intervalo de dos desviaciones estándar $0.3–0.9$. Como tal, el patrón de decaimiento podría estar en cualquier lugar entre $\phi(i)=0.9^i$ y $\phi(i)=0.3^i$. 

El problema es mucho más complicado en los sistemas de orden superior, ya que los coeficientes estimados se correlacionarán. Además, es posible que no se desee asumir la normalidad. Una forma de obtener los intervalos de confianza deseados del proceso $AR(p)$ $y_t=a_0+a_1y_{t-1}+...+a_py_{t-p}+\varepsilon_t$ es realizar el siguiente estudio de Monte Carlo:
   1) Calcule los coeficientes $a_0$ a $a_p$ usando $MCO$ y guarde los residuos. Sea $\hat{a_i}$ el valor estimado de $a_i$ y sean { $\hat{\varepsilon_i}$ } los residuos estimados.
   2) Para un tamaño de muestra de tamaño $T$, seleccione números aleatorios para representar la secuencia { $\varepsilon_i$ }. Por lo tanto, tendrá una serie simulada de longitud T$, llamada $\varepsilon_t^s$, que debería tener las mismas propiedades que el verdadero proceso de error. Use estos números aleatorios para construir la secuencia simulada { $y_t^s$ } como $y_t^s=\hat{a_0}+\hat{a_1}y_{t-1}^s+...+\hat{a_p}y_{t-p}^s+\varepsilon_t^s$ (Asegúrese de que inicializa correctamente la serie para eliminar los efectos de las condiciones iniciales.)
   3) Ahora actúe como si no conociera los valores de los coeficiente utilizados para generar la serie $y_t^s$. Calcule $y_t^s$ como un proceso $AR(p)$ y obtenga la función de impulso-respuesta. Si repite el proceso varios miles de veces, puede generar varios miles de funciones impulso-respuesta. Use estas funciones para construir los intervalos de confianza (por ejemplo, puede construir el intervalo que excluya el 2.5% más bajo y el 2.5% más alto de las respuestas para obtener un intervalo de confianza del 95%).

El beneficio de este método es que no necesita hacer supuestos especia-les con respecto a la distribución de los coeficientes autorregresivos. El cálculo real de los intervalos de confianza es solo un poco más complicado en un $VAR$. 

Considere el sistema de dos variables $i$  y $ii$. El problema se complica porque los residuos de la regresión están correlacionados. Como tal, se necesita escoger aleatoriamente $e_{1t}$ y $e_{2t}$ de tal manera que se mantenga la estructura apropiada del error. Un método simple consiste en escoger aleatoriamente $e_{1t}$ y usar el valor de $e_{2t}$ que corresponde a ese mismo período. Si usa una descomposición de Choleski tal que $b_{21}=0$, construya $\varepsilon_{yt}$ y $\varepsilon_{zt}$  utilizando $e_{1t}=\varepsilon_{yt}-b_{12}\varepsilon_{zt}$ y $e_{2t}=\varepsilon_zt$.

## La Descomposición de Varianza

Otra ayuda útil para descubrir las interrelaciones entre las variables en el sistema es la descomposición de varianza del error de pronóstico. 

Retome la ecuación $\mathbf{x_t= A_0 + A_1x_{t-1}+e_t}$, asuma que conoce los coeficientes de $\mathbf{A_0}$ y $\mathbf{A_1}$ y quiere pronosticar los diversos valores de $\mathbf{x_{t+1}}$ condicionados al valor observado de $\mathbf{x_t}$. Actualizando esta ecuación $n$ períodos hacia adelante y tomando la expectativa condicionada en $t$ de $\mathbf{x_{t+n}}$, se obtiene $E_t \mathbf{x_{t+n}}$. Entonces,

* El error de pronóstico un periodo hacia adelante es $\mathbf{x_{t+1}} - E_t\mathbf{x_{t+1}}=e_{t+1}$ 
* El error de pronóstico dos periodos hacia adelante es $\mathbf{x_{t+2}} - E_t\mathbf{x_{t+2}}=e_{t+2} + A_1e_{t+1}$.
* El error de pronóstico 𝑛 periodos hacia adelante es $\eqalign{\mathbf{x_{t+n}} - E_t\mathbf{x_{t+n}}=\sum_{i=0}^{n-1}A_1^ie_{t+n-i}}$

También podemos considerar estos errores de pronóstico en términos de $\eqalign{ \mathbf{x_t=\mu+\sum_{i=1}^p\phi_i \varepsilon_{t-i}}}$ (es decir, la forma $VMA$ del modelo estructural). 
Por supuesto, los modelos $VMA$ y $VAR$ contienen exactamente la misma información, pero es conveniente describir las propiedades de los errores de pronóstico en términos de la secuencia { $\mathbf{\varepsilon_{t-i}}$ }. 

Si usamos $\eqalign{ \mathbf{x_t=\mu+\sum_{i=1}^p\phi_i \varepsilon_{t-i}}}$ para pronosticar condicionalmente $\mathbf{x_{t+1}}$, un periodo hacia adelante el error de pronóstico es $\mathbf{\phi_0 \varepsilon_{t+i}}$. En general, $\eqalign{ \mathbf{x_t=\mu+\sum_{i=0}^\inf \phi_i \varepsilon_{t+n-i}}}$ de modo que el error de pronóstico del periodo $n$ es $\eqalign{\mathbf{x_{t+n}} - E_t\mathbf{x_{t+n}}=\sum_{i=0}^{n-1}\phi_i \varepsilon_{t+n-i}}$ 

Centrándonos únicamente en la secuencia { $y_t$ }, vemos que el error de pronostico $n$ periodos hacía adelante es 

$\eqalign{y_{t+n}-E_t y_{t+n}= \sum_{i=0}^{n-1}[\phi_{11}(i) \varepsilon_{y(t+n-i)} + \phi_{12}(i) \varepsilon_{z(t+n-i)}]}$

La varianza del error de pronóstico $n$ periodos hacia adelante de $y_{t+n}$ es: $\eqalign{[\sigma_y(n)]^2=\sum_{i=0}^{n-1}(\sigma_y^2[\phi_{11}(i)]^2 + \sigma_z^2[\phi_{12}(i)]^2)}$. Debido a que todos los valores $[\phi_{jk}(i)]^2$ son necesariamente no negativos, la varianza del error de pronóstico aumenta a medida que aumenta el horizonte de pro-nóstico. 

Es posible descomponer la varianza del error de previsión $n$ periodos hacia adelante en las proporciones debidas a cada choque. Las proporciones de $[\sigma_y(n)]^2$ debidas a choques en las secuencias { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ } son $\eqalign{\frac{\sum_{i=0}^{n-1}\sigma_y^2[\phi_{11}(i)]^2}{[\sigma_y(n)]^2}}$ y  $\eqalign{\frac{\sum_{i=0}^{n-1}\sigma_z^2[\phi_{11}(i)]^2}{[\sigma_y(n)]^2}}$ respectivamente. 

**La descomposición de varianza del error de pronóstico nos dice la proporción de los movimientos en una secuencia debido a sus "propios" choques contra los choques de la otra variable.** 
* Si los choques $\varepsilon_{zt}$ no explican la varianza del error de pronóstico de $y_t$ en todos los horizontes de pronóstico, podemos decir que la secuencia { $y_t$ } es exógena [en esta circunstancia, { $y_t$ } evoluciona independientemente de los choques de $\varepsilon_{zt}$ y de la secuencia { $z_t$ }].
* En el otro extremo, los shocks $\varepsilon_{zt}$ podrían explicar toda la varianza del error de pronóstico en la secuencia { $y_t$ } en todos los horizontes de pronóstico, de modo que { $y_t$ } sería completamente endógeno. 

En la investigación aplicada, es usual que una variable explique casi toda su varianza de error de pronóstico en horizontes cortos y proporciones más pequeñas en horizontes más largos. Esperamos este patrón si los choques de $\varepsilon_{zt}$ tuvieran un bajo efecto contemporáneo en $y_t$, pero afectaran la secuencia { $y_t$ } de forma rezagada.

La descomposición de varianza tiene el mismo problema inherente en el análisis de la función impulso-respuesta: Para identificar las secuencias $\varepsilon_{yt}$ y $\varepsilon_{zt}$, es necesario restringir la matriz $\mathbf{B}$. La descomposición de Choleski que utilizamos previamente requiere que toda la varianza del error de pronóstico de un período de $z_t$ se deba a $\varepsilon_{zt}$. Si utilizamos el ordenamiento alternativo, toda la varianza del error de pronóstico de un período de $y_t$ se debería a $\varepsilon_{yt}$. Los efectos de estos supuestos alternativos se reducen en horizontes de pronóstico más largos. 

En la práctica, es útil examinar las descomposiciones de varianza en varios horizontes de pronóstico. A medida que $n$ aumenta, las descomposiciones de varianza deberían converger. Además, si el coeficiente de correlación $\rho_{12}$ es significativamente diferente de cero, es habitual obtener las descomposiciones de varianza en varios ordenamientos.

Sin embargo, el análisis impulso-respuesta y las descomposiciones de va-rianza pueden ser herramientas útiles para examinar las relaciones entre variables económicas. Si las correlaciones entre las diversas innovaciones son pequeñas, es probable que el problema de identificación no sea especialmente importante. Los ordenamientos alernativos deberían producir impulso-respuesta y descomposiciones de varianza similares. Por supuesto, los movimientos contemporáneos de muchas variables económicas están altamente correlacionados.

## Pruebas de Hipótesis

En principio, no hay nada que impida incorporar una gran cantidad de variables en el $VAR$. Es posible construir un $VAR$ de $n$ ecuaciones con cada ecuación conteniendo $p$ rezagos de todas las $n$ variables en el sistema. Por lo general, uno quiere incluir aquellas variables que tienen importantes efectos económicos entre sí. No obstante, tenga en cuenta que los grados de libertad disminuyen rápidamente a medida que se incluyen más variables. [^6] 

[^6]: Por ejemplo, al usar datos con 5 rezagos, la inclusión de una variable adicional usa 5 grados adicionales de libertad en cada ecuación. 

Un examen cuidadoso del modelo teórico relevante lo ayudará a seleccionar el conjunto de variables para incluir en su modelo $VAR$. Un $VAR$ de $n$ ecuaciones puede ser representado por:

$$ {\left\lbrack \matrix{ x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack} = \left\lbrack \matrix{ A_{10} \cr A_{20} \cr \dots \cr A_{n0} } \right\rbrack + \left\lbrack \matrix{A_{11}(L) & A_{12}(L) & \dots & A_{1n}(L) \cr A_{21}(L) & A_{22}(L) & \dots & A_{2n}(L) \cr \dots & \dots & \dots & \dots \cr A_{n1}(L) & A_{n2}(L) & \dots & A_{nn}(L) } \right\rbrack  \left\lbrack \matrix{ x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack + \left\lbrack \matrix{ e_{1t} \cr e_{2t} \cr \dots \cr e_{nt} } \right\rbrack
$$

* los $A_{i0}$ son los interceptos,
* los $A_{ij}(L)$ son los polinomios en el operador de rezagos $L$. Los coeficientes individuales de los $A_{ij}(L)$ se denotan por $a_{ij}(1), a_{ij}(2)$. Como todas las ecuaciones tienen la misma longitud de rezagos, los polinomios $A_{ij}(L)$ son todos del mismo grado.
* Los términos $e_{1t}$ son perturbaciones ruido blanco que pueden estar correlacionadas entre sí.
* La matriz de varianzas y covarianza $\mathbf{\Sigma}$ tiene dimensión ($n \times n$)

Además de la determinación del conjunto de variables a incluir en el $VAR$, es importante determinar la longitud apropiada de rezagos. Un posible procedimiento es permitir diferentes longitudes de rezago para cada variable en cada ecuación. Sin embargo, para preservar la simetría del sistema (y poder usar $MCO$ de manera eficiente), es común usar la misma longitud de rezagos para todas las ecuaciones. [^7] 

[^7]: Siempre que haya regresores idénticos en cada ecuación, las estimaciones MCO son consistentes y asintóticamente eficientes. 



















