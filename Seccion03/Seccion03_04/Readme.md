# Cointegración

A continuación, dando click en la X de la segunda columna de la siguiente tabla se redirigira a la explicación de cada uno de los aspectos teóricos que se encuentran en la primera columna de la tabla. Por otra parte, dando click en la X de la tercera columna de la tabla podra ver las aplicaciones en R de cada uno de estos temas:

| Temas                                                                                                     | Explicación teórica                   |  Aplicación en R                     |
|-----------------------------------------------------------------------------------------------------------|:-------------------------------------:|:---------------------:|
| ¿Qué es la Cointegración?                                                                                 |  [X](Seccion02_02_ADF_T/Readme.md)    | [X](Seccion02_02_ADF_R/Readme.md)    | 
| Prueba de Cointegración de Engle y Granger                                                                |  [X](Seccion02_02_ZA_T/Readme.md)     | [X](Seccion02_02_ZA_R/Readme.md)     |
| Prueba de Cointegración de Johansen                                                                       |  [X](Seccion02_02_ADFGLS_T/Readme.md) | [X](Seccion02_02_ADFGLS_R/Readme.md) |
| ¿Qué es un Modelo VEC? ¿Cómo se estima un Modelo VEC?                                                     |  [X](Seccion02_02_ADFGLS_T/Readme.md) | [X](Seccion02_02_ADFGLS_R/Readme.md) |

En los modelos univariados una tendencia estocástica puede eliminarse por diferenciación, las series estacionarias resultantes se pueden estimar utilizando las técnicas univariadas de Box-Jenkins. Anteriormente, la práctica convencional era generalizar esta idea a los modelos multivariados y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión para luego estimar un modelo VAR en diferencias. Sin embargo, hoy en día la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria y que por lo tanto configure una relación de largo plazo[^1]; si esto ocurre se dice que tales variables están **cointegradas**. 

[^1]: **En cierto sentido, el uso del término equilibrio es desafortunado porque los teóricos económicos y los econometristas usan el término de diferentes maneras. En teoría económica usualmente se usa el término para referirse a una igualdad entre transacciones deseadas y reales. El uso econométrico del término hace referencia a cualquier relación a largo plazo entre variables no estacionarias. La cointegración no requiere que la relación a largo plazo sea generada por las fuerzas del mercado o por las reglas de comportamiento de los individuos. Según Engle y Granger (1987) la relación de equilibrio puede ser causal, conductual o sim-plemente una relación de forma reducida entre variables con tendencias similares.** 

Para entender mejor lo que es cointegración considere el siguiente ejemplo: Asuma que hay $n$ variables $x_{1t}, x_{2t}, \dots, x_{nt}$ integradas del mismo orden que conforman un equilibrio de largo plazo representado por $\eqalign{\sum_{i=1}^n \beta_i x_{it}=0}$. Es decir, $\mathbf{B x_t}=0$ donde $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ y $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$

La desviación del equilibrio a largo plazo, llamado error de equilibrio es $e_t$, por lo tanto $e_t=\mathbf{\beta x_t}$. **Si el equilibrio es significativo, $e_t$ debe ser estacionario**. 

Engle y Granger (1987) proporcionan la siguiente definición de cointegración: Se dice que los componentes del vector $\mathbf{x_t}$ son cointegrados de orden $d,b$, denotado por $\mathbf{x_t}\sim CI(d,b)$  si 
1) Todos los componentes de $\mathbf{x_t}$ son integrados de orden $d$.
2) Existe un vector $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ tal que la combinación lineal $\mathbf{\beta x_t}=0$  está integrada de orden $(d-b)$ donde $b>0$.

  El vector $B$ se llama el vector cointegrante.

Hay cuatro puntos importantes a tener en cuenta sobre la definición:
1) **La cointegración se refiere a una combinación lineal de variables no estacionarias**.

   El vector de cointegración no es único. Si ${\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ es un vector de cointegración, entonces para cualquier $\lambda \not= 0$, ${\left\lbrack \matrix{\lambda\beta_1 & \lambda\beta_2 & \dots & \lambda\beta_n} \right\rbrack}$ también es un vector de cointegración.

   Normalmente, una de las variables se usa para normalizar el vector de cointegración fijando su coeficiente en $1$. Por ejemplo, para normalizar el vector de cointegración con respecto a $x_{1t}$, simplemente se selecciona un $\lambda=\frac{1}{\beta_1}$.

2) **La cointegración se refiere a variables que están integradas en el mismo orden**.

   Esto no implica que todas las variables integradas estén cointegradas; por lo general, un conjunto de variables $I(d)$ no está cointegrado.[^2] 

   **Si dos variables son integradas de órdenes diferentes, no pueden estar cointegradas.**

   Suponga que $x_{1t}$ es $I(d_1)$ y $x_{2t}$ es $I(d_2)$ donde $d_2>d_1$ . Entonces, cualquier combinación lineal de $x_{1t}$ con $x_{2t}$ es $I(d_2)$.

   Sin embargo, **es posible encontrar relaciones de equilibrio entre grupos de variables que están integradas de diferentes órdenes.**

   Suponga que $x_{1t}$ y $x_{2t}$ son $I(2)$ y que las otras variables en consideración son $I(1)$. Como tal, no puede haber una relación de cointegración entre $x_{1t}$ (o $x_{2t}$) y $x_{3t}$. Sin embargo, si $x_{1t}$ y $x_{2t}$ son $CI(2,1)$, existe una combinación lineal de la forma $\beta_1x_{1t}+\beta_2x_{2t}$ que es $I(1)$, la cual es posible que esté cointegrada con las variables $I(1)$.

