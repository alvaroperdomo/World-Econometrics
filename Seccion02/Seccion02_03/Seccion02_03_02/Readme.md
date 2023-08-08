<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.3.2
# Estimando un $ARMA$ - Ejemplo simulado 

Una comparación de la función de autocorrelación $FAC$ y la función de autocorrelación parcial $FACP$ muestrales con las de varios procesos $ARMA(p,q)$ teóricos puede sugerir varios modelos plausibles. Por medio de un ejemplo sencillo, vamos a mostrar cómo se identifica el proceso $ARMA(p,q)$ generador de una variable.

En este ejemplo se va a simular un modelo _AR(1)_ y se va a utilizar la metodología de Box y Jenkins para tratar de identificarlo. Para ello, en _R_:
* Se generaron $200$ números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$.
* Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_t=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Note que la serie { $y_t$ } que se construye es estacionaria, por lo que es factible aplicar la metodología de Box-Jenkins. 

El código de $R$ utilizado en la simulación es:

```r
rm(list = ls())

library(stats) # Esta librería permite generar números aleatorios con distribución normal
library(ggfortify) # Esta librería permite utilizar el comando autoplot

set.seed(150) # Se establece una semilla para la generación de números aleatorios (puedes usar cualquier número entero)

n <- 200 # Se establece la longitud de la secuencia

epsilon <- rnorm(n, mean = 0, sd = 1) # Se crea un vector para almacenar los valores simulados de epsilon(t)

y <- numeric(n) # Se crea un vector para almacenar los valores simulados de y(t)

y[1] <- 0 # Se establece y(0) = 0

# Se establece la fórmula para generar los valores de y(t)
for (t in 2:n) {
  y[t] <- 0.7 * y[t-1] + epsilon[t]
}

plot(1:n, y, type = "l", xlab = "t", ylab = "y(t)", col = "blue") # Gráfico de y(t)

```
El gráfico de $y_t$ esta representado por:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/99b3d3f2-b1b7-4edc-9471-8a9705f00387)

A continuación se van a desarrollar cada uno de los pasos de la metodología Box-Jenkins explicados en la sección 2.3.1. 

### Identificación
Las figuras de abajo muestran la $FAC$ en  y la $FACP$ muestrales de { $y_t$ } (en el eje vertical se muestran, respectivamente, el valor de las autocorrelaciones totales y parciales de la muestra para cada uno de los rezagos que se presentan en el eje horizontal). 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/02f95011-4d78-4ab5-87ff-81624747a451)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/caad4385-a45e-4496-8ff7-fbdefc2b8db7)


Continuando con el código de $R$ de esta sección, los gráficos de la $FAC$ y la $FACP$ de $y_t$ se generaron con los comandos

```r
acf_plot <- autoplot(acf(y, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(y, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $200$ valores muestrales de la simulación que estamos manejando en esta sección y se nos pide descubrir el verdadero proceso $ARMA(p,q)$ utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la $FAC$ y la $FACP$ muestrales de $y_t$ con las de los diversos modelos teóricos. 

#### La parte autorregresiva ($AR$) del proceso generador de datos
La $FACP$ se utiliza para determinar el componente $AR(p)$ del $ARMA(p,q)$. Identifique los rezagos de la $FACP$ de $y_t$ que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{200}}=0.14$ o inferiores a $-0.14$ [^1]. Note que el pico más grande, por mucho, corresponde al rezago $1$ y vale $0.68$. esto sugiere un potencial compomemte $AR(1)$ dentro del modelo $ARMA(p,q)$. Todas las otras $FACP$ (excepto la del rezago $9$ que es aproximadamente $0.17$) son muy pequeñas.

[^1]: **En ocasiones los gráficos de la _FAC_ y  la _FACP_ incluyen el rezago _0_ , el cuál por definición siempre debe ser igual a $1$.** 

Para conocer el valor númerico exacto de la $FACP$ de $y_t$ en los rezagos $1$ y $8$ se utilizaron los comandos:

```r
pacf_result <- pacf(y, plot = FALSE) # Se calcula la función de autocorrelación parcial

pacf_lag_1 <- pacf_result$acf[1]   # Valor de la autocorrelación parcial para el rezago 1
print(pacf_lag_1)

