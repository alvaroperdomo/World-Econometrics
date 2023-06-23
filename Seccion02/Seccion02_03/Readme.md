# Análisis ARIMA (Metodología de Box y Jenkins)

Box-Jenkins (1976) popularizaron un método de tres etapas para seleccionar el modelo apropiado con el fin de estimar y pronosticar una serie de tiempo univariada:

1. **Identificación:** se examina visualmente el gráfico de tiempo de la serie, la función de autocorrelación (_FAC_) y la función de autocorrelación parcial (_FACP_).

La trayectoria temporal de la secuencia { $y_t$ }  proporciona información sobre:
* valores atípicos,
* valores faltantes y
* cambios estructurales en los datos.

Las variables no estacionarias pueden tener una tendencia pronunciada o parecer serpentear sin una media o varianza constante a largo plazo. Los valores faltantes y los valores atípicos se pueden corregir en este punto. 

2. **Estimación:** cada uno de los modelos tentativos se ajusta, y se examinan los diversos coeficientes $a_i$ y $\beta_i$ 𝛽_𝑖. El objetivo es seleccionar un modelo estacionario y parsimonioso que se ajuste bien
3. **Verificación de diagnóstico:** para garantizar que los residuos del modelo estimado imiten un proceso de ruido blanco.

Una comparación de las _FAC_ y las _FACP_ muestrales con las de varios procesos _ARMA_ teóricos puede sugerir varios modelos plausibles. Por medio de dos ejemplos sencillos, vamos a mostrar cómo se identifica el proceso generador de una variable.

### Estimación de un modelo AR(1)
Utilizemos un ejemplo específico para ver cómo la función de autocorrelación muestral (_FAC_) y la función de autocorrelación parcial muestral (_FACP_) se pueden utilizar como ayuda para identificar un modelo _ARMA_. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Tenga en cuenta que el problema de no estacionariedad se evita ya que la condición inicial es consistente con el equilibrio a largo plazo. 

Las figuras de abajo muestran la _FAC_ y la _FACP_ muestral. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d051567c-1c96-4305-9a5b-f2f9340192da)

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $100$ valores muestrales de la figura de arriba y se nos pide descubrir el verdadero proceso utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la _FAC_ muestral y la _FACP_ muestral con las de los diversos modelos teóricos. 

#### La parte autorregresiva (AR) del proceso generador de datos (las _FACP_)
El único pico grande que se observa en el rezago $1$ de la _FACP_ sugiere un modelo _AR(1)_.

##### Sin embargo, si no conociéramos el verdadero proceso subyacente y estuvieramos utilizando datos mensuales, podríamos preocuparnos por la autocorrelación parcial significativa en el rezago _12_. Después de todo, con los datos mensuales, podríamos esperar alguna relación directa entre $y_t$ y $y_{t-12}$.

El pico es de $0.74$ en el rezago $1$, y todas las otras _FACP_ (excepto la del rezago $12$) son muy pequeñas. 

A partir de la figura de las _FACP_, se puede ver que todas las _FACP_ a excepción de la $\phi_{11}$ (y del desfase en el rezago $12$) son menores que $\frac{2}{\sqrt{}T}=0.2$ . 

#### La parte de media móvil (MA) del proceso generador de datos (las _FAC_)
Las _FAC_ revelan una decaimieto progresivo (por ejemplo, las primeras tres _FAC_ son $r_1=0.74$, $r_2=0.58$ y $r_3=0.47$.) que no dan la idea de un proceso _MA_. 

#### Análisis de la conjetura

Aunque sabemos que los datos se generaron en realidad a partir de un proceso de _AR(1)_, es ilustrativo comparar las estimaciones de dos modelos diferentes. Suponga que estimamos un modelo _AR(1)_ e intentamos capturar el pico en el desfase _12_ con un coeficiente _MA_. Por lo tanto, consideramos los dos modelos tentativos:

* **Modelo 1 - _AR(1)_:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - _ARMA(1,12)_:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{12}\varepsilon_{t-12} $

La tabla de abajo informa los resultados de las dos estimaciones

| Indicadores                               | Modelo 1       | Modelo 2       | 
|-------------------------------------------|:--------------:|:--------------:|
| Grados de Libertad                        |98              |97              | 
| SRC (Suma de Residuos al Cuadrado)        |85.10           |85.07           | 
| $\hat{a_1}$  <br> (Error estándar)        |0.7904 <br> (0.0624) |0.7938 <br> (0.0643) | 
| $\hat{\beta_{12}}$  <br> (Error estándar) |                |-0.0325 <br> (0.11)  | 
| Criterio de Información de Akaike         |441.9           |443.9           | 
| Criterio Bayesianode Schwartz             |444.5           |449.1           | 
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis)      |Q(8) = 6.43 (0.490)   <br> Q(16) = 15.86 (0.391)    <br> Q(24) = 21.74 (0.536) |Q(8) = 6.48 (0.485)  <br>  Q(16) = 15.75 (0.400)  <br>   Q(24) = 21.56 (0.547)          | 

El coeficiente del Modelo 1 satisface la condición de estabilidad $|a_1|<0$ y tiene un error estándar bajo (el estadístico t asociada es mayor que $12$). Como una verificación de diagnóstico útil, dibujamos el correlograma de los residuos del modelo ajustado en la figura de abajo. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/bf030c21-940a-4842-9971-5a42410f7b52)

Los estadísticos _Q_ de los residuos indican que:
* cada una de las autocorrelaciones es menor que dos desviaciones estándar de cero.
* como grupo, los rezagos $1$ a $8$, $1$ a $16$ y $1$ a $24$ no son significativamente diferentes de cero. 