[^2]: **Tal falta de cointegración implica que no hay un equilibrio a largo plazo entre las variables, de modo que puedan desviarse arbitrariamente una de la otra.**

3) **Puede haber más de un vector de cointegración independiente para un conjunto de variables $I(1)$.**[^3]

   Si $\mathbf{x_t}$ tiene $n$ componentes no estacionarios, puede haber hasta $n-1$ vectores de cointegración linealmente independientes. Por lo tanto, si $\mathbf{x_t}$ contiene solo dos variables, puede haber a lo sumo un vector de cointegración independiente.

[^3]: **El número de vectores de cointegración se denomina rango de cointegración del vector.**

4) **La mayor parte de la literatura sobre cointegración se centra en el caso en el que cada variable tiene una sola raíz unitaria.**

   La razón es que la regresión tradicional o el análisis de series de tiempo se aplica cuando las variables son $I(0)$ y pocas variables económicas están integradas en un orden superior a $1$. 

Para terminar la explicación, en el siguiente gráfico se muestra una situación en la que tres variables $I(1)$ estan con cointegradas

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/36a62b2e-6b7a-4643-8b77-507b154e0a5f)

Mientras que en este último gráfico se muestra una situación en la que dos variables $I(1)$ no estan con cointegradas

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4213813c-dae1-49b3-b6d7-f82180ac19fb)


## Pruebas de Cointegración

Para el análisis de cointegración las metodologías más utilizadas son la de Engle y Granger (1987) y la de Johansen (1988)
 
### La Metodología de Engle-Granger

Para explicar el procedimiento de prueba de Engle-Granger, comencemos con el tipo de problema que es usual en los estudios aplicados. Suponga que dos variables, digamos $y_t$ y $z_t$, son integradas de orden $1$ y se desea determinar si existe una relación de equilibrio entre las dos. Engle y Granger (1987) proponen un procedimiento de cuatro pasos para determinar si dos variables $I(1)$ están cointegradas de orden $CI(1,1)$:

1) **Haga una prueba preliminar de las variables para conocer su orden de integración.**
   
   Por definición, la cointegración requiere que dos variables estén integradas en el mismo orden. Por lo tanto, el primer paso en el análisis es determinar para cada variable su orden de integración.
     * Si ambas variables son estacionarias, no es necesario continuar ya que el método VAR se aplica a las variables estacionarias.
     * Si las variables están integradas de diferentes órdenes, se concluye que no están cointegradas.
     * Sin embargo, si tiene más de dos variables, de manera que algunas son $I(1)$ y otras son $I(2)$, es posible que desee determinar si las variables están cointegradas en forma múltiple.

2) **Estime la relación del equilibrio a largo plazo.**

   Si los resultados indican que tanto { $y_t$ } como { $z_t$ } son $I(1)$, estime utilizando Mínimos Cuadrados Ordinarios la relación de equilibrio: $y_t=\beta_0+\beta_1z_t+e_t$.

   Para determinar si las variables están realmente cointegradas, sea { $\hat{e_t}$ } la secuencia de residuos de esta ecuación. Por lo tanto, la serie { $\hat{e_t}$ } contiene los valores estimados de las desviaciones de la relación de largo plazo. Si se encuentra que estas desviaciones son estacionarias, las secuencias { $y_t$ } y { $z_t$ } son cointegradas de orden $(1,1)$. 

   Sería conveniente si si se pudiera realizar una prueba ADF de estos residuos para determinar su orden de integración. Considere la regresión de los residuos: $\Delta\hat{e}=a_1\hat{e_{t-1}}+\varepsilon_t$ (dado que la secuencia { $\hat{e_t}$ } es el residuo de una regresión, por lo que su media necesariamente igual a cero, entonces no es necesario incluir un intercepto dentro de la regresión) donde el parámetro de interés es $a_1$:

   * Si no se puede rechazar la hipótesis nula $a_1=0$, se concluye que los residuos tienen una raíz unitaria y por lo tanto, se deduce que las secuencias { $y_t$ } y { $z_t$ } no están cointegradas.
   * En cambio, el rechazo de la hipótesis nula implica que la secuencia de residuos es estacionaria. Si se encuentra que { $y_t$ } y { $z_t$ } son $I(1)$ y que los residuos son estacionarios, entonces se puede concluir que las series están cointegradas de orden $(1,1)$.

   En la mayoría de los estudios aplicados, no es posible utilizar las tablas de Dickey-Fuller. El problema es que la secuencia { $\hat{e_t}$ } se genera a partir de una regresión; donde el investigador no conoce el error real $e_t$, solo el error estimado { $\hat{e_t}$ }.[^4]

[^4]: **Engle y Granger (1987) propusieron nuevas tablas para hacer los cálculos. Y estas posteriormente fueron actualizadas por MacKinnon (1990). En estas tablas, los valores críticos dependen del tamaño de la muestra y del número de variables utilizadas en el análisis**