pacf_lag_8 <- pacf_result$acf[8]   # Valor de la autocorrelación parcial para el rezago 8
print(pacf_lag_8)
```
Obteniendose:
```r
> pacf_lag_1 <- pacf_result$acf[1]   # Valor de la autocorrelación parcial para el rezago 1
> print(pacf_lag_1)
[1] 0.6784294
> pacf_lag_8 <- pacf_result$acf[8]   # Valor de la autocorrelación parcial para el rezago 8
> print(pacf_lag_8)
[1] 0.1651488
```
#### La parte de media móvil ($MA$) del proceso generador de datos
La $FAC$ se utiliza para determinar el componente $MA(q)$ del $ARMA(p,q)$. Identifique los rezagos de la $FAC$, del rezago $1$ en adelante, que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{200}}=0.14$ o inferiores a $-0.14$. Lo que se observa en la $FAC$ es un decaimieto progresivo (por ejemplo, los primeros tres rezagos de la $FAC$ son $0.68$, $0.45$ y $0.27$). Por lo tanto, parece que no se genera un proceso $MA(q)$ dentro de nuestro $ARMA(p,q)$. 

Para saber el valor de la $FAC$ de $y_t$ en los rezagos $1$, $2$ y $3$ se utilizaron los comandos:

```r
acf_result <- acf(y, plot = FALSE) # Se calcula la función de autocorrelación total

acf_lag_1 <- acf_result$acf[2]   # Valor de la autocorrelación total para el rezago 1
print(acf_lag_1)

acf_lag_2 <- acf_result$acf[3]   # Valor de la autocorrelación total para el rezago 2
print(acf_lag_2)

acf_lag_3 <- acf_result$acf[4]   # Valor de la autocorrelación total para el rezago3
print(acf_lag_3)
```
Obteniendose:
```r
> acf_result <- acf(y, plot = FALSE) # Se calcula la función de autocorrelación total
> 
> acf_lag_1 <- acf_result$acf[2]   # Valor de la autocorrelación total para el rezago 1
> print(acf_lag_1)
[1] 0.6784294
> 
> acf_lag_2 <- acf_result$acf[3]   # Valor de la autocorrelación total para el rezago 2
> print(acf_lag_2)
[1] 0.4482265
> 
> acf_lag_3 <- acf_result$acf[4]   # Valor de la autocorrelación total para el rezago3
> print(acf_lag_3)
[1] 0.2704874
```

### Estimación y verificación de diagnóstico
Aunque sabemos que los datos se generaron en realidad a partir de un proceso de $AR(1)$, es ilustrativo comparar las estimaciones de varios modelos diferentes. Suponga que estimamos un modelo $AR(1)$, un modelo $MA(1)$ y un modelo $ARMA(1,1)$, en todos los casos los estimamos con y sin intercepto. Por lo tanto, consideramos los seis modelos tentativos:

* **Modelo 1 - $AR(1)$:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - $AR(1)$:** $y_t=a_0+a_1y_{t-1}+\varepsilon_t$
* **Modelo 3 - $MA(1)$:** $y_t=\varepsilon_t+\beta_{1}\varepsilon_{t1} $
* **Modelo 4 - $MA(1)$:** $y_t=a_0+\varepsilon_t+\beta_{1}\varepsilon_{t1}$
* **Modelo 5 - $ARMA(1,1)$:** $y_t=a_0+a_1y_{t-1}+\varepsilon_t+\beta_{1}\varepsilon_{t1}$
* **Modelo 6 - $ARMA(1,1)$:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{1}\varepsilon_{t1} $
  
La estimación en $R$ de los seis modelos, de una vez haciendo la verificación de diagnóstico, se hace con los siguientes comandos:

```r
Modelo_1<- Arima(y, order=c(1,0,0), include.constant=FALSE)  # Se calcula la función AR(1) sin mostrarla
Modelo_2<- Arima(y, order=c(1,0,0))  # Se calcula la función AR(1) sin mostrarla
Modelo_3<- Arima(y, order=c(0,0,1), include.constant=FALSE)  # Se calcula la función MA(1) sin mostrarla. 
Modelo_4<- Arima(y, order=c(0,0,1))  # Se calcula la función MA(1) sin mostrarla. 
Modelo_5<- Arima(y, order=c(1,0,1), include.constant=FALSE)  # Se calcula la función ARMA(1,1) sin mostrarla. 
Modelo_6<- Arima(y, order=c(1,0,1))  # Se calcula la función ARMA(1,1) sin mostrarla. 

