## SECCIÓN 2.3.2
# Estimación de un $ARMA$ - Ejemplos simulados 

Una comparación de la función de autocorrelación $FAC$ y la función de autocorrelación parcial $FACP$ muestrales con las de varios procesos $ARMA(p,q)$ teóricos puede sugerir varios modelos plausibles. Por medio de dos ejemplos sencillos, vamos a mostrar cómo se identifica el proceso $ARMA(p,q)$ generador de una variable.

## Estimación de un modelo AR(1)
En este ejemplo se generaron $100$ números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Note que la serie { $y_t$ } que se construye es estacionaria, por lo que es factible aplicar la metodología de Box-Jenkins. 

A continuación se van a desarrollar cada uno de los pasos de la metodología Box-Jenkins explicados en la sección 2.3.1. 

### Identificación
Las figuras de abajo muestran la $FAC$ y la $FACP$ muestrales de { $y_t$ } (en el eje vertical se muestran, respectivamente, el valor de las autocorrelaciones totales y parciales de la muestra para cada uno de los rezagos que se presentan en el eje horizontal). 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d051567c-1c96-4305-9a5b-f2f9340192da)

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $100$ valores muestrales que originaron las dos figuras de arriba y se nos pide descubrir el verdadero proceso $ARMA(p,q)$ utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la $FAC$ y la $FACP$ muestrales con las de los diversos modelos teóricos. 

#### La parte autorregresiva ($AR$) del proceso generador de datos
La $FACP$ se utiliza para determinar el componente $AR(p)$ del $ARMA(p,q)$. Identifique los rezagos de la $FACP$, del rezago $1$ en adelante, que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{100}}=\frac{2}{10}=0.2$ [^1]. Note que el pico más grande, por mucho, corresponde al rezago $1$ y vale $0.74$. esto sugiere un potencial compomemte $AR(1)$ dentro del modelo $ARMA(p,q)$. Todas las otras $FACP$ (excepto la del rezago $12$) son muy pequeñas[^2]

[^1]: **Dentro del análisis no se toma en consideración el rezago _0_ porque la autocorrelación parcial en este rezego por definición siempre es igual a $1$.** 
[^2]: **Si no conociéramos el verdadero proceso subyacente y estuvieramos utilizando datos mensuales, podría interesarnos incluir el componente AR(12). Después de todo, con los datos mensuales, cabe esperar alguna relación directa entre el valor de una variable y su valor en el mismo mes del año previo.**

#### La parte de media móvil ($MA$) del proceso generador de datos
La $FAC$ se utiliza para determinar el componente $MA(q)$ del $ARMA(p,q)$. Identifique los rezagos de la $FAC$, del rezago $1$ en adelante, que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{100}}=\frac{2}{10}=0.2$.[^3] No hay ningún rezago que se comporte así, lo que se observa es que las $FAC$ revelan una decaimieto progresivo (por ejemplo, los primeros tres rezagos de la $FAC$ son $0.74$, $0.58$ y $0.47$.). Por lo tanto, parece que no se genera un proceso $MA(q)$ dentro de nuestro $ARMA(p,q)$. 

[^3]: **Dentro del análisis no se toma en consideración el rezago _0_ porque la autocorrelación total en este rezego por definición siempre es igual a $1$.** 

### Estimación y verificación de diagnóstico
Aunque sabemos que los datos se generaron en realidad a partir de un proceso de $AR(1)$, es ilustrativo comparar las estimaciones de dos modelos diferentes. Suponga que estimamos un modelo $AR(1)$ e intentamos capturar el pico en el desfase $12$ con un coeficiente $MA$. Por lo tanto, consideramos los dos modelos tentativos:

* **Modelo 1 - $AR(1)$:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - $ARMA(1,12)$:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{12}\varepsilon_{t-12} $

La tabla de abajo informa los resultados de las dos estimaciones

| Indicadores                               | Modelo 1                | Modelo 2                | 
|-------------------------------------------|:-----------------------:|:-----------------------:|
| Grados de Libertad                        |$98$                     |$97$                     | 
| $\hat{a_1}$  <br> (Error estándar)        |$0.7904$ <br> ($0.0624)$ |$0.7938$ <br> ($0.0643$) | 
| $\hat{\beta_{12}}$  <br> (Error estándar) |                         |$-0.0325$ <br> ($0.11$)  | 
| Criterio de Información de Akaike         |$441.9$                  |$443.9$                  | 
| Criterio Bayesianode Schwartz             |$444.5$                  |$449.1$                  | 
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis) |$Q(8) = 6.43 (0.490)$ <br> $Q(16) = 15.86 (0.391)$ <br> $Q(24) = 21.74 (0.536)$ |$Q(8) = 6.48 (0.485)$ <br> $Q(16) = 15.75 (0.400)$ <br> $Q(24) = 21.56 (0.547)$ | 

**Análicemos el Modelo 1**

El coeficiente del **Modelo 1** satisface la condición de estabilidad $|\hat{a_1}|<0$ y tiene un error estándar bajo (es decir, menor a dos desviaciones estandar del valor de $|\hat{a_1}|$). Como una verificación de diagnóstico útil, en el gráfico de abajo se dibuja la $FAC$ de los residuos del modelo ajustado (es decir, los residuos del modelo cuando se estima $y_t=a_1y_{t-1}+\varepsilon_t$). Note que las barras de las autocorrelaciones de todos los rezagos superiores a $0$ son inferiores a $0.2$. Es decir, los residuos parecen ser ruido blanco. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/bf030c21-940a-4842-9971-5a42410f7b52)