3) **Estime el vector de corrección de errores**

   Si las variables están cointegradas, los residuos de la regresión de equilibrio se pueden utilizar para estimar el vector de corrección de errores ($VEC$). El $VEC$ es un $VAR$ en primeras diferencias, en donde las variables { $y_t$ } y { $z_t$ } son $CI(1,1)$, y se incluyen dentro del VEC de la siguiente forma:

   $I)$ $\Delta y_t = \alpha_1 + \alpha_y[y_{t-1}-\beta_1z_{t-1}]+\sum_{i=1}\alpha_{11}(i)\Delta y_{t-i}+\sum_{i=1}\alpha_{12}(i)\Delta z_{t-i}+\varepsilon_{yt}$

   $II)$ $\Delta z_t = \alpha_2 + \alpha_z[y_{t-1}-\beta_1z_{t-1}]+\sum_{i=1}\alpha_{21}(i)\Delta y_{t-i}+\sum_{i=1}\alpha_{22}(i)\Delta z_{t-i}+\varepsilon_{zt}$

   donde

   * $\beta_1$ es el parámetro del vector de cointegración dado por $y_t=\beta_0+\beta_1 z_t + e_t$ 𝑦_𝑡=𝛽_0+𝛽_1 𝑧_𝑡+𝑒_𝑡;
   * $\varepsilon_{yt}$ y $\varepsilon_{zt}$ son  perturbaciones ruido blanco (que pueden estar corre-lacionadas entre sí), y
   * $\alpha_1$, $\alpha_2$, $\alpha_y$, $\alpha_z$, $\alpha_{11}(i)$, $\alpha_{12}(i)$, $\alpha_{21}(i)$, $\alpha_{22}(i)$ son todos los parámetros.
   
   Engle y Granger (1987) proponen una forma de sortear las restricciones de ecuaciones cruzadas involucradas en la estimación directa de $I$ y $II$. La magnitud del residuo $\hat{e_{t-1}}$ es la desviación del equilibrio a largo plazo en el período {t-1}. Por lo tanto, es posible usar los residuos estimados { $\hat{e_{t-1}}$ }  obtenidos en el Paso 2 como una estimación de la expresión $y_{t-1}-\beta_1z_{t-1}$ en $I$ y $II$. Por lo tanto, utilizando los $\hat{e_{t-1}}$ estime el VEC como:

   $i)$ $\Delta y_t = \alpha_1 + \alpha_y\hat{e_{t-1}}+\sum_{i=1}\alpha_{11}(i)\Delta y_{t-i}+\sum_{i=1}\alpha_{12}(i)\Delta z_{t-i}+\varepsilon_{yt}$

   $ii)$ $\Delta z_t = \alpha_2 + \alpha_z\hat{e_{t-1}}+\sum_{i=1}\alpha_{21}(i)\Delta y_{t-i}+\sum_{i=1}\alpha_{22}(i)\Delta z_{t-i}+\varepsilon_{zt}$

   Dado que la estructura del molelo $VEC$ es la de un $VAR$, entonces puede estimarse utilizando la misma metodología desarrollada en la presentación de los modelos $VAR$. Dado que todas las variables en $i$ y $ii$ son estacionarias (es decir, $\Delta y_t$ y sus rezagos, $\Delta z_t$ y sus rezagos, y $\hat{e_{t-1}}$ son $I(0)$ ), los estadísticos de prueba utilizados en el análisis $VAR$ tradicional son apropiados para $i$ y $ii$. 

4) **Evaluar la adecuación del modelo**

   Hay varios procedimientos que pueden ayudar a determinar si el $VEC$ es apropiado:

   a) **Realice verificaciones de diagnóstico para determinar si los residuos de las ecuaciones de corrección de errores se aproximan al ruido blanco**.

   Si los residuos están serialmente correlacionados, las longitudes de los rezagos pueden ser demasiado cortas. Vuelva a estimar el modelo utilizando longitudes de rezago que produzcan errores que no están serialmente correlacionados.

   b) **La velocidad de ajuste de los coeficientes $\alpha_y$  y $\alpha_z$  son de particular interés, ya que tienen implicaciones importantes para la dinámica del sistema.**

   Los valores de $\alpha_y$ y $\alpha_z$ están directamente relacionados con las raíces características del sistema de ecuaciones en diferencias. La convergencia directa requiere $\alpha_y$  que sea negativo y que $\alpha_z$  sea positivo.

   Si nos centramos en $ii$, para cualquier valor dado de $\hat{e_{t-1}}$,
   * un gran valor de $\alpha_z$ se asocia con un gran valor de $\Delta z_t$. 
   * Si $\alpha_z=0$ , el $\Delta z_t$  no responde en absoluto a $\hat{e_{t-1}}$. 
   * Si $\alpha_z=0$ y si todos los $\alpha_{21}=0$, entonces se puede decir que { $\Delta y_t$ } no causa en el sentido de Granger a { $\Delta z_t$ } .
   
     Sabemos que $\alpha_y$ y/o $\alpha_z$ deberían ser significativamente diferentes de cero si las variables están cointegradas. Después de todo, si $\alpha_y=\alpha_z=0$, no hay corrección de errores y $i$ y $ii$ serían nada más que un $VAR$ en las primeras diferencias. Además, los valores absolutos de estas velocidades de ajuste de los coeficientes no deben ser demasiado grandes. Las estimaciones puntuales deberían implicar que $\Delta y_t$ y $\Delta z_t$ convergen a la relación de equilibrio a largo plazo.

     c) **Al igual que en un análisis $VAR$ tradicional, Lutkepohl y Reimers (1992) muestran que el análisis de los impulso-respuesta y la descomposición de varianza se puede utilizar para obtener información sobre las interacciones entre las variables.**

     Como cuestión práctica, las dos innovaciones $\varepsilon_{yt}$  y $\varepsilon_{zt}$  pueden correlacionarse simultáneamente si $y_t$ tiene un efecto contemporáneo en $z_t$ y/o si $z_t$ tiene un efecto contemporáneo en $y_t$. Para obtener funciones impulso-respuesta y las descomposiciones de varianza, se debe utilizar algún método, como la descomposición de Choleski, para ortogonalizar las innovaciones. La forma de las funciones impulso-respuesta y los resultados de las descomposiciones de varianza pueden indicar si las respuestas dinámicas de las variables se ajustan a la teoría. Como todas las variables en $i$ y $ii$ son $I(0)$, los impulso-respuesta de $\Delta y_t$ y $\Delta z_t$ deben converger a cero.[^3]

     Es tentador utilizar estadísticos $t$ para realizar pruebas de significancia en el vector de cointegración. Sin embargo, debe evitar esta tentación ya que, en general, los coeficientes no tienen una distribución $t$ asintótica.
     
