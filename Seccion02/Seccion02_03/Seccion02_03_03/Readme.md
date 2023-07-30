<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SECCIÓN 2.3.3.
# Caso de estudio utilizando la base de datos _Indicadores de Desarrollo Mundial_ (segunda parte)

Vamos a continuar con el ejercicio empírico planteado en la sección 2.2.5. Dado que en la sección 2.2.5 habiamos demostrado que la variable $PIBpc$ es integrada de orden uno, por lo que la variable $C1PIBpc$ es estacionaria. Entonces, vamos a retomar parte del código utilizado en secciones previas para analizar el comportamiento de $C1PIBpc$ y a partir del mismo poder deducir el comportamiento de $PIBpc$:

``` r
rm(list = ls())

library(WDI)         # Esta libreria sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta libreria permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggfortify)   # Esta libreria tienen comando utiles para plantear gráficos de series de tiempo, por ejemplo utilizando ell comando autoplot()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(fUnitRoots)  # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(urca)        # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(forecast)    # Esta libreria sirve para hacer pronósticos.

WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')

# Obtenemos los datos de PIBpc para Colombia desde 1960 hasta 2019
dat <- WDI(indicator = "NY.GDP.PCAP.KN", country = "CO", start = 1960, end = 2019)
dat <- dat %>% arrange(year)
dat <- na.omit(dat)
```
Creamos las variables de series de tiempo $PIBpc$ y $C1PIBpc$ y gráficamos $C1PIBpc$:
``` r
PIBpc <- ts(dat$NY.GDP.PCAP.KN, frequency = 1, start = c(1960)) # Creamos la variable PIBpc

C1PIBpc <- diff(PIBpc, differences = 1) # Creamos la variable C1PIBpc

df <- data.frame(Año = time(C1PIBpc), C1PIBpc = as.numeric(C1PIBpc)) # Convertimos C1PIBpc a un dataframe

# Gráfico de C1PIBpc
ggplot(df, aes(x = Año, y = C1PIBpc)) + 
  geom_line(linewidth = 0.2) + 
  labs(subtitle = "1961-2019", y = "Pesos constantes", 
       title = "Cambio en el PIB per cápita real de Colombia", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5))
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/249d7c19-9de7-480b-b755-b91e5ac03a55)

Del gráfico de $C1PIBpc$ ya hablamos en la subsección 2.2.5, por lo que no diremos nada mas al respecto. Ahora vamos a comenzar a aplicar la metodología de tres pasos de Box y Jenkins para identificar el modelo que vamos a utilizar en el pronóstico de la variable PIBpc para el periodo 2020-2025.

## 1) Identificación:
Gráficamos las $FAC$ y $FACP$ muestrales de la variable **C1PIBpc** utilizando el comando _autoplot_[^1]

[^1]: **Observe que el comando _autoplot_ no gráfica las funciones para el rezago 0 porque es redundante hacerlo ta que siempre valen 1**

``` r
acf_plot <- autoplot(acf(C1PIBpc, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(C1PIBpc, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/490cc0c5-6a52-41b7-81af-ad396f61c581)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5653ea93-2a6f-4798-b893-cd2c46566cda)



## 2) Estimación:
La $FACP$ da a entender que la serie de $C1PIBpc$ sigue un proceso autorregresivo de orden $1$, y la $FAC$ revela la existencia de un comportamiento de media movil de orden $1$. En consecuencia, vamos a estimar diferentes modelos $ARIMA(p,1,q)$ para la variable $PIBpc$ [^1], para evaluar cuál especificación es más parsimoniosa. Para ello se copian los siguientes comandos: [^2]

[^1]: **Recuerden que un modelo _ARIMA(p,1,q)_ para la variable _PIBpc_ es es equivalente a un modelo _ARIMA(p,0,q)_ o _ARMA(p,q)_ para la variable _C1PIBpc_.**

[^2]: **Observen que los diferentes modelos _ARIMA(p,1,q)_ los estimamos con y sin intercepto, para determinar si la presencia del mismo aumenta la parsimonia del modelo. Recuerden que el argumento include.drift=TRUE implica que se esta asumiendo que la variable integrada de orden _1_ en niveles depende de una tendencia, por lo que al aplicarle la primera diferencia esa tendencia se vuelve una constante**

``` r
arima1<- Arima(PIBpc, order=c(0,1,2))
arima2<- Arima(PIBpc, order=c(1,1,0))  # Este es uno de los modelos que parecen identificar las FAC y FACP
arima3<- Arima(PIBpc, order=c(1,1,2))  
arima4<- Arima(PIBpc, order=c(1,1,1))  
arima5<- Arima(PIBpc, order=c(0,1,1))

arima1d<- Arima(PIBpc, order=c(0,1,2), include.drift=TRUE)
arima2d<- Arima(PIBpc, order=c(1,1,0), include.drift=TRUE)  # Este es uno de los modelos que parecen identificar las FAC y FACP
arima3d<- Arima(PIBpc, order=c(1,1,2), include.drift=TRUE)  
arima4d<- Arima(PIBpc, order=c(1,1,1), include.drift=TRUE)  
arima5d<- Arima(PIBpc, order=c(0,1,1), include.drift=TRUE)

AIC(arima1,arima2,arima3,arima4,arima5,arima1d,arima2d,arima3d,arima4d,arima5d)
BIC(arima1,arima2,arima3,arima4,arima5,arima1d,arima2d,arima3d,arima4d,arima5d)
```
Obteniendose,
``` r
> AIC(arima1,arima2,arima3,arima4,arima5,arima1d,arima2d,arima3d,arima4d,arima5d)
        df      AIC
arima1   3 1629.190
arima2   2 1617.199
arima3   4 1618.947
arima4   3 1618.328
arima5   2 1633.899
arima1d  4 1614.331
arima2d  3 1611.045
arima3d  5 1614.947
arima4d  4 1613.007
arima5d  3 1614.027
> BIC(arima1,arima2,arima3,arima4,arima5,arima1d,arima2d,arima3d,arima4d,arima5d)
        df      BIC
arima1   3 1635.423
arima2   2 1621.354
arima3   4 1627.257
arima4   3 1624.561
arima5   2 1638.054
arima1d  4 1622.641
arima2d  3 1617.277
arima3d  5 1625.335
arima4d  4 1621.317
arima5d  3 1620.260
```
Por consiguiente, según el _Criterio de Información de Akaike_ (AIC) y el _Criterio Bayesiano de Schwartz_ (BIC), dentro de los modelos analizados, el modelo más parsimonioso es el **modelo $ARIMA(1,1,0)$ con intercepto** [el cual, dentro del código de $R$ lo hemos llamado **arima2d**]. Más específicamente, los valores que se obtuvieron con este modelo son: $AIC=1611.045$ y $BIC=1617.277$. 

``` r
auto.arima(PIBpc, stepwise = FALSE, approximation = FALSE, ic="aic")
auto.arima(PIBpc, stepwise = FALSE, approximation = FALSE, ic="bic")
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
En este caso la función certifica que el mejor modelo que representa a la función PIBpc sería un $ARIMA(1,1,0)$ con intercepto.[^3]
[^3]: **Observen que si se hace el mismo analisis para la variable C1PIBpc obtienen que el mejor modelo ARIMA es el modelo _ARIMA(1,0,0)_**

Ahora, sabiendo cuál es el modelo más parsimonioso, procedemos a visualizar la estimación del modelo escogido, $y_t=a_0+a_1y_{t-1}+\varepsilon_t$, con el comando:
``` r
summary(arima2d)
```
Obteniendose

``` r
> summary(arima2d)
Series: PIBpc 
ARIMA(1,1,0) with drift 

Coefficients:
         ar1      drift
      0.5256  205388.50
s.e.  0.1087   52528.73

sigma^2 = 3.933e+10:  log likelihood = -802.52
AIC=1611.04   AICc=1611.48   BIC=1617.28

Training set error measures:
                   ME     RMSE      MAE         MPE     MAPE      MASE       ACF1
Training set 1266.823 193299.6 142613.8 -0.08571389 1.360031 0.5900399 0.01941263
```

Note que el coeficiente estimado $\hat{a}_1=0.7326$ del modelo $ARIMA(1,1,0)$ es estadísticamente significativo (es decir, es más de dos veces superior al valor de su error estándar de $0.0856$). Algo similar se puede decir para el coeficiente estimado $\hat{a}_0$

Como último paso, para contrastar nuestros resultados acerca del modelo más parsimonioso utilizamos los comandos:
``` r
auto.arima(PIBpc, ic = c("aic"), stepwise = FALSE, approximation = FALSE)
auto.arima(PIBpc, ic = c("bic"), stepwise = FALSE, approximation = FALSE)
```
Al operar ambos comandos, volvemos a obtener el mismo resultado: **El mejor modelo $ARIMA(1,1,0)$ es el mejor modelo que se ajusta a los datos**

## 3) Verificación de diagnóstico:
El último paso de Box-Jenkins consiste en hacer las pruebas de validación del modelo $ARIMA(1,1,0)$. Para ello en primer lugar se grafican los correlogramas de los residuos estimados para comprobar que son ruido blanco:

``` r
acf_plot <- autoplot(acf(arima2d$residuals, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(arima2d$residuals, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/c7ba9561-94d2-4657-a740-c7dc2544a020)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/c591440a-4a94-4a8b-8fa6-1983e7eef53b)

Se puede apreciar en las $FAC$ y en las $FACP$ que no hay ningún rezago significativo que denote algún tipo de estructura $ARMA$ adicional, por lo tanto podemos decir que los residuos son ruido blanco. 

Para corroborar aún más este resultado, vamos a utilizar un comando para visualizar en gráficos los residuos estimados estandarizados y los _p-values_ de la prueba de Ljung-Box sobre estos residuos: [^2]

[^2]: También se visualiza la _FAC_ de los residuos, pero esto ya la habiamos analizado previamente.

``` r
ggtsdiag(arima2d, gof.lag = 30) +  labs(subtitle = "arima2d") # Con este comando se pueden hacer pruebas sobre los residuos del arima2d.
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5c487f6a-f726-46d7-84cf-2de088626ee0)

El gráfico de los residuos no muestra señales de autocorrelación y esto se confirma en el gráfico de los _p-values_ de la prueba de Ljung-Box en donde para todos los rezagos no se rechaza la hipótesis nula de no autocorrelación en los residuos. En particular, note que el _p-value_ más bajo corresponde al rezago 18, entonces vamos a utilizar el siguiente comando para conocer exactamente su valor: 

``` r
lb <- Box.test(arima2d$residuals, lag=18, type="Ljung-Box") # Test de Ljung-Box
lb$p.value
```

Obteniendo
``` r
> lb$p.value
[1] 0.3817737
```
Como $0.38>0.10$ entonces no se rechaza la hipótesis nula aun al 10% de significancia. En consecuencia, se certifica nuevamente que los residuos son ruido blanco.

## Pronóstico

Ahora vamos a hacer un pronostico de tres periodos (correspondiente al periodo 2020-2022) con la especificación $ARIMA$ escogida
``` r
forecast1<-forecast(arima2d, level = c(95), h = 3)
summary(forecast1)
autoplot(forecast1)
```
Obteniendo:
``` r
> summary(forecast1)

Forecast method: ARIMA(1,1,0) with drift

Model Information:
Series: PIBpc 
ARIMA(1,1,0) with drift 

Coefficients:
         ar1      drift
      0.5256  205388.50
s.e.  0.1087   52528.73

sigma^2 = 3.933e+10:  log likelihood = -802.52
AIC=1611.04   AICc=1611.48   BIC=1617.28

Error measures:
                   ME     RMSE      MAE         MPE     MAPE      MASE       ACF1
Training set 1266.823 193299.6 142613.8 -0.08571389 1.360031 0.5900399 0.01941263

Forecasts:
     Point Forecast    Lo 95    Hi 95
2020       17775884 17387182 18164587
2021       17987489 17278445 18696534
2022       18196145 17199508 19192782
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/26deeca9-b18b-401a-ba0f-18f84c9487b8)


Noten que la tendencia de la economía antes de la pandemia del Covid-19 era creciente, pero con una tendencia crecimiento había disminuido en los últimos años. Este comportamiento se trasladó al compartamiento de las proyecciones del $PIBpc$. En la siguiente tabla contrastamos las proyecciones del PIB per cápita real (y su banda de confianza al 95%) con los resultados reales durante el periodo en cuention:

| Año    | Dato Real | Pronóstico | Piso de la <br> banda de confianza | Techo de la <br> banda de confianza |
|:------:|:---------:|:----------:|:----------------------------------:|:-----------------------------------:| 
| $2019$ |$17558668$ |            |                                    |                                     | 
| $2020$ |$16047602$ |$17775884$  | $17387182$                         | $18164587$                          | 
| $2021$ |$17612821$ |$17987489$  | $17278445$                         | $18696534$                          | 
| $2022$ |$18802572$ |$18196145$  | $17199508$                         | $19192782$                          | 

Durante el año $2020$, a raíz de la cuarentena producto de la pandemia del Covid-19 se dio un crecimiento negativo de la economía que llevo a situar a la producción real por debajo de su valor tendencial. Sin embargo, durante los años $2021$ y $2022$ se dió un repunte bastante fuerte en la producción, colocando a la producción real por encima de su valor promedio de diagnóstico (pero, aún dentro de la banda de confianza del análisis)   


---
---
# Preguntas de selección múltiple

Cómo se comentó en la subsección 2.2.5, en esta sesión de preguntas se va a continuar con el análisis de la variable **Gasto en consumo final del gobierno general de Chile como % del PIB** la cual ya se habia demostrado que es integrada de orden "1". Por lo tanto, van a aplicarle la metodología de Box y Jentins a la primera diferencia de esta variable, la cual se ha denominado como $C1GGOV$

Retomen parte del código utilizado en secciones previas para preparar los datos para el análisis:

``` r
library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')

dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS"), country=c('CL'), language = "es")

dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, GGOV_lag1 = lag(GGOV, order_by = year), C1GGOV = GGOV - GGOV_lag1, country = NULL, iso2c = NULL, iso3c = NULL) # mutate lo utilizamos para adicionar las columnas GGOV_lag1 y C1GGOV en "dat" y para eliminar las columnas country, iso2c y iso3c

ggplot(dat, aes(year, GGOV)) + 
  geom_line(linewidth=0.2) + 
  scale_x_continuous(name = "Años") + 
  theme(plot.caption = element_text(size=7)) + 
  labs(subtitle = "1960-2022", y = "%", title = "Gasto en el consumo final del gobierno general de Chile como % del PIB", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

ggplot(dat, aes(year, C1GGOV)) + 
  geom_line(linewidth=0.2) + 
  scale_x_continuous(name = "Años") + 
  theme(plot.caption = element_text(size=7)) + 
  labs(subtitle = "1961-2022", y = "%", title = "Cambio en el gasto en el consumo final del gobierno general de Chile como % del PIB", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

GGOV <- ts(dat$GGOV, frequency = 1, start = c(1960)) # Creamos la variable GGOV como serie de tiempo
C1GGOV <- diff(GGOV, differences = 1) # Creamos la variable C1PIBpc como serie de tiempo 

```

1. **Gráfiquen la $FAC$ y $FACP$ muestrales de la variable $C1GGOV$. En su orden, ¿cuántos barras, superiores a la del rezago $0$, de ambas funciones se salen por fuera de la banda de confianza?**:
 
   a) 0 y 0.

   b) 1 y 0.

   c) 0 y 1.

   d) 1 y 1.