Confirmando lo anterior, en la tabla de arriba, el estadistico $Q$ de los residuos del modelo ajustado a diferentes rezagos indica que como grupo, los rezagos $1$ a $8$, $1$ a $16$ y $1$ a $24$ no son significativamente diferentes de cero (es decir, el nivel de significancia, de cada uno de los estadísticos $Q$ es superior al 5%). Esto es  una fuerte evidencia de que el modelo $AR(1)$ se ajusta bien con los datos. Después de todo, si las autocorrelaciones de los residuos fueran significativas, el modelo $AR(1)$ no usaría toda la información disponible sobre los movimientos en la secuencia { $y_t$ }

**Análicemos el Modelo 2**

Al examinar los resultados para el **Modelo 2**, observe que arroja estimaciones similares a las del **Modelo 1** para :
* el coeficiente autorregresivo de primer orden $|\hat{a_1}|$ y
* el error estándar asociado. 

Sin embargo, 
* la estimación para $\hat{beta_{12}}$ es de mala calidad;
* el valor del estadístico t no es significativo y sugiere que debería eliminarse del modelo.
  
Además, la comparación de los valores del Criterio de Información de Akaike y del Criterio Bayesianode Schwartz de los dos modelos sugiere que los beneficios de reducir la Suma de Residuos al Cuadrado se ven superados por los efectos perjudiciales de la estimación de un parámetro adicional. 

##### Todos estos indicadores apuntan a la elección del Modelo 1.

### Estimación de un modelo $ARMA(1,1)$

Utilizemos un segundo ejemplo para ver cómo la función de autocorrelación muestral ($FAC$) y la función de autocorrelación parcial muestral ($FACP$) sirven para identificar un modelo $ARMA(1,1)$. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=-0.7y_{t-1}+\varepsilon_t+0.7\varepsilon_{t-1}$ y la condición inicial $y_0=0$ y $\varepsilon_0=0$. 

Las figuras de abajo muestran la $FAC$ y la $FACP$ muestral. Estos nos dan idea de un $ARMA(1,1)$.


![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/49d0c43b-c2a0-4f54-a68c-baa66931aafd)

Sin embargo, si se desconoce el verdadero proceso de generación de datos, uno podría tener ciertas dudas acerca del modelo real. 

Entonces, se pueden analizar diferentes modelos como los siguientes:

* **Modelo 1 - $AR(1)$:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - $ARMA(1,1)$:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{12}\varepsilon_{t-1}$
* **Modelo 3 - $ARMA(2)$:** $y_t=a_1y_{t-1}+a_2y_{t-2}+\varepsilon_t$

| Indicadores                               | Modelo 1               | Modelo 2               |Modelo 3                |
|-------------------------------------------|:----------------------:|:----------------------:|:----------------------:|
| $\hat{a_1}$  <br> (Error estándar)        |$-0.835$ <br> $(0.053)$ |$-0.679$ <br> $(0.076)$ |$-1.160$ <br> $(0.093)$ | 
| $\hat{a_2}$  <br> (Error estándar)        |                        |                        |$-0.378$ <br> $(0.092)$ |
| $\hat{\beta_1}$  <br> (Error estándar)    |                        |$-0.676$ <br> $(0.081)$ |                        |
| Criterio de Información de Akaike         |$496.5$                 |$471.0$                 |$482.8$                 |  
| Criterio Bayesiano de Schwartz            |$499.0$                 |$476.2$                 |$487.9$                 |  
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis)      |$Q(8) = 3.86 (0.695)$ <br> $Q(24) = 14.23 (0.892)$ | $Q(8) = 26.19 (0.000)$ <br> $Q(24) = 41.10 (0.001)$ | $Q(8) = 11.44 (0.057)$ <br> $Q(24) = 22.59 (0.424)$ | 

Al examinar la tabla, note que todos los $\hat{a_1}$ son muy significativos; cada uno de los valores estimados se desvía al menos ocho desviaciones estándar de cero. Está claro que el modelo $AR(1)$ es inapropiado. Los estadísticos Q para el Modelo 1 indican que existe una autocorrelación significativa en los residuos. El modelo estimado $ARMA(1,1)$ no sufre este problema. Además, tanto el Criterio de Información de Akaike como el Criterio Bayesiano de Schwartz seleccionan el Modelo 2 sobre el Modelo 1.

El mismo tipo de razonamiento indica que el Modelo 2 es preferible al Modelo 3. Tenga en cuenta que, para cada modelo, los coeficientes estimados son altamente significativos y las estimaciones puntuales implican convergencia. Aunque el estadístico Q en $24$ rezagos indica que estos dos modelos no tienen residuos correlacionados, el estadístico Q de $8$ rezagos indica una correlación serial en los residuos del Modelo 3. Por lo tanto, el modelo $AR(2)$ no capta la dinámica de corto plazo como lo hace el modelo $ARMA(1,1)$.  También tenga en cuenta que tanto el Criterio de Información de Akaike como el Criterio Bayesiano de Schwartz seleccionan el Modelo 2.