summary(Modelo_1) # Se reportan los resultados del Modelo_1
summary(Modelo_2) # Se reportan los resultados del Modelo_2
summary(Modelo_3) # Se reportan los resultados del Modelo_3
summary(Modelo_4) # Se reportan los resultados del Modelo_4
summary(Modelo_5) # Se reportan los resultados del Modelo_5
summary(Modelo_6) # Se reportan los resultados del Modelo_6

AIC(Modelo_1,Modelo_2, Modelo_3, Modelo_4, Modelo_5, Modelo_6) # Se reportan los Criterios de Información de Akaike para todos los modelos considerados
BIC(Modelo_1,Modelo_2, Modelo_3, Modelo_4, Modelo_5, Modelo_6) # Se reportan los Criterios Bayesianos de Schwartz para todos los modelos considerados

ggtsdiag(Modelo_1, gof.lag = 30) +  labs(subtitle = "Modelo 1") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 1.
ggtsdiag(Modelo_2, gof.lag = 30) +  labs(subtitle = "Modelo 2") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 2.
ggtsdiag(Modelo_3, gof.lag = 30) +  labs(subtitle = "Modelo 3") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 3.
ggtsdiag(Modelo_4, gof.lag = 30) +  labs(subtitle = "Modelo 4") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 4.
ggtsdiag(Modelo_5, gof.lag = 30) +  labs(subtitle = "Modelo 5") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 5.
ggtsdiag(Modelo_6, gof.lag = 30) +  labs(subtitle = "Modelo 6") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 6.

```
Obteniendose 

```r
> summary(Modelo_1) # Se reportan los resultados del Modelo_1
Series: y 
ARIMA(1,0,0) with zero mean 

Coefficients:
         ar1
      0.6783
s.e.  0.0514

sigma^2 = 0.935:  log likelihood = -276.87
AIC=557.74   AICc=557.8   BIC=564.34

Training set error measures:
                   ME      RMSE       MAE      MPE     MAPE      MASE       ACF1
Training set 0.042355 0.9645131 0.7702037 31.32772 197.8036 0.9444288 0.01526996
> summary(Modelo_2) # Se reportan los resultados del Modelo_2
Series: y 
ARIMA(1,0,0) with non-zero mean 

Coefficients:
         ar1    mean
      0.6752  0.1289
s.e.  0.0516  0.2077

sigma^2 = 0.9379:  log likelihood = -276.68
AIC=559.36   AICc=559.48   BIC=569.25

Training set error measures:
                      ME      RMSE       MAE  MPE MAPE      MASE      ACF1
Training set 0.000604357 0.9636119 0.7719439 -Inf  Inf 0.9465626 0.0182812
> summary(Modelo_3) # Se reportan los resultados del Modelo_3
Series: y 
ARIMA(0,0,1) with zero mean 

Coefficients:
         ma1
      0.5401
s.e.  0.0454

sigma^2 = 1.14:  log likelihood = -296.57
AIC=597.14   AICc=597.2   BIC=603.73

Training set error measures:
                     ME    RMSE       MAE      MPE     MAPE     MASE      ACF1
Training set 0.08345461 1.06507 0.8467178 79.27278 141.0902 1.038251 0.2171121
> summary(Modelo_4) # Se reportan los resultados del Modelo_4
Series: y 
ARIMA(0,0,1) with non-zero mean 

Coefficients:
         ma1    mean
      0.5384  0.1278
s.e.  0.0455  0.1153

sigma^2 = 1.139:  log likelihood = -295.96
AIC=597.91   AICc=598.04   BIC=607.81

Training set error measures:
                       ME     RMSE       MAE  MPE MAPE     MASE      ACF1
Training set 0.0003588097 1.061824 0.8465603 -Inf  Inf 1.038058 0.2193961
> summary(Modelo_5) # Se reportan los resultados del Modelo_5
Series: y 
ARIMA(1,0,1) with zero mean 

Coefficients:
         ar1     ma1
      0.6641  0.0264
s.e.  0.0748  0.0980

sigma^2 = 0.9393:  log likelihood = -276.83
AIC=559.67   AICc=559.79   BIC=569.56