[^3]: **Debe reexaminar los resultados de cada paso si obtiene una función de impulso-respuesta explosiva o que no decae.**

### Inconvenientes con la Metodología de Engle-Granger
Aunque el procedimiento de Engle y Granger (1987) se implementa fácilmente, tiene varios defectos importantes. 

1) La estimación de la regresión de equilibrio a largo plazo requiere que el investigador coloque una variable en el lado izquierdo y utilice las otras variables como regresores. 

Por ejemplo, en el caso de dos variables, es posible hacer la prueba de cointegración de Engle-Granger utilizando los residuos de cualquiera de las siguientes dos regresiones de "equilibrio":

   * $y_t=\beta_{10}+\beta_{11}z_t+e_{1t}$  o
   * $y_t=\beta_{20}\beta_{21}y_t+e_{2t}$      

A medida que el tamaño de la muestra crece hacia infinito, la teoría asintótica indica que la prueba de raíz unitaria en la secuencia { $e_{1t}$ } se vuelve equivalente a la prueba de raíz unitaria en la secuencia { $e_{2t}$ } . Desafortunadamente, las propiedades de muestras grandes que obtienen este resultado pueden no ser aplicables a los tamaños de muestra generalmente disponibles. En la práctica, es posible encontrar que una regresión implique que las variables están cointegradas, mientras que revertir el orden implique que no hay cointegración. Esta es una característica muy indeseable del procedimiento porque la prueba de cointegración debe ser invariable a la elección de la variable seleccionada para la normalización. El problema se agrava utilizando tres o más variables, ya que cualquiera de las variables puede seleccionarse como la variable del lado izquierdo. Además, en las pruebas que usan tres o más variables, sabemos que puede haber más de un vector de cointegración. El método de Engle-Granger no tiene un proce-dimiento sistemático para la estimación separada de los múltiples vectores de coin-tegración

2) Otro defecto del procedimiento de Engle-Granger es que se basa en un estimador de dos pasos.

El primer paso es generar la serie de residuos $\hat{e_t}$, y el segundo paso utiliza estos errores generados para estimar una regresión de la forma $\Delta\hat{e_t}=a_1\Delta\hat{e_{t-1}}+\dots$ Entonces, el coeficiente $a_1$ se obtiene estimando una regresión que utiliza los residuos de otra regresión. Por lo tanto, cualquier error introducido por el investigador en el Paso $i$ se lleva al Paso $ii$. 

### La Metodología de Johansen

Los estimadores de máxima verosimilitud de Johansen (1988) evitan el uso de estimadores de dos pasos y pueden estimar y probar la presencia de múltiples vectores de cointegración. Además, estas pruebas permiten probar versiones restringidas de los vectores de cointegración y la velocidad de los parámetros de ajuste.[^3] 

[^3]: **A menudo, es interesante determinar si es posible verificar una teoría probando restricciones en las magnitudes de los coeficientes estimados**

El procedimiento de Johansen (1988) se basa en gran medida en la relación entre el rango de una matriz y sus raíces características. Este no es más que una generalización multivariada de la prueba $DF$. En el caso univariado, es posible ver que la estacionariedad de { $y_t$ } depende de $a_1$; es decir, dados $y_t=a_1y_{t-1}+\varepsilon_t$ o $\Delta y_t=(a_1-1)y_{t-1}+\varepsilon_t$. Si $(a_1-1)=0$, el proceso { $y_t$ } tiene una raíz unitaria. Descartando el caso en el que { $y_t$ } es explosivo, si $(a_1-1)≠0$ podemos concluir que la secuencia { $y_t$ } es estacionaria. Las tablas de Dickey-Fuller proporcionan los estadísticos apropiados para probar formalmente la hipótesis nula $(a_1-1)=0$.

