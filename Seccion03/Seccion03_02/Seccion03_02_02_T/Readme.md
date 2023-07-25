<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.2.
# La Metodología de Engle y Granger

Para explicar el procedimiento de prueba de Engle-Granger, consideremos la clase de problema que es usual en los estudios aplicados. Suponga que dos variables, digamos $y_t$ y $z_t$, son integradas de orden $1$ y se desea determinar si existe una relación de equilibrio entre las dos. Para ello, Engle y Granger (1987) proponen el siguiente procedimiento de tres pasos para determinar si dos variables $I(1)$ están cointegradas de orden $CI(1,1)$:

### 1) Haga una prueba preliminar de las variables para conocer su orden de integración.
   
   Por definición, la cointegración requiere que dos variables estén integradas en el mismo orden. Por lo tanto, el primer paso en el análisis es determinar para cada variable su orden de integración.[^1] 
   
Recuerde que:
   * Si ambas variables son estacionarias, no es necesario continuar porque el método $VAR$ se aplica a las variables estacionarias.
   * Si las variables están integradas de diferentes órdenes, se concluye que no están cointegradas.
   * Sin embargo, si tiene más de dos variables, de manera que algunas son $I(1)$ y otras son $I(2)$, es posible que desee determinar si las variables están cointegradas en forma múltiple.

[^1]: **De la sección 2, recuerde que para hallar el orden de integración de una variable tiene que hacer pruebas de raíz unitaria. Si la variable en niveles no tiene raíz unitaria entonces es _I(0)_ (es decir, es estacionaria); si tiene raíz unitaria en niveles, pero se convierte en estacionaria al sacarle la primera diferencia es _I(1)_; si tiene raíz unitaria en niveles y al sacarle la primera diferencia, pero se convierte en estacionaria al sacarle la segunda diferencia es _I(2)_; y así en adelante**

### 2) Estime la relación del equilibrio a largo plazo.

   Si los resultados indican que tanto las secuencias { $y_t$ } como las secuencias { $z_t$ } son $I(1)$, estime utilizando Mínimos Cuadrados Ordinarios la relación de equilibrio: $y_t=\beta_0+\beta_1z_t+e_t$.

   Para determinar si las variables están realmente cointegradas, sea { $\hat{e_t}$ } la secuencia de residuos de esta ecuación. Por lo tanto, la serie { $\hat{e_t}$ } contiene los valores estimados de las desviaciones de la relación de largo plazo. Si se encuentra que estas desviaciones son estacionarias, las secuencias { $y_t$ } y { $z_t$ } son cointegradas de orden $(1,1)$. 

   Sería conveniente si se pudiera realizar una prueba $ADF$ de estos residuos para determinar su orden de integración. Considere la regresión de los residuos: $\Delta\hat{e}=a_1\hat{e_{t-1}}+\varepsilon_t$[^2] donde el parámetro de interés es $a_1$:

   * Si no se puede rechazar la hipótesis nula $a_1=0$, se concluye que los residuos tienen una raíz unitaria y por lo tanto, se deduce que las secuencias { $y_t$ } y { $z_t$ } no están cointegradas.
   * En cambio, el rechazo de la hipótesis nula $a_1=0$ implica que la secuencia de residuos es estacionaria. Si se encuentra que { $y_t$ } y { $z_t$ } son $I(1)$ y que los residuos son estacionarios, entonces se puede concluir que las series están cointegradas de orden $(1,1)$.

   Tenga en cuenta que en la mayoría de los estudios aplicados, no es posible utilizar las tablas de Dickey-Fuller para probar la hipótesis nula que acabamos de mencionar. El problema es que la secuencia { $\hat{e_t}$ } se genera a partir de una regresión; donde el investigador no conoce el error real $e_t$, solo el error estimado { $\hat{e_t}$ }. Por ello es que Engle y Granger (1987) propusieron nuevas tablas para hacer los cálculos [^3]. En estas tablas, los valores críticos dependen del tamaño de la muestra y del número de variables utilizadas en el análisis.

[^2]: **Los residuos estimados de una regresión tienen media igual a cero, por ello es que no es necesario incluir un intercepto dentro de esta regresión**
[^3]: **Estas tablas posteriormente fueron actualizadas por MacKinnon (1990).**

### 3) Estime el vector de corrección de errores

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

### 4) Evaluar la adecuación del modelo

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

## Inconvenientes con la Metodología de Engle-Granger
Aunque el procedimiento de Engle y Granger (1987) se implementa fácilmente, tiene varios defectos importantes. 

### 1) La estimación de la regresión de equilibrio a largo plazo requiere que el investigador coloque una variable en el lado izquierdo y utilice las otras variables como regresores. 

Por ejemplo, en el caso de dos variables, es posible hacer la prueba de cointegración de Engle-Granger utilizando los residuos de cualquiera de las siguientes dos regresiones de "equilibrio":

   * $y_t=\beta_{10}+\beta_{11}z_t+e_{1t}$  o
   * $y_t=\beta_{20}\beta_{21}y_t+e_{2t}$      

A medida que el tamaño de la muestra crece hacia infinito, la teoría asintótica indica que la prueba de raíz unitaria en la secuencia { $e_{1t}$ } se vuelve equivalente a la prueba de raíz unitaria en la secuencia { $e_{2t}$ } . Desafortunadamente, las propiedades de muestras grandes que obtienen este resultado pueden no ser aplicables a los tamaños de muestra generalmente disponibles. En la práctica, es posible encontrar que una regresión implique que las variables están cointegradas, mientras que revertir el orden implique que no hay cointegración. Esta es una característica muy indeseable del procedimiento porque la prueba de cointegración debe ser invariable a la elección de la variable seleccionada para la normalización. El problema se agrava utilizando tres o más variables, ya que cualquiera de las variables puede seleccionarse como la variable del lado izquierdo. Además, en las pruebas que usan tres o más variables, sabemos que puede haber más de un vector de cointegración. El método de Engle-Granger no tiene un proce-dimiento sistemático para la estimación separada de los múltiples vectores de coin-tegración

### 2) Otro defecto del procedimiento de Engle-Granger es que se basa en un estimador de dos pasos.

El primer paso es generar la serie de residuos $\hat{e_t}$, y el segundo paso utiliza estos errores generados para estimar una regresión de la forma $\Delta\hat{e_t}=a_1\Delta\hat{e_{t-1}}+\dots$ Entonces, el coeficiente $a_1$ se obtiene estimando una regresión que utiliza los residuos de otra regresión. Por lo tanto, cualquier error introducido por el investigador en el Paso $i$ se lleva al Paso $ii$. 


<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