``` r
acf_plot <- autoplot(acf(C1GGOV, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(C1GGOV, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```

2. **La variable $C1GGOV$ pareciera no estar correlacionada con su pasado. Sin embargo, se podría ser más exigente en cuanto al ancho de las bandas de la $FAC$ y la $FACP$ de tal formas que estas sean algo más angostas. y eso lo llevaría a pensar que el modelo de series de tiempo a escoger para analizar $C1GGOV$ sea un $AR(1)$, un $MA(1)$ o un $ARMA(1)$, entonces estime estos tres modelos. ¿Cuál especificación da el $Criterio de Información de Akaike$ y $Criterio Bayesiano de Schwartz$ con los valores más bajos?**:

   a) $AR(1)$.

   b) $AR(1)$ con tendencia en $GGOV$.

   c) $MA(1)$.

   d) $MA(1)$ con tendencia en $GGOV$.

   e) $ARMA(1,1)$.

   f) $ARMA(1,1)$ con tendencia en $GGOV$.

``` r
arima1<- Arima(GGOV, order=c(1,1,0))
arima2<- Arima(GGOV, order=c(0,1,1))  
arima3<- Arima(GGOV, order=c(1,1,1))  

arima1d<- Arima(GGOV, order=c(1,1,0), include.drift=TRUE)
arima2d<- Arima(GGOV, order=c(0,1,1), include.drift=TRUE)  
arima3d<- Arima(GGOV, order=c(1,1,1), include.drift=TRUE)  
```

