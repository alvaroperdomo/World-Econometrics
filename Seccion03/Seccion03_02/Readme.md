# Modelos de Vectores Autorregresivos $VAR$
Si se quiere tener en cuenta todas las relaciones posibles entre, digamos, $k$ variables, parece sensato construir un modelo conjunto que incluya todas estas series de tiempo en lugar de construir modelos para cada una de las series individuales, a este tipo de modelos se les llama modelos _VAR_. A continuación. Dando click en la **X** de la segunda columna de la siguiente tabla se redirigira a la explicación de cada uno de los aspectos teóricos que se encuentran en la primera columna de la tabla. Por otra parte, dando click en la **X** de la tercera columna de la tabla podra ver las aplicaciones en R de cada uno de estos temas:

| Temas                                                                                                     | Explicación teórica                   |  Aplicación en R                     |
|-----------------------------------------------------------------------------------------------------------|:-------------------------------------:|:---------------------:|
| ¿Qué es un modelo VAR? ¿Cuáles son los tipos de modelos VAR? y ¿Cómo hacer pronósticos con un modelo VAR? |  [X](Seccion02_02_ADF_T/Readme.md)    | [X](Seccion02_02_ADF_R/Readme.md)    | 
| La Función Impulso Respuesta y la Descomposición de Varianza                                              |  [X](Seccion02_02_ADFGLS_T/Readme.md) | [X](Seccion02_02_ADFGLS_R/Readme.md) |
| Pruebas de Hipótesis en Modelos VAR                                                                       |  [X](Seccion02_02_ZA_T/Readme.md)     | [X](Seccion02_02_ZA_R/Readme.md)     |

A continuación, se presenta un ejemplo utilizando la base de datos "Indicadores de Desarrollo Económico del Banco Mundial". Para el ejercicio, se ha escogido analizar la variable más utilizada en términos de desarrollo económico, el PIB per cápita, de un país en vías de desarrollo. Más específicamente, se va a analizar la evolución del PIB per cápita de Colombia a precios constantes.

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

En principio, no hay nada que impida incorporar una gran cantidad de variables en el $VAR$. Es posible construir un $VAR$ de $n$ ecuaciones con cada ecuación conteniendo $p$ rezagos de todas las $n$ variables en el sistema. Por lo general, uno quiere incluir aquellas variables que tienen importantes efectos económicos entre sí. No obstante, tenga en cuenta que los grados de libertad disminuyen rápidamente a medida que se incluyen más variables. [^3] 

[^3]: **Por ejemplo, al usar datos con 5 rezagos, la inclusión de una variable adicional usa 5 grados adicionales de libertad en cada ecuación.** 

Un examen cuidadoso del modelo teórico relevante lo ayudará a seleccionar el conjunto de variables para incluir en su modelo $VAR$. Un $VAR$ de $n$ ecuaciones puede ser representado por:

