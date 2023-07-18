## SECCIÓN 2.3:
# Análisis ARMA (La Metodología de Box-Jenkins)

Box y Jenkins (1976) popularizaron una metodología para seleccionar el modelo $ARMA(p,q)$ apropiado para estimar una serie de tiempo univariada. Tenga, en cuenta que la misma sólo es valida en series estacionarias. Por lo tanto, como se comento anteriormente, si una serie de tiempo no es estacionaria, transformela en una serie estacionaria para poder aplicar la propuesta de Box y Jenkins.

Esta sección se encuentra dividida en tres subsecciones. Para acceder a cada una de las subsecciones haga click en la tabla de abajo en la subsección respectiva. Sin embargo, lo ideal es que se aborden las tres secciones en el orden que esta prescrito.

| Subsecciones                                                                                                  | 
|---------------------------------------------------------------------------------------------------------------|
| [**2.3.1.** Las tres etapas de la metodología de Box-Jenkins](Seccion02_03_01/Readme.md)                      | 
| [**2.3.2.** Estimación de un ARMA - Ejemplos simulados](Seccion02_03_02/Readme.md)                            |
| [**2.3.3.** Ejemplo utilizando la base de datos Indicadores de Desarrollo Mundial](Seccion02_03_03/Readme.md) |



4. EJEMPLOS SIMULADOS: 
Una comparación de las $FAC$ y las $FACP$ muestrales con las de varios procesos $ARMA()$ teóricos puede sugerir varios modelos plausibles. Por medio de dos ejemplos sencillos, vamos a mostrar cómo se identifica el proceso generador de una variable.

### Estimación de un modelo AR(1)
Utilizemos un ejemplo específico para ver cómo la función de autocorrelación muestral ($FAC$) y la función de autocorrelación parcial muestral ($FACP$) se pueden utilizar como ayuda para identificar un modelo $ARMA()$. 
Se utilizó R para obtener 100 números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Tenga en cuenta que el problema de no estacionariedad se evita ya que la condición inicial es consistente con el equilibrio a largo plazo. 

Las figuras de abajo muestran la $FAC$ y la $FACP$ muestral. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d051567c-1c96-4305-9a5b-f2f9340192da)

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $100$ valores muestrales de la figura de arriba y se nos pide descubrir el verdadero proceso utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la $FAC$ muestral y la $FACP$ muestral con las de los diversos modelos teóricos. 

#### La parte autorregresiva (AR) del proceso generador de datos (las $FACP$)
El único pico grande que se observa en el rezago $1$ de la $FACP$ sugiere un modelo $AR(1)$.

##### Sin embargo, si no conociéramos el verdadero proceso subyacente y estuvieramos utilizando datos mensuales, podríamos preocuparnos por la autocorrelación parcial significativa en el rezago $12$. Después de todo, con los datos mensuales, podríamos esperar alguna relación directa entre $y_t$ y $y_{t-12}$.

El pico es de $0.74$ en el rezago $1$, y todas las otras $FACP$ (excepto la del rezago $12$) son muy pequeñas. 

A partir de la figura de las $FACP$, se puede ver que todas las $FACP$ a excepción de la $\phi_{11}$ (y del desfase en el rezago $12$) son menores que $\frac{2}{\sqrt{}T}=0.2$ . 

#### La parte de media móvil (MA) del proceso generador de datos (las _FAC_)
Las $FAC$ revelan una decaimieto progresivo (por ejemplo, las primeras tres $FAC$ son $r_1=0.74$, $r_2=0.58$ y $r_3=0.47$.) que no dan la idea de un proceso $MA$. 

#### Análisis de la conjetura

Aunque sabemos que los datos se generaron en realidad a partir de un proceso de $AR(1)$, es ilustrativo comparar las estimaciones de dos modelos diferentes. Suponga que estimamos un modelo $AR(1)$ e intentamos capturar el pico en el desfase $12$ con un coeficiente $MA$. Por lo tanto, consideramos los dos modelos tentativos:

* **Modelo 1 - _AR(1)_:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - _ARMA(1,12)_:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{12}\varepsilon_{t-12} $

La tabla de abajo informa los resultados de las dos estimaciones

