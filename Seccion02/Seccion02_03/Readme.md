## SECCIÓN 2.3:
# Análisis ARMA (La Metodología de Box-Jenkins)

Box y Jenkins (1976) popularizaron una metodología para seleccionar el modelo $ARMA(p,q)$ apropiado para estimar una serie de tiempo univariada. Tenga, en cuenta que la misma sólo es valida en series estacionarias. Por lo tanto, como se comento anteriormente, si una serie de tiempo no es estacionaria, transformela en una serie estacionaria para poder aplicar la propuesta de Box y Jenkins.

La siguiente tabla presenta tres subsecciones en que esta dividida esta sección. Dando _click_ en la **X** de la segunda columna de la tabla puede redirigirse a la explicación teórica de la respectiva subsección y dando click en la **X** de la tercera columna de la tabla puede redirigirse para ver las diferentes aplicaciones en $R$. Lo ideal es que aborden todas las secciones en el orden establecido, primero viendo la explicación de la prueba y posteriormente su aplicación en $R$.  

| Subsecciones                                                                     | Explivación teórica               | Aplicación en R                 |
|----------------------------------------------------------------------------------|:---------------------------------:|:-------------------------------:|
| **2.3.1.** Las tres etapas de la metodología de Box-Jenkins                      | [X](Seccion02_03_01_T/Readme.md)  |[X](Seccion02_03_01_R/Readme.md) |
| **2.3.2.** Estimación de un ARMA - Ejemplos simulados                            |                                   |[X](Seccion02_03_02/Readme.md)   |
| **2.3.3.** Ejemplo utilizando la base de datos Indicadores de Desarrollo Mundial |                                   |[X](Seccion02_03_03/Readme.md)   |

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Anterior Sección: 2.2. Pruebas de Raíz Unitaria](../Seccion02_02/Readme.md) | [:house: Inicio](../../README.md) |[Siguiente Sección: 3. Análisis Multivariado (Modelos _VAR_ y _VEC_)](../../Seccion03/Readme.md) |
|------------------------------------------------------------------------------|-----------------------------------|-------------------------------------------------------------------------------------------------|




## Ejemplo utilizando la primera diferencia en el Producto Interno Bruto per cápita de Colombia a precios Constantes en moneda local

Para este ejercicio, dado que previamente ya habiamos demostrado que la variable $PIBpc$ es integrada de orden uno, es decir la variable $C1PIBpc$ es estacionaria. entonces, vamos a retomar parte del código utilizado en secciones previas para analizar el comportamiento de $C1PIBpc$ y a partir del mismo poder deducir el comportamiento de $PIBpc$, tal como se muestra a continuación:

``` r
rm(list = ls())

rm(list = ls())

library(WDI)         # Esta libreria sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta libreria permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggfortify)   # Esta libreria tienen comando utiles para plantear gráficos de series de tiempo, por ejemplo utilizando ell comando autoplot()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(fUnitRoots)  # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(urca)        # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(forecast)    # Esta libreria sirve para hacer pronósticos.

WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')

dat = WDI(indicator= c(PIBpc = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, PIBpc_lag1 = lag(PIBpc, order_by = year), C1PIBpc=PIBpc-PIBpc_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

ggplot(dat, aes(year, C1PIBpc)) + scale_x_continuous(name="Años", limits=c(1961, 2019))  + geom_line (linewidth=0.2) + labs(subtitle="1960-2019", y="Pesos constantes", title="Cambio en el PIB per cápita real de Colombia", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/a42692a0-f034-42b7-82b5-9aae03615a37)

Del gráfico de **C1PIBpc** ya hablamos en la sección 2.2., por lo que no diremos nada mas al respecto. Antes de pasar a la etapa de identificación, vamos a preparar un poco la base de datos para que $R$ identifique a la variable **C1PIBpc** como una serie de tiempo

``` r
C1PIBpc_ <- dat[-c(1,61:63),] # Con este comando creamos la base de datos "C1PIBpc_" la cual es igual a la base de datos "dat", pero sin incluir los años 1961 (Fila (1) - del cual "dat" no existe información para C1PIBpc) y los años 2020-2023 (Filas (61-63) - los cuales no vamos a utilizar en la estimación de C1PIBpc) 
C1PIBpc_ = subset(C1PIBpc_, select = c(C1PIBpc)) # Con este comando deburamos la base de datos C1PIBpc_ para que sólo incluya la variable C1PIBpc
C1PIBpc <- ts(C1PIBpc_) # Con el comando ts() identificamos a la variable C1PIBpc como una serie de tiempo que se encuentra en la base de datos C1PIBpc_

autoplot(acf(C1PIBpc, plot = FALSE))
autoplot(pacf(C1PIBpc, plot = FALSE))

**1) Identificación:**
Gráficamos las variable **C1PIBpc** así como sus funciones $FAC$ y $FACP$



Entonces, nos preparamosa para gráficar las funciones $FAC$ y $FACP$ de estas dos variables,



autoplot(acf(C1PIBpc, plot = FALSE))

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