Training set error measures:
                     ME      RMSE       MAE      MPE    MAPE      MASE        ACF1
Training set 0.04300838 0.9643357 0.7707917 32.23084 198.014 0.9451498 0.002210626
> summary(Modelo_6) # Se reportan los resultados del Modelo_6
Series: y 
ARIMA(1,0,1) with non-zero mean 

Coefficients:
         ar1     ma1    mean
      0.6598  0.0287  0.1289
s.e.  0.0754  0.0983  0.2040

sigma^2 = 0.9423:  log likelihood = -276.64
AIC=561.27   AICc=561.48   BIC=574.47

Training set error measures:
                      ME      RMSE       MAE  MPE MAPE      MASE        ACF1
Training set 0.000551532 0.9634029 0.7728966 -Inf  Inf 0.9477308 0.004247725
> 
> AIC(Modelo_1,Modelo_2, Modelo_3, Modelo_4, Modelo_5, Modelo_6) # Se reportan los Criterios de Información de Akaike para todos los modelos considerados
         df      AIC
Modelo_1  2 557.7390
Modelo_2  3 559.3574
Modelo_3  2 597.1365
Modelo_4  3 597.9132
Modelo_5  3 559.6663
Modelo_6  4 561.2718
> BIC(Modelo_1,Modelo_2, Modelo_3, Modelo_4, Modelo_5, Modelo_6) # Se reportan los Criterios Bayesianos de Schwartz para todos los modelos considerados
         df      BIC
Modelo_1  2 564.3356
Modelo_2  3 569.2524
Modelo_3  2 603.7332
Modelo_4  3 607.8082
Modelo_5  3 569.5613
Modelo_6  4 574.4651
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5ba8ff9f-4170-4ed3-afdd-c85a1b3abcf8)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/18a531a2-7baf-48c1-8f9e-792469a87486)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/12b7cd53-df2a-4966-8eef-fe9102fa19f3)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4d29f7e1-e5aa-4edc-a3a5-24bff8ce6972)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/3bfb6490-38c5-437a-a049-d5b15aedc554)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/991c5c19-f4d5-4899-8bc2-93cb116b450f)

La tabla de abajo y los seis conjuuntos de tres gráficos de arriba (un conjunto por modelo) resumen los resultados de las seis estimaciones

| Indicadores                               | Modelo 1                | Modelo 2                |  Modelo 3               | Modelo 4                |  Modelo 5               | Modelo 6                | 
|-------------------------------------------|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| $\hat{a}_0$  <br> (Error estándar)        |                         |$0.1289$ <br> ($0.2077)$ |                         |$0.1278$ <br> ($0.1153)$ |                         |$0.1289$ <br> ($0.2040)$ | 
| $\hat{a}_1$  <br> (Error estándar)        |$0.6783$ <br> ($0.0514)$ |$0.6752$ <br> ($0.0516$) |                         |                         |$0.6641$ <br> ($0.0748)$ |$0.6598$ <br> ($0.0754)$ | 
| $\hat{\beta}_{1}$  <br> (Error estándar)  |                         |                         |$0.5401$ <br> ($0.0454)$ |$0.5384$ <br> ($0.0455)$ |$0.0264$ <br> ($0.0980)$ |$0.0287$ <br> ($0.0983)$ |
| Criterio de Información de Akaike         |$557.7390$               |$559.3574$               |$597.1365$               | $597.9132$              | $559.6663$              | $561.2718$              | 
| Criterio Bayesianode Schwartz             |$564.3356$               |$569.2524$               |$603.7332$               | $607.8082$              | $569.5613$              | $574.4651$              |   

**Análicemos los Modelo 1 y 2**

El coeficiente de ambos modelos satisface la condición de estabilidad $|\hat{a}_1| < 0$ y tiene un error estándar bajo (es decir, el coeficente estimado es mator a dos desviaciones estandar del valor de $|\hat{a}_1|$).

