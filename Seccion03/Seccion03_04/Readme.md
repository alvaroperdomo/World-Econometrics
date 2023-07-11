# Cointegración

En los modelos univariados una tendencia estocástica puede eliminarse por diferenciación, las series estacionarias resultantes se pueden estimar utilizando las técnicas univariadas de Box-Jenkins. Anteriormente, la práctica convencional era generalizar esta idea a los modelos multivariados y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión para luego estimar un modelo VAR en diferencias. Sin embargo, hoy en día la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria y que por lo tanto configure una relación de largo plazo[^1]; si esto ocurre se dice que tales variables están **cointegradas**. 

[^1]: **En cierto sentido, el uso del término equilibrio es desafortunado porque los teóricos económicos y los econometristas usan el término de diferentes maneras. En teoría económica usualmente se usa el término para referirse a una igualdad entre transacciones deseadas y reales. El uso econométrico del término hace referencia a cualquier relación a largo plazo entre variables no estacionarias. La cointegración no requiere que la relación a largo plazo sea generada por las fuerzas del mercado o por las reglas de comportamiento de los individuos. Según Engle y Granger (1987) la relación de equilibrio puede ser causal, conductual o sim-plemente una relación de forma reducida entre variables con tendencias similares.** 

Para entender mejor lo que es cointegración considere el siguiente ejemplo: Asuma que hay $n$ variables $x_{1t}, x_{2t}, \dots, x_{nt}$ integradas del mismo orden que conforman un equilibrio de largo plazo representado por $\eqalign{\sum_{i=1}^n \beta_i x_{it}=0}$. Es decir, $\mathbf{B x_t}=0$ donde $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ y 

$$ 
\mathbf{x_t} = \left[\begin{array}{ccc} 
x_{1t} \\ 
x_{2t} \\ 
\dots \\ 
x_{nt}  
\end{array}\right] 
$$ 

La desviación del equilibrio a largo plazo, llamado error de equilibrio es $e_t$, por lo tanto $e_t=\mathbf{\beta x_t}$. **Si el equilibrio es significativo, $e_t$ debe ser estacionario**. 

Engle y Granger (1987) proporcionan la siguiente definición de cointegración: Se dice que los componentes del vector 

$$ 
\mathbf{x_t} = \left[\begin{array}{ccc} 
x_{1t} \\ 
x_{2t} \\ 
\dots \\ 
x_{nt}  
\end{array}\right] 
$$ 

son cointegrados de orden $d,b$, denotado por $\mathbf{x_t}\sim CI(d,b)$  si 
1) Todos los componentes de $\mathbf{x_t}$ son integrados de orden $d$.
2) Existe un vector $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ tal que la combinación lineal $\mathbf{\beta x_t}=0$  está integrada de orden $(d-b)$ donde $b>0$.

  El vector $B$ se llama el vector cointegrante.

Hay cuatro puntos importantes a tener en cuenta sobre la definición:
1) **La cointegración se refiere a una combinación lineal de variables no estacionarias**.

   El vector de cointegración no es único. Si ${\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ es un vector de cointegración, entonces para cualquier $\lambda \not= 0$, ${\left\lbrack \matrix{\lambda\beta_1 & \lambda\beta_2 & \dots & \lambda\beta_n} \right\rbrack}$ también es un vector de cointegración.

   Normalmente, una de las variables se usa para normalizar el vector de cointegración fijando su coeficiente en $1$. Por ejemplo, para normalizar el vector de cointegración con respecto a $x_{1t}$, simplemente se selecciona un $\lambda=\frac{1}{\beta_1}$.

2) **La cointegración se refiere a variables que están integradas en el mismo orden**.

   Esto no implica que todas las variables integradas estén cointegradas; por lo general, un conjunto de variables $I(d)$ no está cointegrado.[^3] 

   **Si dos variables son integradas de órdenes diferentes, no pueden estar cointegradas.**

   Suponga que $x_{1t}$ es $I(d_1)$ y $x_{2t}$ es $I(d_2)$ donde $d_2>d_1$ . Entonces, cualquier combinación lineal de $x_{1t}$ con $x_{2t}$ es $I(d_2)$.

   Sin embargo, **es posible encontrar relaciones de equilibrio entre grupos de variables que están integradas de diferentes órdenes.**

   Suponga que $x_{1t}$ y $x_{2t}$ son $I(2)$ y que las otras variables en consideración son $I(1)$. Como tal, no puede haber una relación de cointegración entre $x_{1t}$ (o $x_{2t}$) y $x_{3t}$. Sin embargo, si $x_{1t}$ y $x_{2t}$ son $CI(2,1)$, existe una combinación lineal de la forma $\beta_1x_{1t}+\beta_2x_{2t}$ que es $I(1)$, la cual es posible que esté cointegrada con las variables $I(1)$.