Consideremos la generalización al caso simple con {n} variables; asuma que el vector $\mathbf{x_t}$ de $n$ variables, se comporta como $\mathbf{x_t=A_1x_{t-1}+\varepsilon_t}$  así que $\mathbf{\Delta x_t=A_1x_{t-1}-x_{t-1}+\varepsilon_t=(A_1-I)x_{t-1}+\varepsilon_t=\pi x_{t-1}+\varepsilon_t}$ donde 
* $\mathbf{\varepsilon_t}$ es un vector ( $n\times 1$ ),
* $\mathbf{A_1}$ es una matriz ( $n\times n$ ) de parámetros, 
* $\mathbf{I}$ es una matriz identidad ( $n\times n$ ),
* $\mathbf{\pi}$ se define como $\mathbf{(A_1-I}$.

El rango de $\mathbf{(A_1-I}$ es igual al número de vectores de cointegración. Por analogía con el caso univariado, si $\mathbf{(A_1-I}$  tiene solo ceros, de modo que el $rango(\pi)=0$, todas las secuencias { $x_{it}$ } tienen raíz unitaria. En esta situación, dado que no hay una combinación lineal de los procesos { $x_{it}$ } que sea estacionaria, las variables no se cointegran. Descartando la presencia de raíces características mayores que $1$ y si el $rango(\pi)=n$, $\mathbf{\Delta x_t=\pi x_{t-1} + \varepsilon_t}$ es un sistema convergente de ecuaciones en diferencias, de modo que todas las variables son estacionarias.

Hay varias formas de generalizar $\mathbf{\Delta x_t=\pi x_{t-1}+\varepsilon_t}$. Por ejemplo, la ecuación se modifica fácilmente si se considera la presencia de un intercepto; simplemente asuma $\mathbf{\Delta x_t=A_0 \pi x_{t-1}+\varepsilon_t}$ donde $$ \mathbf{A_0}={\left\lbrack \matrix{ a_{10} \cr a_{20} \cr \dots \cr a_{n0} } \right\rbrack}. El efecto de incluir los diversos $a_{i0}$ es permitir la posibilidad de una tendencia lineal en el proceso de generación de datos. Lo ideal es incluir el intercepto si las variables exhiben una tendencia decidida a aumentar o a disminuir. Aquí, el rango de $\pi$ se puede ver como el número de relaciones de cointegración existentes en los datos sin tendencia. En el largo plazo, $\mathbf{\pi x_{t-1}=0}$ tal que el valor esperado de cada secuencia { $\Delta x_{it}$ } es $a_{i0}$. Al agregar todos estos cambios en el tiempo se obtiene la expresión determinista $a_{i0}t$.

Las figuras de abajo ilustran los efectos de incluir un intercepto en el proceso de generación de datos. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/61152b61-d2a2-4dab-8f67-133f11dde731)

En las figuras, se generaron dos secuencias aleatorias, { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ }, con $100$ observaciones cada una. Por otro lado, se asumió $y_0=z_0=0$, y que los siguientes $100$ valores de las secuencias { $y_t$ } y { $z_t$ } eran

de modo que la relación de cointegración es $y_t=z_t$

* En la figura de la izquierda, puede ver que cada secuencia se asemeja a un proceso aleatorio y que ninguno se aleja demasiado del otro.
* En la figura del centro se agregan interceptor de manera que $a_{10}=a_{20}=0.1$; ahora cada serie tiende a aumentar $0.1$ unidades en cada período. Además del hecho de que cada secuencia comparte la misma tendencia estocástica, tenga en cuenta que cada una también tiene la misma tendencia de tiempo determinista. El hecho de que cada uno tenga la misma tendencia determinista no es el resultado de la equivalencia entre $a_{10}$ y $a_{20}$; ya que $y_t$ y $z_t$ están cointegrados, la solución general a $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ requiere que cada uno tenga la misma tendencia lineal.
* Para verificar esto, la figura de la derecha establece $a_{10}=0.1$ y $a_{20}=0.4$. De nuevo, las secuencias tienen las mismas tendencias estocásticas y deterministas.

A continuación veremos que manipulando apropiadamente los elementos de $A_0$ es posible incluir una constante en los vectores de cointegración sin necesidad de in-cluir una tendencia de tiempo determinista al sistema.

Una forma de incluir una constante en las relaciones de cointegración es restringir los valores de los diversos $a_i0$. Por ejemplo, si $rango(\pi)=1$, las filas de $\pi$ pueden diferir sólo en un escalar, por lo que es posible escribir cada secuencia { $\Delta x_{it}$ } en $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)}) + a_{10} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)}) + a_{20} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)}) + a_{n0} + \varepsilon_{nt}$

donde $s_i$ es un escalar tal que $s_i \pi_{1j}= \pi_{ij}$  

Si $a_{i0}$ se puede restringir tal que $a_i0=s_ia_{10}$, todas las secuencias { $\Delta x_{it}$ } se pueden escribir con la constante incluida en el vector de cointegración:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + a_{10}) + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + a_{20}) + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + a_{20}) + \varepsilon_{nt}$