3. **Según el comando auto.arima cuál debería ser la especificación escogida?**:

``` r
auto.arima(GGOV, stepwise = FALSE, approximation = FALSE, ic="aic")
auto.arima(GGOV, stepwise = FALSE, approximation = FALSE, ic="bic")
```

   a) $AR(1)$.

   b) $AR(1)$ con tendencia en $GGOV$.

   c) $MA(1)$.

   d) $MA(1)$ con tendencia en $GGOV$.

   e) $ARMA(1,1)$.

   f) $ARMA(1,1)$ con tendencia en $GGOV$.

4. **Utilizando el comando ggtsdiag, gráfique las pruebas de diagnóstico sobre los errores estimados del modelo escogido y responda ¿En cuál año los residuos estandarizados tuvieron un valor más bajo.**:

   a) $1960$.

   b) $1980$.

   c) $1990$.

   d) $2000$.

   e) $2010$.

   f) $2020$.

``` r
ggtsdiag(arima1, gof.lag = 30)
```
5. **¿En cuál rezago la prueba de Ljung-Box sobre los residuos estinados tuvó el p-value más alto? ¿Cuánto valía ese p-value (a dos dígitos)?**:

   a) $1$ en $0.89$.

   b) $2$ en $0.19$.

   c) $3$ en $0.24$.

   d) $4$ en $0.23$.