[^3]: Tal falta de cointegración implica que no hay un equilibrio a largo plazo entre las variables, de modo que puedan desviarse arbitrariamente una de la otra.

3) **Puede haber más de un vector de cointegración independiente para un conjunto de variables $I(1)$.**[^4]

   Si $\mathbf{x_t}$ tiene $n$ componentes no estacionarios, puede haber hasta $n-1$ vectores de cointegración linealmente independientes. Por lo tanto, si $\mathbf{x_t}$ contiene solo dos variables, puede haber a lo sumo un vector de cointegración independiente.

[^4]: El número de vectores de cointegración se denomina rango de cointegración del vector.

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

   En la mayoría de los estudios aplicados, no es posible utilizar las tablas de Dickey-Fuller. El problema es que la secuencia { $\hat{e_t}$ } se genera a partir de una regresión; donde el investigador no conoce el error real $e_t$, solo el error estimado { $\hat{e_t}$ }.[^5]

[^5]: **Engle y Granger (1987) propusieron nuevas tablas para hacer los cálculos. Y estas posteriormente fueron actualizadas por MacKinnon (1990). En estas tablas, los valores críticos dependen del tamaño de la muestra y del número de variables utilizadas en el análisis**

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

   Dado que la estructura del molelo $VEC$ es la de un $VAR$, entonces puede estimarse utilizando la misma metodología desarrollada en la presentación de los modelos $VAR$. Dado que todas las variables en $i$ y $ii$ son estacionarias (es decir, $\Delta y_t$ y sus rezagos, $\Delta z_t$ y sus rezagos, y $\hat{e_{t-1}}$ son $I(0)$), los estadísticos de prueba utilizados en el análisis $VAR$ tradicional son apropiados para $i$ y $ii$. 

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

Consideremos la generalización al caso simple con 𝑛 variables; asuma que el vector $x_t$ de $n$ variables, se comporta como $x_t=A_1x_{t-1}+\varepsilon_t$  así que $\Delta x_t=A_1x_{t-1}-x_{t-1}+\varepsilon_t=(A_1-I)x_{t-1}+\varepsilon_t=\pi x_t-1+\varepsilon_t$ donde 
* $\varepsilon_t$ es un vector ( $n\times 1$ ),
* $A_1$ es una matriz ( $n\times n$ ) de parámetros, 
* $I$ es una matriz identidad ( $n\times n$ ), 
𝜋 se define como (𝐴_1−𝐼).
El rango de (𝐴_1−𝐼) es igual al número de vectores de cointegración. 
Por analogía con el caso univariado, si (𝐴_1−𝐼)  tiene solo ceros, de modo que el 𝑟𝑎𝑛𝑔𝑜(𝜋)=0, todas las secuencias {𝑥_𝑖𝑡} son raíz unitaria. 
En esta situación, dado que no hay una combinación lineal de los procesos {𝑥_𝑖𝑡} que sea estacio-naria, las variables no se cointegran. 
Descartando la presencia de raíces características mayores que 1 y si el 𝑟𝑎𝑛𝑔𝑜(𝜋)=𝑛, ∆𝑥_𝑡=𝜋𝑥_(𝑡−1)+𝜀_𝑡 es un sistema convergente de ecuacio-nes en diferencias, de modo que todas las variables son estacionarias.