$$ {\left\lbrack \matrix{ x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack} = \left\lbrack \matrix{ A_{10} \cr A_{20} \cr \dots \cr A_{n0} } \right\rbrack + \left\lbrack \matrix{A_{11}(L) & A_{12}(L) & \dots & A_{1n}(L) \cr A_{21}(L) & A_{22}(L) & \dots & A_{2n}(L) \cr \dots & \dots & \dots & \dots \cr A_{n1}(L) & A_{n2}(L) & \dots & A_{nn}(L) } \right\rbrack  \left\lbrack \matrix{ x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack + \left\lbrack \matrix{ e_{1t} \cr e_{2t} \cr \dots \cr e_{nt} } \right\rbrack
$$

* los $A_{i0}$ son los interceptos,
* los $A_{ij}(L)$ son los polinomios en el operador de rezagos $L$. Los coeficientes individuales de los $A_{ij}(L)$ se denotan por $a_{ij}(1), a_{ij}(2)$. Como todas las ecuaciones tienen la misma longitud de rezagos, los polinomios $A_{ij}(L)$ son todos del mismo grado.
* Los términos $e_{1t}$ son perturbaciones ruido blanco que pueden estar correlacionadas entre sí.
* La matriz de varianzas y covarianza $\mathbf{\Sigma}$ tiene dimensión ($n \times n$)

Además de la determinación del conjunto de variables a incluir en el $VAR$, es importante determinar la longitud apropiada de rezagos. Un posible procedimiento es permitir diferentes longitudes de rezago para cada variable en cada ecuación. Sin embargo, para preservar la simetría del sistema (y poder usar $MCO$ de manera eficiente), es común usar la misma longitud de rezagos para todas las ecuaciones. [^4] 

[^4]: **Siempre que haya regresores idénticos en cada ecuación, las estimaciones MCO son consistentes y asintóticamente eficientes.** 

Si algunas de las ecuaciones $VAR$ tienen regresores no incluidos en las otras, las regresiones aparentemente no relacionadas ($SUR$ por sus siglas en inglés) proporcionan estimaciones eficientes de los coeficientes $VAR$. Por lo tanto, cuando hay una buena razón para dejar que las longitudes de los rezagos difieran entre las ecuaciones, estime un $near-VAR$ utilizando un $SUR$.

En un $VAR$, los rezagos consumen rápidamente los grados de libertad. Si la longitud del rezago es $p$, cada una de las $p$ ecuaciones contiene $np$ coeficientes más el intercepto. La selección apropiada de la longitud de rezagos puede ser crítica. 
* Si $p$ es demasiado pequeño, el modelo está mal especificado;
* si $p$ es demasiado grande, se pierden grados de libertad. 
Para verificar la longitud del rezago,
1) comience con la longitud plausible más larga o la longitud más larga posible dadas las consideraciones de grados de libertad.
2) Estime el $VAR$ y forme la matriz de varianzas y covarianzas de los residuos.

Por ejemplo, puede comenzar con un rezago de $3$ años basado en la noción a priori de que este tiempo es lo suficientemente largo para capturar la dinámica del sistema. La matriz de varianzas y covarianzas de los residuos del modelo a $3$ rezagos es $\mathbf{\Sigma_3}$. Ahora suponga que quiere determinar si $2$ rezagos son apropiados.  Después de todo, restringir el modelo de $3$ a 42$ rezagos reduciría el número de parámetros estimados en $n$ en cada ecuación. La prueba adecuada para esta restricción de ecuación cruzada es una prueba de razón de verosimilitud. Vuelva a estimar el $VAR$ durante el mismo período de muestra utilizando $2$ rezagos y obtenga la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_2}$. Tenga en cuenta que $\mathbf{\Sigma_2}$ pertenece a un sistema de $n$ ecuaciones con $n$ restricciones en cada ecuación, para un total de $n^2$ restricciones. El estadístico de la razón de verosimilitud es $T(\ln{|\mathbf{\Sigma_2}|}-\ln{|\mathbf{\Sigma_3}|})$. Sin embargo, dados los tamaños de muestra que generalmente se encuentran en el análisis económico, Sims (1980) recomendó usar $(T-c)(\ln{|\mathbf{\Sigma_2}|}-\ln{|\mathbf{\Sigma_3}|})$ donde 
* $T$ es el número de observaciones utilizables,
* $c$ el número de parámetros estimados en cada ecuación del sistema no restringido, y
* $\ln{|\mathbf{\Sigma_n}|}$ es el logaritmo natural del determinante de $\mathbf{\Sigma_n}$

En el ejemplo que nos ocupa, $c=1+3n$  ya que cada ecuación del modelo no restringido tiene $3$ rezagos para cada variable más un intercepto

Este estadístico tiene una distribución asintótica $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. En el ejemplo considerado, hay $n$ restricciones en cada ecuación, para un total de restricciones $n^2$ en el sistema. Si la restricción de un número reducido de rezagos no es vinculante, esperaríamos que $\ln{|\mathbf{\Sigma_2}|}=\ln{|\mathbf{\Sigma_3}|}$. Los valores grandes de este estadístico muestral indican que tener sólo $2$ rezagos es una restricción vinculante; por lo tanto, podemos rechazar la hipótesis nula de que la longitud de rezagos es igual a $2$. Si el valor calculado del estadístico es menor que el $\chi^2$ en un nivel de significación preespecificado, no podremos rechazar la hipótesis nula de sólo $2$ rezagos. En ese momento, podríamos tratar de determinar si $1$ rezagos eran apropiados construyendo $(T-c)(\ln{|\mathbf{\Sigma_1}|}-\ln{|\mathbf{\Sigma_2}|})$. Se debe tener mucho cuidado al reducir la longitud del rezago de esta manera. A menudo, este procedimiento no rechazará las hipótesis nulas de $2$ versus $3$ rezagos y $1$ versus $2$ rezagos, aunque rechazará una hipótesis nula de $1$ versus 43$ rezagos. El problema con reducir el modelo es que puede perder una pequeña cantidad de poder explicativo en cada etapa. En general, la pérdida total en el poder explicativo puede ser significativa. En tales circunstancias, es mejor utilizar las longitudes de rezago más largas.

