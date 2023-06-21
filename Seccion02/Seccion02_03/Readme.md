# Análisis ARIMA (Metodología de Box y Jenkins)

Las autocovarianzas y autocorrelaciones sirven como herramientas útiles en el enfoque de Box y Jenkins (1976) para identificar y estimar modelos de series de tiempo. 


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

| Indicadores                         | Modelo 1       | Modelo 2       | 
|-------------------------------------|:--------------:|:--------------:|
| Grados de Libertad                  |98              |97              | 
| SRC (Suma de Residuos al Cuadrado   |85.10           |85.07           | 
| $\hat{a_1}$ (Error estándar)        |0.7904 (0.0624) |0.7938 (0.0643) | 
| $\hat{\beta_{12}}$ (Error estándar) |                |-0.0325 (0.11)  | 
| Criterio de Información de Akaike   |441.9           |443.9           | 
| Criterio Bayesianode Schwartz       |444.5           |449.1           | 