o en la forma compacta $\mathbf{\Delta x_t= A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ donde 

$\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$, 
$\eqalign{\mathbf{x_{t-1}^*} = {\left\lbrack \matrix{x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack}}$,     y 

$\eqalign{\mathbf{ \pi* } = {\left\lbrack \matrix{\pi_{11} & \pi_{12} & \dots & \pi_{1n} \cr \pi_{21} & \pi_{22} & \dots & \pi_{2n} \cr \dots & \dots & \dots & \dots \cr \pi_{n1} & \pi_{n2} & \dots & \pi_{nn} } \right\rbrack}}$ 

La característica interesante de $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ es que la tendencia lineal se elimina del sistema. En esencia, los diversos $a_io$ se han modificado de tal manera que la solución general para cada { $x_it$ } no contiene una tendencia temporal. La solución al conjunto de ecuaciones en diferencias representadas $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ por es tal que se espera que todos los $Delta x_{it}$ sean iguales a cero cuando $\pi_{11} x_{1(t-1)} + \pi_{12} x_{2(t-1)} + \dots + \pi_{1n} x_{n(t-1)} + a_{i0} =0$.

Para resaltar la diferencia entre $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ y $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + varepsilon_t}$, la figura de abajo ilustra las consecuencias de utilizar $a_10=0.1$ y $a_20=-0.1$.  

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/217d2e36-944c-493c-8cab-b45d1ff27ff6)

Se puede ver que ninguna secuencia contiene una tendencia determinista. De hecho, para los datos que se muestran en la figura, la tendencia desaparecerá siempre que seleccionemos valores de los interceptos manteniendo la relación $a_10=-a_20$.

**Algunos econometristas prefieren incluir un intercepto en el vector de cointegración junto con un intercepto fuera de éste.** Esto tiene sentido si las variables contienen un intercepto y si la teoría económica sugiere que el vector de cointegración contiene un intercepto. Sin embargo, debe quedar claro que el intercepto en el vector de cointegración no se identifica en presencia de un intercepto fuera de este. Después de todo, alguna parte del intercepto no restringido siempre puede incluirse en el vector de cointegración. 

En términos del ejemplo anterior, el sistema siempre puede escribirse como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + b_{10}) + b_{11} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + b_{20}) + b_{21} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + b_{n0}) + b_{n1} + \varepsilon_{nt}$

donde  $s_i b_{10} + b_{11}= a_{10}$  

Todo lo que se hace es dividir $a_{10}$ en dos partes y colocar una parte dentro de la relación de cointegración. Es necesaria alguna estrategia de identificación ya que la proporción del intercepto a incluir en el vector de cointegración es arbitraria. 

De los gráficos previos, note que es necesario una constante fuera de la relación de cointegración para capturar los efectos de una tendencia sostenida de las variables a aumentar (o a disminuir). La mayoría de los investigadores incluyen interceptos si los datos se asemejan a los gráficos donde ambos interceptos eran 0.1 o donde los interceptos eram 0.1 y 0.4. De lo contrario, incluyen interceptos en el vector de cointegración o excluyen por completo a los regresores deterministas. Si no está seguro, puede usar los métodos que se describen más adelante para probar si las desviaciones se pueden restringir adecuadamente. R permite incluir una tendencia de tiempo determinista en el modelo. Claro que es mejor evitar el uso de la tendencia como variable explicativa a menos que tenga una buena razón para incluirla en el modelo. Johansen (1994) discute el papel de los regresores deterministas en una relación de cointegración.

Al igual que con la prueba $ADF$, el modelo multivariado también puede generalizarse para permitir un proceso autorregresivo de orden superior. 

Considere $x_t = A_1x_{t-1} + A_2x_{t-2}  + \dots +  A_px_{t-p} + \varepsilon_t$ donde $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$ y $\mathbf{\varepsilon_t}$ es un vector n-dimensional distribuido de forma independiente e idéntica con media cero y matriz de varianza $\Sigma_\varepsilon$.

Como se hizo con la prueba $ADF$ en el modelo univariado, con las sumas apropiadas, la ecuación $\mathbf{x_t= \displaystyle\sum_{i=1}^{p} A_i x_{t-i} + \varepsilon_t}$ se puede transformar en: $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ donde $\mathbf{\pi = -(I-\displaystyle\sum_{i=1}^p A_i)}$ y $\mathbf{\pi_i = -(I-\displaystyle\sum_{j=i+1}^p A_j)}$ 

La característica clave a tener en esta nueva ecuación es el rango de la matriz $\mathbf{\pi}$ ; **el rango de $\mathbf{\pi}$ es igual al número de vectores cointegrantes independientes**:

* si $\mathbf{rango(\pi)=0}$, la matriz es nula y $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ es el modelo $VAR$ en las primeras diferencias.
* si $\mathbf{rango(\pi)=n}$, el proceso vectorial es estacionario.
* si $\mathbf{rango(\pi)=1}$, hay un solo vector de cointegración y la expresión $\mathbf{\pi x_{t-1}}$ es el término de corrección de errores.
* si $\mathbf{1 \lt rango(\pi) \lt n}$, hay múltiples vectores de cointegración.