Este tipo de prueba de razón de verosimilitud es aplicable a cualquier tipo de restricción de ecuación cruzada. Sean $\mathbf{\Sigma_u}$ y $\mathbf{\Sigma_r}$ las matrices de varianzas y covarianzas de los sistemas no restringidos y restringidos, respectivamente. Si las ecuaciones del modelo no restringido contienen regresores diferentes, vamos a denotar el número máximo de regresores contenidos en la ecuación más larga. La recomendación de Sims es comparar el estadís-tico de prueba $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ con respecto a una distribución $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. La prueba de razón de verosimilitud se basa en la teoría asintótica, que puede no ser muy útil en las muestras pequeñas disponibles para los econometristas de series de tiempo. 

Además, la prueba de razón de verosimilitud sólo es aplicable cuando un modelo es una versión restringida del otro. Los criterios de prueba alternativos son las generalizaciones multivariadas del Criterio de Información de Akaike (AIC por sus siglas en inglés) y el Criterio Bayesiano de Schwartz (BSC por sus siglas en inglés):
* $AIC=T \ln{|\mathbf{\Sigma}|}+2N$
* $BSC=T \ln{|\mathbf{\Sigma}|}+N\ln{T}$ 
donde
* $|\mathbf{\Sigma}|$ es el determinante de la varianza matriz de varianzas y covarianzas de los residuos
* $N$ es el número total de parámetros estimados en todas las ecuaciones. 

Por lo tanto, si cada ecuación en un $VAR$ de $n$ variables tiene $p$ rezagos y un intercepto, $N=n^2p+n$; cada una de las $n$ ecuaciones tiene $np$ regresores rezagados y un intercepto.La adición de regresores adicionales reducirá $|\mathbf{\Sigma}|$ a expensas de aumentar $N$. Como en el caso univariado, seleccione el modelo con el valor más bajo de $AIC$ o $BSC$. 

Tenga en cuenta que el Criterio de Información de Akaike y el Criterio Bayesiano de Schwartz multivariados no se pueden usar para probar la importancia estadística de modelos alternativos. En cambio, son medidas del ajuste general de las alternativas. 

Una prueba de causalidad establece si los rezagos de una variable entran en la ecuación de otra variable. En un modelo de dos ecuaciones con $p$ rezagos, { $y_t$ } no causa en el sentido de Granger a { $z_t$ } si y solo si todos los coeficientes de $A_{21}(L)$ son nulos. Por lo tanto, si { $y_t$ } no mejora el rendimiento de pronóstico de { $z_t$ }, entonces { $y_t$ } no causa en el sentido de Granger a { $z_t$ }. Si todas las variables en el $VAR$ son estacionarias, la forma directa de probar la causalidad de Granger es usar una prueba $F$ estándar de la restricción $a_21(1)=a_21(2)=a_21(3)= \dots = a_21(p)=0$. 

Es sencillo generalizar esta noción al caso de $n$ variables

