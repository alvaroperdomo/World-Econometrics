# Análisis ARIMA (Metodología de Box-Jentins)

Las autocovarianzas y autocorrelaciones sirven como herramientas útiles en el enfoque de Box y Jenkins (1976) para identificar y estimar modelos de series de tiempo. 


### Estimación de un modelo AR(1)
Utilizemos un ejemplo específico para ver cómo la función de autocorrelación muestral (_FAC_) y la función de autocorrelación parcial muestral (_FACP_) se pueden utilizar como ayuda para identificar un modelo _ARMA_. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Tenga en cuenta que el problema de no estacionariedad se evita ya que la condición inicial es consistente con el equilibrio a largo plazo. 

Las figuras de abajo muestran la _FAC_ y la _FACP_ muestral. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d051567c-1c96-4305-9a5b-f2f9340192da)

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $100$ valores muestrales de la figura de arriba y se nos pide descubrir el verdadero proceso utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la _FAC_ muestral y la _FACP_ muestral con las de los diversos modelos teóricos. El patrón de decaimiento de la _FAC_ y el único pico grande en el rezago $1$ en la _FACP_ sugiere un modelo _AR(1)_. 

Las primeras tres _FAC_ son $r_1=0.74$, $r_2=0.58$ y $r_3=0.47$. En el _FACP_, hay un pico considerable de $0.74$ en el rezago $1$, y todas las otras _FACP_ (excepto la del rezago $12$) son muy pequeñas