| Indicadores                               | Modelo 1                | Modelo 2                | 
|-------------------------------------------|:-----------------------:|:-----------------------:|
| Grados de Libertad                        |$98$                     |$97$                     | 
| SRC (Suma de Residuos al Cuadrado)        |$85.10$                  |$85.07$                  | 
| $\hat{a_1}$  <br> (Error estándar)        |$0.7904$ <br> ($0.0624)$ |$0.7938$ <br> ($0.0643$) | 
| $\hat{\beta_{12}}$  <br> (Error estándar) |                         |$-0.0325$ <br> ($0.11$)  | 
| Criterio de Información de Akaike         |$441.9$                  |$443.9$                  | 
| Criterio Bayesianode Schwartz             |$444.5$                  |$449.1$                  | 
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis) |$Q(8) = 6.43 (0.490)$ <br> $Q(16) = 15.86 (0.391)$ <br> $Q(24) = 21.74 (0.536)$ |$Q(8) = 6.48 (0.485)$ <br> $Q(16) = 15.75 (0.400)$ <br> $Q(24) = 21.56 (0.547)$ | 

El coeficiente del Modelo 1 satisface la condición de estabilidad $|a_1|<0$ y tiene un error estándar bajo (el estadístico t asociada es mayor que $12$). Como una verificación de diagnóstico útil, dibujamos el correlograma de los residuos del modelo ajustado en la figura de abajo. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/bf030c21-940a-4842-9971-5a42410f7b52)

Los estadísticos Q de los residuos indican que:
* cada una de las autocorrelaciones es menor que dos desviaciones estándar de cero.
* como grupo, los rezagos $1$ a $8$, $1$ a $16$ y $1$ a $24$ no son significativamente diferentes de cero. 

Esta es una fuerte evidencia de que el modelo $AR(1)$ se ajusta bien con los datos. Después de todo, si las autocorrelaciones de los residuos fueran significativas, el modelo $AR(1)$ no usaría toda la información disponible sobre los movimientos en la secuencia { $y_t$ }

Al examinar los resultados para el Modelo 2, observe que ambos modelos arrojan estimaciones similares para :
* el coeficiente autorregresivo de primer orden y
* el error estándar asociado. 

Sin embargo, 
* la estimación para $\beta_{12}\$ es de mala calidad;
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

## Ejemplo utilizando la primera diferencia en el Producto Interno Bruto per cápita de Colombia a precios Constantes en moneda local

Para este ejercicio, dado que previamente ya habiamos demostrado que la variable $PIBpc$ es integrada de orden uno, es decir la variable $C1PIBpc$ es estacionaria. entonces, vamos a retomar parte del código utilizado en secciones previas para analizar el comportamiento de $C1PIBpc$ y a partir del mismo poder deducir el comportamiento de $PIBpc$, tal como se muestra a continuación:

``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)

WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')

dat = WDI(indicator= c(PIBpc = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, PIBpc_lag1 = lag(PIBpc, order_by = year), C1PIBpc=PIBpc-PIBpc_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

ejercicio <- dat[-c(61:63),]
ejercicioC1 <- ejercicio[-c(1),]
ejercicio <- mutate(ejercicio, PIBpc_lag1=NULL, C1PIBpc=NULL)
ejercicioC1 <- mutate(ejercicioC1, PIBpc=NULL, PIBpc_lag1=NULL)
```
**1) Identificación:**
Gráficamos la variable **C1PIBpc** así como sus funciones $FAC$ y $FACP$

``` r
PIBpc_ = subset(ejercicio, select = c(PIBpc))
PIBpc <- ts(PIBpc_)
C1PIBpc_ = subset(ejercicioC1, select = c(C1PIBpc))
C1PIBpc <- ts(C1PIBpc_)
autoplot(acf(C1PIBpc, plot = FALSE))
autoplot(pacf(C1PIBpc, plot = FALSE))
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/7de314f5-3345-4b56-ad70-136bfe6b33ed)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/b29da0bb-daff-4ffc-a416-91066fd9edcd)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/6b57037b-87e6-4c19-a9d3-f01787e41fbe)

**2) Estimación:**
La $FACP$ da a entender que la serie sigue un proceso autorregresivo de orden $1$, y la $FAC$ revela la existencia de un comportamiento de media movilde orde $1$ o de orden $2$. En consecuencia, vamos a comparar los siguientes modelos $ARIMA(p,1,q)$ para la variable $PIBpc$ (esto es equivalente a comparar los siguientes modelos $ARIMA(p,0,q)$ para la variable $C1PIBpc$).