Como una verificación de diagnóstico útil, arriba en el gráfico de la $FAC$ de los residuos estimados de los Modelos 1 y 2. Note que las barras de las autocorrelaciones de todos los rezagos superiores a $0$ son inferiores a $0.14$. Es decir, en ambos modelos los residuos estimados parecen ser ruido blanco. Confirmando lo anterior, en el gráfico de la prueba de Ljung-Box sobre los residuos estimados de ambos modelos note que los p-values son superiores al 5%. Esto es  una fuerte evidencia de que ambos modelos $AR(1)$ se ajustan bien con los datos. Después de todo, si las autocorrelaciones de los residuos fueran significativas, el modelo $AR(1)$ no usaría toda la información disponible sobre los movimientos en la serie { $y_t$ }

Sin embargo, de los dos modelos, **el Modelo 1 es mejor que el Modelo 2**, porque  $\hat{a}_0$ no es estadíticamente significativa, y además los valores del _Criterio de Información de Akaike_ y del _Criterio Bayesiano de Schwartz_ del Modelo 1 son menores.

**Análicemos los modelos 3 y 4**

En ambos modelos, observe que la estimación del $\hat{\beta}_1$ es de buena calidad (es decir, el coeficiente estimado es mayor a dos desviaciones estandar del valor de $|\hat{\beta}_1|$). Sin embargo, ambos modelos tienen problemas de autocorrelación en los residuos por lo que no serían ruido blanco. Los problemas de autocorrelación se evidencian en:
* El gráfico de la $FAC$ de sus residuos estimados: Note que las barras de los rezagos $1$ y $2$ son muy superiores a $0.14$.
* El  gráfico de la prueba de Ljung-Box sobre los residuos estimados de ambos modelos: Note que los p-values son inferiores al 5%. Es decir, se rechaza la hipótesis nula de que los residuos de la estimación no estan corelacionados.

Por ejemplo, contraste usando el siguiente comando de R, los resultados detallados de la prueba de Lung-Box cuando se hace para los rezagos $1$ a $8$, $1$ a $16$ y $1$ a $24$, en ell Modelo 1 y en el Modelo 3:

```r
Box.test(Modelo_1$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_1$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_1$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box

Box.test(Modelo_3$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_3$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_3$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box
```

Obteniendose 
```r
	Box-Ljung test

data:  Modelo_1$residuals
X-squared = 9.0987, df = 8, p-value = 0.334

> Box.test(Modelo_1$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_1$residuals
X-squared = 12.059, df = 16, p-value = 0.7399

> Box.test(Modelo_1$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_1$residuals
X-squared = 13.166, df = 24, p-value = 0.9633

> 
> Box.test(Modelo_3$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_3$residuals
X-squared = 56.072, df = 8, p-value = 2.731e-09

> Box.test(Modelo_3$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_3$residuals
X-squared = 62.598, df = 16, p-value = 1.899e-07

> Box.test(Modelo_3$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_3$residuals
X-squared = 64.443, df = 24, p-value = 1.469e-05
```
Note que el estadistico Ljung-Box en el Modelo 1 es muy superior al 5% y en el Modelo 3 es muy inferior al 5%.


**Análicemos los modelos 5 y 6**

En ambos modelos se cumplen las pruebas de verificación de los residuos estimados; sin embargo el coeficiente $\hat{\beta}_{1}$ no es significativo (el coeficiente estimado es menor a dos desviaciones estandar del valor de $|\hat{\beta}_1|$); por lo que nos se podrían considrear buenos modelos.

Por último, note que en la comparación de los valores del _Criterio de Información de Akaike_ y del _Criterio Bayesiano de Schwartz_ de los todos los modelos analizados sugiere que es mucho mejor estimar el **Modelo 1** .

---
---
# Preguntas de selección múltiple

Se va a aprovechar este apartado que a partir de simulaciones no siempre es fácil construir un ejemplo que sirva para identificar el modelo $ARMA(p,q)$. En buena parte, eso se debe a que los residuos $\varepsilon_t$ se generan de forma aleatoria, por lo que no se tiene demasiado control sobre la generación de los mismos. Por eso es que generalmente, los ejemplos de identificación se hacen sobre datos reales como se hace en la subsección 2.3.3.

En este ejemplo se va a simular un modelo _MA(1)_ y se va a utilizar la metodología de Box y Jenkins para tratar de identificarlo. Para ello, en _R_:
* Se generaron $200$ números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$.
* Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_t=\varepsilon_t-0.7\varepsilon_{t-1}$. 

El código de $R$ utilizado en la simulación es:

rm(list = ls())