$$ {\left\lbrack \matrix{ x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack} = \left\lbrack \matrix{ A_{10} \cr A_{20} \cr \dots \cr A_{n0} } \right\rbrack + \left\lbrack \matrix{A_{11}(L) & A_{12}(L) & \dots & A_{1n}(L) \cr A_{21}(L) & A_{22}(L) & \dots & A_{2n}(L) \cr \dots & \dots & \dots & \dots \cr A_{n1}(L) & A_{n2}(L) & \dots & A_{nn}(L) } \right\rbrack  \left\lbrack \matrix{ x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack + \left\lbrack \matrix{ e_{1t} \cr e_{2t} \cr \dots \cr e_{nt} } \right\rbrack
$$

Dado que $A_{ij}(L)$ representa los coeficientes de los valores rezagados de la variable $j$ en la variable $i$, la variable $j$ no causa la variable $i$ en el sentido de Granger si todos los coeficientes del polinomio $A_{ij}(L)$ pueden establecerse iguales a cero.

La causalidad de Granger es algo muy diferente de una prueba de exogeneidad. Para que $z_t$ sea exógena, requerimos que no se vea afectada por el valor contemporáneo de $y_t$. Sin embargo, la causalidad de Granger se refiere sólo a los efectos de valores pasados de { $y_t$ } en el valor actual de $z_t$. Por lo tanto, la causalidad de Granger en realidad mide si los valores actuales y pasados de { $y_t$ } ayudan a pronosticar valores futuros de { $z_t$ }. 

Para ilustrar la distinción en términos de un modelo $VMA$, considere la siguiente ecuación tal que $y_t$ no causa en el sentido de Granger $z_t$ y a pesar de todo $z_t$ no es exógeno $\eqalign{z_t=\bar{z} +\phi_{21}(0) \varepsilon_{yt} + \sum_{i=0}^{\infty} \phi_{22}(i) \varepsilon_{z(t-i)} }$

Si pronosticamos $z_{t+1}=$ condicionado a los valores de $\varepsilon_{z(t-i)}$ ($i=0, 1, \dots$) solo, obtendremos el error de pronóstico $\phi_{21}(0) \varepsilon_{y(t+1)} + \phi_{22}(1) \varepsilon_{z(t+1)}$. Sin embargo, obtenemos el mismo error de pronóstico si pronosticamos $z_{t+1}$ condicionado en $\varepsilon_{z(t-1)} y $\varepsilon_{y(t-1)}$ ($i=0, 1, \dots$). Dado el valor de $z_t$, la información sobre $y_t$ no ayuda a reducir el error de pronóstico para $z_{t+1}. 

En otras palabras, para el modelo en consideración, $E_t(z_{t+1}|z_t)=E_t(z_{t+1}|z_t, y_t). Por lo tanto, { $y_t$ } no causa { $z_t$ } en el sentido de Granger. 

Por otro lado, dado que estamos asumiendo que $\phi_{21}(0) \not= 0$, { $z_t$ } no es exógeno. Si $\phi_{21}(0) \not= 0$, los choques puros de $y_{t+1}$ (es decir, $\varepsilon_{y(t+1)}$ ) afectan el valor de $z_{t+1}$  aunque la secuencia { $y_t$ } Granger no cause la secuencia { $z_t$ }. 

Una prueba de bloques de exogeneidad es útil para detectar si se debe incorporar una variable adicional en un $VAR$. El problema consiste en determinar si los rezagos de una variable, por ejemplo, $w_t$, causan en el sentido de Granger alguna otra variable en el sistema. En el caso de tres variables con $w_t$, $y_t$ y $z_t$, la prueba es si los rezagos de $w_t$ causan en el sentido de Granger a $y_t$ o $z_t$. En esencia, el bloque de exogeneidad restringe todos los rezagos de $w_t$ en $y_t$ y $z_t$ para que sean iguales a cero. 

Esta restricción se prueba adecuadamente usando la prueba de razón de verosimilitud. Calcule las ecuaciones $y_t$ y $z_t$  usando valores rezagados de { $y_t$ } , { $z_t$ } y { $w_t$ } y calcule $\mathbf{\Sigma_u}$. Vuelva a estimar excluyendo los valores rezagados de { $w_t$ } y calcule $\mathbf{\Sigma_r}$. Encuentre el estadístico de razón de verosimilitud: $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ este estadístico tiene una distribución $\chi^2$ con grados de libertad iguales a $2p$ (ya que los $p$ valores de rezagados de { $w_t$ } se excluyen de cada ecuación). Aquí $c=3p+1$ porque las ecuaciones no restringidas de $y_t$ y $z_t$ contienen $p$ rezagos de { $y_t$ } , { $z_t$ } y { $w_t$ } más una constante.
