Esta es una fuerte evidencia de que el modelo _AR(1)_ se ajusta bien con los datos. Después de todo, si las autocorrelaciones de los residuos fueran significativas, el modelo _AR(1)_ no usaría toda la información disponible sobre los movimientos en la secuencia { $y_t$ }

Al examinar los resultados para el Modelo 2, observe que ambos modelos arrojan estimaciones similares para :
* el coeficiente autorregresivo de primer orden y
* el error estándar asociado. 

Sin embargo, 
* la estimación para $\beta_{12}\$ es de mala calidad;
* el valor del estadístico t no es significativo y sugiere que debería eliminarse del modelo.
  
Además, la comparación de los valores del Criterio de Información de Akaike y del Criterio Bayesianode Schwartz de los dos modelos sugiere que los beneficios de reducir la Suma de Residuos al Cuadrado se ven superados por los efectos perjudiciales de la estimación de un parámetro adicional. 

##### Todos estos indicadores apuntan a la elección del Modelo 1.

### Estimación de un modelo ARMA(1,1)

Utilizemos un segundo ejemplo para ver cómo la función de autocorrelación muestral (_FAC_) y la función de autocorrelación parcial muestral (_FACP_) sirven para identificar un modelo _ARMA(1,1)_. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=-0.7y_{t-1}+\varepsilon_t+0.7\varepsilon_{t-1}$ y la condición inicial $y_0=0$ y $\varepsilon_0=0$. 

Las figuras de abajo muestran la _FAC_ y la _FACP_ muestral. Estos nos dan idea de un _ARMA(1,1)_.


![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/49d0c43b-c2a0-4f54-a68c-baa66931aafd)

Sin embargo, si se desconoce el verdadero proceso de generación de datos, uno podría tener ciertas dudas acerca del modelo real. 

Entonces, se pueden analizar diferentes modelos como los siguientes:

* **Modelo 1 - _AR(1)_:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - _ARMA(1,1)_:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{12}\varepsilon_{t-1}$
* **Modelo 3 - _ARMA(2)_:** $y_t=a_1y_{t-1}+a_2y_{t-2}+\varepsilon_t$

| Indicadores                               | Modelo 1           | Modelo 2           |Modelo 3            |
|-------------------------------------------|:------------------:|:------------------:|:------------------:|
| $\hat{a_1}$  <br> (Error estándar)        |-0.835 <br> (0.053) |-0.679 <br> (0.076) |-1.160 <br> (0.093) | 
| $\hat{a_2}$  <br> (Error estándar)        |                    |                    |-0.378 <br> (0.092) |
| $\hat{\beta_1}$  <br> (Error estándar)    |                    |-0.676 <br> (0.081) |                    |
| Criterio de Información de Akaike         |496.5               |471.0               |482.8               |  
| Criterio Bayesiano de Schwartz            |499.0               |476.2               |487.9               |  
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis)      |Q(8) = 3.86 (0.695)   <br> Q(24) = 14.23 (0.892) | Q(8) = 26.19 (0.000)   <br> Q(24) = 41.10 (0.001) | Q(8) = 11.44 (0.057)   <br> Q(24) = 22.59 (0.424) | 

Al examinar la tabla, note que todos los $\hat{a_1}$  son muy significativos; cada uno de los valores estimados se desvía al menos ocho desviaciones estándar de cero. Está claro que el modelo _AR(1)_ es inapropiado. Los estadísticos Q para el Modelo 1 indican que existe una autocorrelación significativa en los residuos. El modelo estimado _ARMA(1,1)_ no sufre este problema. Además, tanto el Criterio de Información de Akaike como el Criterio Bayesiano de Schwartz seleccionan el Modelo 2 sobre el Modelo 1.

El mismo tipo de razonamiento indica que el Modelo 2 es preferible al Modelo 3. Tenga en cuenta que, para cada modelo, los coeficientes estimados son altamente significativos y las estimaciones puntuales implican convergencia. Aunque el estadístico Q en $24$ rezagos indica que estos dos modelos no tienen residuos correlacionados, el estadístico Q de $8$ rezagos indica una correlación serial en los residuos del Modelo 3. Por lo tanto, el modelo _AR(2)_ no capta la dinámica de corto plazo como lo hace el modelo _ARMA(1,1)_.  También tenga en cuenta que tanto el Criterio de Información de Akaike como el Criterio Bayesiano de Schwartz seleccionan el Modelo 2.

Una idea fundamental en el enfoque de Box-Jenkins es el principio de parsimonia. La incorporación de coeficientes adicionales aumenta necesariamente el ajuste del modelo (por ejemplo, el valor del $R^2$ aumenta) pero al costo de reducir los grados de libertad. 

Box y Jenkins argumentan que los modelos parsimoniosos producen mejores pro-nósticos que los modelos sobreparametrizados. Un modelo parsimonioso se adapta bien a los datos sin incorporar ningún coeficiente innecesario. 

Un buen modelo se ajustará bien a los datos. 

El $R^2$ y el promedio de la Suma de los Residuos al Cuadrado son medidas comunes de bondad de ajuste en estimaciones de Mínimos Cuadrados Ordinarios. El problema con estas medidas es que el ajuste necesariamente mejora a medida que se incluyen más parámetros en el modelo. 

La parsimonia sugiere usar el Criterio de Información de Akaike y/o el Criterio Bayesiano de Schwartz como medidas más apropiadas del ajuste general del modelo. 

Además, tenga cuidado con las estimaciones que no convergen rápidamente. 

La mayoría de los paquetes econométricos estiman los parámetros de un modelo _ARMA_ utilizando un procedimiento de búsqueda no lineal. Si la búsqueda no logra converger rápidamente, es posible que los parámetros estimados sean inestables. 

En tales circunstancias, agregar una o dos observaciones adicionales puede alterar en gran medida las estimaciones.