library(stats) # Esta librería permite generar números aleatorios con distribución normal
library(ggfortify) # Esta librería permite utilizar el comando autoplot
library(forecast) # esta libreria permite utilizar el comando auto.arima

set.seed(50) # Se establece una semilla para la generación de números aleatorios (puedes usar cualquier número entero)

n <- 200 # Se establece la longitud de la secuencia

epsilon <- rnorm(n, mean = 0, sd = 1) # Se crea un vector para almacenar los valores simulados de epsilon(t)

y <- numeric(n) # Se crea un vector para almacenar los valores simulados de y(t)

# Se establece la fórmula para generar los valores de y(t)
for (t in 2:n) {
  y[t] <- epsilon[t] - 0.7 * epsilon[t-1]
}

plot(1:n, y, type = "l", xlab = "t", ylab = "y(t)", col = "blue") # Gráfico de y(t)

```r
rm(list = ls())

library(stats) # Esta librería permite generar números aleatorios con distribución normal
library(forecast) # esta libreria permite utilizar el comando auto.arima

set.seed(50) # Se establece una semilla para la generación de números aleatorios (puedes usar cualquier número entero)

n <- 200 # Se establece la longitud de la secuencia

epsilon <- rnorm(n, mean = 0, sd = 1) # Se crea un vector para almacenar los valores simulados de epsilon(t)

y <- numeric(n) # Se crea un vector para almacenar los valores simulados de y(t)

y[1] <- 0 # Se establece y(0) = 0

# Se establece la fórmula para generar los valores de y(t)
for (t in 2:n) {
  y[t] <- epsilon[t] - 0.7 * epsilon[t-1]
}

plot(1:n, y, type = "l", xlab = "t", ylab = "y(t)", col = "blue") # Gráfico de y(t)

```
El gráfico de $y_t$ esta representado por:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/91f180f2-e72b-489d-9408-f782f5425a9b)

Halle la $FAC$ y la $FACP$ de $y_t$ y responda a la siguiente pregunta:

1. **Gráfique la $FAC$ y la $FACP$ muestrales de $y_t$. En su orden, ¿cuántos barras, superiores a la del rezago $0$, de ambas funciones se salen por fuera de la banda de confianza?:**

   a) 3 y 4.

   b) 1 y 1.

   c) 0 y 1.

   d) 2 y 2.
   
```r
acf_plot <- autoplot(acf(y, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(y, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```

De acuerdo a la $FAC$ y a la $FACP$, existen varios modelos potenciales que pueden representar a nuestra simulación. el comando $auto.arima$ sirve para calcular de forma rápida el _Criterio de Información de Akaike_ y el _Criterio Bayesiano de Schwartz_ de diferentes especificaciones de modelos $ARIMA(p,I,q)$. Copie los siguientes comandos, 

 ```r
auto.arima(y, ic = c("aic"), trace=TRUE, stepwise = FALSE, approximation = FALSE)
auto.arima(y, ic = c("bic"), trace=TRUE, stepwise = FALSE, approximation = FALSE)
```
y responda a las dos preguntas que siguen a continuación:

2. **Según el _Criterio de Información de Akaike_, en su orden, cuales son los dos modelos $ARIMA(p,I,q)$ que mejor se ajustan a los datos simulados:**
   
   a) MA(1) sin intercepto y MA(1) con intercepto.

   b) MA(1) con intercepto y MA(1) sin intercepto.

   c) MA(1) sin intercepto y AR(1) sin intercepto.

   d) MA(1) cin intercepto y AR(1) sin intercepto.

3. **Según el _Criterio Bayesiano de Schwartz_, en su orden, cuales son los dos modelos $ARIMA(p,I,q)$ que mejor se ajustan a los datos simulados:**
   
   a) MA(1) sin intercepto y MA(1) con intercepto.

   b) MA(1) con intercepto y MA(1) sin intercepto.

   c) MA(1) sin intercepto y AR(1) sin intercepto.

   d) MA(1) cin intercepto y AR(1) sin intercepto.
---
---
| [Subsección 2.3. - Análisis _ARMA_ (La Metodología de Box-Jenkins)](../Readme.md)| [Siguiente Subsección: 2.3.3 Caso de estudio utilizando la base de datos _Indicadores de Desarrollo Mundial_ (segunda parte)](../Seccion02_03_03/Readme.md) |
|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
