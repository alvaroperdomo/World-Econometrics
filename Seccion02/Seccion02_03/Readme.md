# Análisis ARIMA
### Estimación de un modelo AR(1)
Utilizemos un ejemplo específico para ver cómo la función de autocorrelación muestral (_FAC_) y la función de autocorrelación parcial muestral (_FACP_) se pueden utilizar como ayuda para identificar un modelo _ARMA_. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Tenga en cuenta que el problema de no estacionariedad se evita ya que la condición inicial es consistente con el equilibrio a largo plazo. 

Las figuras de abajo muestran la _FAC_ y la _FACP_ muestral. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d051567c-1c96-4305-9a5b-f2f9340192da)