``` r
ggtsdiag(arima1, gof.lag = 30) 

lb1 <- Box.test(arima1$residuals, lag=1, type="Ljung-Box") # Test de Ljung-Box
lb2 <- Box.test(arima1$residuals, lag=2, type="Ljung-Box") # Test de Ljung-Box
lb3 <- Box.test(arima1$residuals, lag=3, type="Ljung-Box") # Test de Ljung-Box
lb4 <- Box.test(arima1$residuals, lag=4, type="Ljung-Box") # Test de Ljung-Box

lb1$p.value
lb2$p.value
lb3$p.value
lb4$p.value
```
5. **¿Cuáles son los pronósticos del valor de $GGOV$ para 2023 y 2024 a dos dígitos?**:

   a) $14.41 y $14.41$.

   b) $15.12$ y $14.81$.

   c) $14.81$ y $15.12$.

   d) $15.12$ y $15.12$.

``` r
forecast_GGOV<-forecast(arima1, level = c(95), h = 2)
summary(forecast_GGOV)
autoplot(forecast_GGOV)
```


---
---


| [Anterior Subsección: 2.3.2. Estimando un _ARMA_ - Ejemplo simulado](../Seccion02_03_02/Readme.md) | [Subsección 2.3 - Análisis _ARMA_ (La Metodología de Box-Jenkins)](../Readme.md) |
|----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>