``` r
arima1<- Arima(PIBpc, order=c(0,1,2))
arima2<- Arima(PIBpc, order=c(1,1,0))
arima3<- Arima(PIBpc, order=c(1,1,2))
arima4<- Arima(PIBpc, order=c(1,1,1))
arima5<- Arima(PIBpc, order=c(0,1,1))

AIC(arima1,arima2,arima3,arima4,arima5)
BIC(arima1,arima2,arima3,arima4,arima5)
```
Obteniendo,
``` r
> AIC(arima1,arima2,arima3,arima4,arima5)
       df      AIC
arima1  3 1629.190
arima2  2 1617.199
arima3  4 1618.947
arima4  3 1618.328
arima5  2 1633.899
> BIC(arima1,arima2,arima3,arima4,arima5)
       df      BIC
arima1  3 1635.423
arima2  2 1621.354
arima3  4 1627.257
arima4  3 1624.561
arima5  2 1638.054
```
Se puede apreciar que los menores valores del Criterio de Información de Akaike (AIC) y del Criterio Bayesiano de Schwartz (BIC) se presentan el modelo ARIMA(1,1,0) en donde $AIC=1617.199$ y $BIC=1621.354. 

**3) Verificación de diagnóstico:**
Una vez estimados los modelos y elegido el mejor de ellos, en este caso ARIMA(1,1,0), se procede a validarlo. Para ello en primer lugar se grafican los correlogramas de los residuos para comprobar que son ruido blanco:
``` r
autoplot(acf(arima2$residuals, plot = FALSE))
autoplot(pacf(arima2$residuals, plot = FALSE))
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5b427f75-205e-4d3a-a903-da89bfc762db)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/71fea7cf-2fbc-458c-bd46-5184a63e76ff)

Se puede apreciar en las FAC y en las FACP que no hay ningún rezago significativo que denote ningún tipo de estructura, por lo tanto podemos decir que los residuos son ruido blanco. Ahora, vamos a ver un gráfico de los residuos:

``` r
ggtsdiag(arima2)
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/09bbd66d-473b-4cf5-93bb-9b04bb63b38a)

Y hacemos la prueba $Q$ de Ljung-Box
``` r
lb <- Box.test(arima2$residuals, type="Ljung-Box") # Test de Ljung-Box
lb$p.value
```
Obteniendo
``` r
> lb$p.value
[1] 0.2478848
```
En consecuencia, la prueba de rechaza la hipótesis nula de correlación cero en los residuos; por lo tanto, se certifica nuevamente que los residuos son ruido blanco.

En R existe una función llamada auto.arima que puede calcular automáticamente cuál es la mejor combinación de órdenes para el modelo ARIMA:

``` r
auto.arima(PIBpc, stepwise = FALSE, approximation = FALSE)
```
Obteniendo
``` r
> auto.arima(PIBpc, stepwise = FALSE, approximation = FALSE)
Series: PIBpc 
ARIMA(1,1,0) with drift 

Coefficients:
         ar1      drift
      0.5256  205388.50
s.e.  0.1087   52528.73

sigma^2 = 3.933e+10:  log likelihood = -802.52
AIC=1611.04   AICc=1611.48   BIC=1617.28
```
En este caso la función certifica que el mejor modelo que representa a la función PIBpc sería un ARIMA(1,1,0) con intercepto.[1^]
[1^]: Observen que si se hace el mismo analisis para la variable C1PIBpc obtienen que el mejor modelo ARIMA es el modelo ARIMA(1,0,0)

Ahora vamos a hacer un pronostico de tres periodos con esta variable (correspondiente al periodo 2020-2022)
``` r
forecast1<-forecast(arima2, level = c(95), h = 3)
summary(forecast1)
autoplot(forecast1)
```
Obteniendo:
``` r
Forecast method: ARIMA(1,1,0)

Model Information:
Series: PIBpc 
ARIMA(1,1,0) 

Coefficients:
         ar1
      0.7326
s.e.  0.0856

sigma^2 = 4.405e+10:  log likelihood = -806.6
AIC=1617.2   AICc=1617.41   BIC=1621.35

Error measures:
                   ME     RMSE      MAE       MPE     MAPE      MASE       ACF1
Training set 56740.45 206347.3 157757.3 0.5673997 1.512671 0.6526939 -0.1455209

Forecasts:
   Point Forecast    Lo 95    Hi 95
61       17725632 17314285 18136980
62       17847959 17025052 18670866
63       17937581 16693138 19182024
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/09900197-149b-460e-b481-51c942aa0a54)

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>