El número de distintos vectores de cointegración se puede obtener al verificar la significancia de las raíces características de $\mathbf{\pi}$. 
Sabemos que el rango de una matriz es igual al número de sus raíces características que difieren de cero. Suponga que se obtiene la matriz $\mathbf{\pi}$ y se ordenan las $n$ raíces características de forma que $\lambda_1>\lambda_2>\dots>\lambda_n$. 

Si las variables en $\mathbf{x_t}$ no están cointegradas, $\mathbf{rango(\pi)=0}$ y todas las raíces características serán iguales a cero. Como $\ln{(1)}=0$, entonces cada una de las expresiones $\ln{(1 - \lambda_i)}$ será igual a cero si las variables no están cointegradas. 

De manera similar, si $\mathbf{rango(\pi)=1}$, $0 < 𝜆_1 < 1$, entonces $\ln{(1 - \lambda_1)} < 0$ y $\ln{(1 - \lambda_2)} = \ln{(1 - \lambda_3)} = \dots = \ln{(1 - \lambda_n)} = 0$.

En la práctica, solo podemos obtener estimaciones de $\mathbf{\pi}$ y de sus raíces características. La prueba para el número de raíces características que son significativamente diferentes de $1$ se puede realizar utilizando los siguientes dos estadísticos de prueba:

* $\lambda_{traza}(r)=-T\displaystyle\sum_{r+1}^n \ln{(1-\hat{\lambda_i})}$ 
* $\lambda_{max}(r,r+1)=-T\ln{(1-\hat{\lambda_{r+1}})}$

donde
* $\hat{\lambda_i}$ son los valores estimados de las raíces características (también llamados valores propios) obtenido de la matriz $\mathbf{\pi}$ estimada 
* $T$ es el número de observaciones utilizables

El estadístico $\lambda_{traza}$ prueba la hipótesis nula de que el número de diferentes vectores de cointegración es menor o igual que $r$ frente a una alterna-tiva general. Note que $\lambda_{traza}=0$ cuando todos los {\lambda_i=0}. Cuanto más lejos están las raíces características estimadas de cero, más negativo es $(1-\hat{\lambda_i})$  y más grande es el estadístico $\lambda_{traza}$. 

El estadístico $\lambda_{max}$ prueba la hipótesis nula de que el número de vectores de cointegración es $r$ frente a la alternativa de $r+1$ vectores de cointe-gración. Si el valor estimado de la raíz característica está cerca de cero, $\lambda_{max}$ será pequeño.

Los valores críticos de los estadísticos $\lambda_{traza}$  y $\lambda_{max}$ se obtienen utilizan-do el enfoque de Monte Carlo. La distribución de estos estadísticos depende de dos cosas:
1) El número de componentes no estacionarios bajo la hipótesis nula (es decir, $n-r$).
2) La forma del vector $A_0$. Es decir: 
    * si no incluye interceptos ni en la ecuación ni en el vector de cointegración.
    * si incluye un intercepto en la ecuación.
    * si incluye un intercepto en el vector de cointegración.

Es importante anotar que estos estadísticos necesitan que sus residuos sean ruido blanco. Cualquier evidencia de que los errores no son ruido blanco generalmente significa que las longitudes de rezago son demasiado cortas. 

En la prueba de Johansen, es importante determinar correctamente la forma de los regresores deterministas. Por ejemplo, los valores críticos de los estadísticos $\lambda_{traza}$ y $\lambda_{max}$ son más pequeños sin ningún tipo de regresores deterministas y más grandes con un intercepto en el vector de cointegración. 
En lugar de plantear con cautela la forma de $A_0$, es posible probar formas restringidas del vector [^*]. La idea clave de todas las pruebas de hipótesis es que **si hay $r$ vectores de cointegración, solo estas $r$ combinaciones lineales de las variables son estacionarias**. Todas las demás combinaciones lineales son no estacionarias. Por lo tanto, suponga que se reestima el modelo restringiendo los parámetros de $\pi$. Si las restricciones no son vinculantes, debe encontrar que el número de vectores de cointegración no ha disminuido. 

[^*]: Uno de los aspectos más interesantes del procedimiento de Johansen es que permite probar formas restringidas de los vectores de cointegración.

Para probar la presencia de un intercepto en el vector de cointegración en oposición al intercepto no restringido $A_0$, estime las dos formas del modelo. Denote 

* las raíces características ordenadas de la matriz no restringida $\pi$ por $\hat{\lambda_i},\dots,\hat{\lambda_n}$, y
* las raíces características del modelo con los interceptos en los vectores de cointegración por $\hat{\lambda_i^*},\dots,\hat{\lambda_n^*}$. 

Suponga que la forma no restringida del modelo tiene $r$ raíces características distintas de cero.  Asintóticamente, el estadístico

−𝑇∑_(𝑖=𝑟+1)^𝑛▒[ln⁡(1−𝜆 ̂_𝑖^∗ )−ln⁡(1−𝜆 ̂_𝑛 ) ]  tiene una distribución $\chi^2$ con $(n-r)$ grados de libertad. La intuición detrás de la prueba es que todos los valores de ln⁡(1−𝜆 ̂_𝑖^∗ ) y ln⁡(1−𝜆 ̂_𝑛 ) deben ser equivalentes si la restricción no es vinculante. Por lo tanto, valores pequeños del estadístico de prueba implican que está permitido incluir el intercepto en el vector de cointegración. Sin embargo, la probabilidad de encontrar una combinación lineal estacionaria de las 𝑛 variables es mayor con el intercepto en el vector de cointegración que si el intercepto está ausente de este. 









+++++++++++++++++++++

Utilice los siguientes cuatro pasos cuando implemente el procedimiento Johansen:

1) **Realice una prueba de todas las variables para evaluar su orden de integración.**

   Grafique los datos para ver si es probable que una tendencia de tiempo lineal esté presente en el proceso de generación de datos. Los resultados de la prueba pueden ser bastante sensibles a la longitud del rezago, por lo que es importante tener cuidado. El procedimiento más común es estimar $VAR$ de los datos no diferenciados. Luego use las mismas pruebas de longitud de rezagos que en un $VAR$ tradicional. Comience con el rezago mas largo que considere razonable y verifique si se puede acortar. Por ejemplo, si queremos probar si los rezagos $1$ a $4$ son importantes, podemos estimar los siguientes dos 𝑉𝐴𝑅:
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+A_2 x_{t-2}+A_3 x_{t-3}+A_4 x_{t-4}+e_{1t}}$
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+e_{2t}}$

donde 
   * $\mathbf{x_t}$ es un vector de variables ( $n\times 1$ ),
   * $\mathbf{A_0}$ es la matriz ( $n\times 1$ ) de interceptos,
   * $\mathbf{A_1}$ son matrices ( $n\times n$  de coeficientes, y
   * $\mathbf{e_{1t}}$ y $\mathbf{e_{2t}}$ son vectores ( $n\times 1$ ) de términos de error.
   
   Calcule el primer sistema con cuatro rezagos de cada variable en cada ecuación y llame a la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_4}$. Ahora estime la segunda ecuación usando solo un rezago de cada variable en cada ecuación y llame a la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_1}$. 

Aunque trabajamos con variables no estacionarias, podemos realizar pruebas de longitud de rezagos utilizando el estadístico de prueba de razón de verosi-militud recomendado por Sims (1980): $(T-c)(\ln{|\mathbf{\Sigma_1}|}-\ln{|\mathbf{\Sigma_4}|})$ donde 
* $T$ es el número de observaciones,
* $c" es el número de parámetros en el sistema no restringido y
* $ln{|\mathbf{\Sigma_i}|}$ es el logaritmo natural del determinante de $\mathbf{\Sigma_i}$.

Siguiendo a Sims, use la distribución $\chi^2$ con grados de libertad igual al número de restricciones de los coeficientes. Como cada $\mathbf{A_i}$ tiene $n^2$ coeficientes, la restricción $\mathbf{A_2=A_3=A_4=0}$  implica restricciones $3n^2$. 

Alternativamente, puede seleccionar la longitud del rezago $p$ utilizando las generalizaciones multivariadas del Criterio de Información de Akaike o del Criterio Bayesiano de Schwartz. En el modelo en cuestión, puede darse por ejemplo que el método general a específico y el Criterio de Información de Akaike seleccionan una longitud de rezago de $2$, mientras que el Criterio Bayesiano de Schwartz selecciona una longitud de rezago de $1$. 

2) **Estime el modelo y determine el rango de $\pi$.**

   Muchos paquetes de software econométrico contienen una rutina para estimar el modelo. Aquí, basta con decir que Mínimos Cuadrados Ordinarios no es apropiado porque es necesario imponer restricciones de ecuaciones cruzadas en la matriz. En la mayoría de los casos, puede elegir estimar el modelo en tres formas:
   
   i) con todos los elementos de $\mathbf{A_0}$ establecidos en cero,
   
   ii) con una constante en la ecuación, o
   
   iii) con un término constante en el vector de cointegración. 

3) **Analice el(los) vector(es) de cointegración normalizados y la velocidad de los coeficientes de ajuste.**

   Por ejemplo, asuma que si seleccionamos $r=1$, el vector estimado de cointegración $(\beta_0,\beta_1,\beta_2,\beta_3)$ es $\mathbf{\beta_t}=(0.00553, 0.41532, 0.42988, −0.42207)$. Normalizando con respecto a $\beta_1$, el vector de cointegración normalizado es $\mathbf{\beta_t}=(−0.01331, −1.0000, −1.0350, 1.0162)$.

   Asuma que los valores teóricos del vector de cointegración son $(0,−1,−1, 1)$. En consecuencia, considere hacer las siguientes pruebas:
   a) La prueba de $\beta_0=0$ implica una restricción en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con un grado de libertad.
   b) Para restringir el vector de cointegración normalizado tal que $\beta_2=-1$ y $\beta_3=-1$ implica dos restricciones en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con dos grados de libertad.
   c) Para probar la restricción conjunta $\mathbf{\beta_t}=(0,−1,−1, 1)$ implica las tres restricciones $\beta_0=0$, $\beta_2=-1$ y $\beta_3=-1$.

4) Finalmente, **las pruebas de impulso-respuesta, descomposición de varianza y causalidad en el modelo de corrección de errores** podrían ayudar a identificar un modelo estructural y determinar si el modelo estimado parece ser razonable.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>














