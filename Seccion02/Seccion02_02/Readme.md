# Pruebas de Raíz Unitaria

La siguiente tabla presenta las pruebas que vamos a analizar. Dando click en la **X** de la segunda columna de la tabla puede redirigirse a la explicación teórica de la prueba y dando click en la **X** de la tercera columna de la tabla puede redirigirse para ver cómo la misma se lleva a cabo en R.  

| Pruebas                                                                                 | Explicación general de la prueba      |  Aplicación en R                     |
|-----------------------------------------------------------------------------------------|:-------------------------------------:|:------------------------------------:|
| Las Pruebas DF y ADF: _Pruebas de Dickey Fuller y Aumentada de Dickey Fuller_           |  [X](Seccion02_02_ADF_T/Readme.md)    | [X](Seccion02_02_ADF_R/Readme.md)    | 
| La Prueba ADF-GLS: _Prueba de Elliott, Rothenberg y Stock_                              |  [X](Seccion02_02_ADFGLS_T/Readme.md) | [X](Seccion02_02_ADFGLS_R/Readme.md) |
| La Prueba KPSS: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_                       |  [X](Seccion02_02_KPSS_T/Readme.md)   | [X](Seccion02_02_KPSS_R/Readme.md)   |   
| Pruebas de Cambio Estructural: _Prueba de Perron y _Prueba de Zivot y Andrews_          |  [X](Seccion02_02_ZA_T/Readme.md)     | [X](Seccion02_02_ZA_R/Readme.md)     |

A continuación, se presenta un ejemplo utilizando la base de datos "Indicadores de Desarrollo Económico del Banco Mundial". Para el ejercicio, se ha escogido analizar la variable más utilizada en términos de desarrollo económico, el PIB per cápita, de un país en vías de desarrollo. Más específicamente, se va a analizar la evolución del PIB per cápita de Colombia a precios constantes.

## Ejemplo: PIB per cápita de Colombia

La prueba se va a hacer con respecto al **PIB per cápita de Colombia a precios constantes en pesos durante el periodo 1960-2019** en niveles y en primeras diferencias. No se utiliza la información hasta 2022 por motivos didacticos; en particular, en 2020 (es decir, en la parte final de la base de datos) hay un cambio estructural bastante fuerte de un solo periodo, a raíz de la cuarentena producto de la epidemia del Covid-19, que vuelve algunas pruebas más engorrosa de interpretar. Sin embargo, ello no quita que también se puedan implementar las pruebas de raíz unitaria hasta 2022.

Retomamos parte del código de R que se había utilizado en la sección 01-02, solicitando la activación de algunas librerias adicionales: 
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

ggplot(ejercicio, aes(year, PIBpc)) + geom_line (linewidth=0.2) + labs(subtitle="$", y="Pesos constantes", x="Años", title="PIB per cápita real de Colombia", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Económico del Banco Mundial")

ggplot(ejercicioC1, aes(year, C1PIBpc)) + geom_line (linewidth=0.2) + labs(subtitle="$", y="Pesos constantes", x="Años", title="Cambio en el PIB per cápita real de Colombia", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Económico del Banco Mundial")

```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/a9cf6e33-1204-405a-865d-2b55d90b3f1b)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/771298eb-e6d5-49b4-96bd-6252b91e3493)

Para el análisis de series de tiempo que viene, es necesario establecer las variables a analizar como una serie de tiempo, así:
``` r
PIBpc_ = subset(ejercicio, select = c(PIBpc))
C1PIBpc_ = subset(ejercicioC1, select = c(C1PIBpc))
PIBpc <- ts(PIBpc_, start=1960, end=2020)
C1PIBpc <- ts(C1PIBpc_, start=1961, end=2020)
```
Por otro lado, a partir de los gráficos se puede comenzar a inferir que la variable $PIBpc$ no tiene un comportamiento estacionario y que la variable $C1PIBpc$ si lo tiene. Sin embargo, hay que recolectar más evidencia al respecto, para ello primero se visualiza la función de autocorrelación de las variables $PIBpc$ y $C1PIBpc$ utilizando el siguiente comando: 

Para el gráfico de la $FAC$ se ejecuta el comando
``` r
autoplot(acf(PIBpc, plot = FALSE))
autoplot(acf(C1PIBpc, plot = FALSE))
```
Obteniendose
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/e48c24fd-7cc2-481e-a51f-3e81a71e22ae)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/bde961de-2060-425e-9504-d7c71846de8e)

El decaimiento continuo pero moderado de la $FAC$ de la variable $PIBpc$ da una idea de raíz unitaria. Por otro lado, la $FAC$ de la variable $C1PIBpc$ desde el comienzo cae raídamente, dando así una idea de estacionariedad. 

Antes de desarrollar las diferentes pruebas de raíz unitaria, se desarrolla la siguiente prueba sencilla de Mínimos Cuadrados Ordinarios para analizar si es necesario incluirles la tendencia y el intercepto a las diferentes pruebas de raíz unitaria.

``` r
PIBpc_MCO <- lm(PIBpc ~ year, data = ejercicio)
C1PIBpc_MCO <- lm(C1PIBpc ~ year, data = ejercicioC1)
summary(PIBpc_MCO)
summary(C1PIBpc_MCO)
```
Obteniendose, 
``` r
> summary(PIBpc_MCO)

Call:
lm(formula = PIBpc ~ year, data = ejercicio)

Residuals:
     Min       1Q   Median       3Q      Max 
-1753296  -382467      993   432933  1682154 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) -379979406   12091757  -31.43   <2e-16 ***
year            196216       6078   32.28   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 815300 on 58 degrees of freedom
Multiple R-squared:  0.9473,	Adjusted R-squared:  0.9464 
F-statistic:  1042 on 1 and 58 DF,  p-value: < 2.2e-16

> summary(C1PIBpc_MCO)

Call:
lm(formula = C1PIBpc ~ year, data = ejercicioC1)

Residuals:
    Min      1Q  Median      3Q     Max 
-907232 -108680    8272  128200  541135 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)  
(Intercept) -7204478    3429180  -2.101   0.0401 *
year            3724       1723   2.161   0.0349 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 225400 on 57 degrees of freedom
Multiple R-squared:  0.07575,	Adjusted R-squared:  0.05953 
F-statistic: 4.671 on 1 and 57 DF,  p-value: 0.03488
```
Note que al 1% de significancia la variable $PIBpc$ tiene constante y tendencia significativas y la variable $C1PIBpc$ no las tiene. Por lo tanto, en la medida de lo posible, las pruebas de raíz unitaria de la variable $PIBpc$ se analizaran con intercepto y tendencia, mientras que los de la variable $C1PIBpc$ se analizaran sin intercepto y sin tendencia.[^1]
[^1]: **De todas formas, todas las pruebas de raíz unitaria se van a ser con diferentes variantes dentro de la prueba para que el alumno por su cuenta pueda contrastar los resultados de las mismas.**

A continuación se desarrollan diferentes pruebas de raíz unitaria. Primero, para la variable $PIBpc$ y luego para la variable $C1PIBpc$.

*****************************************************************************************************************************************************************************
En cuanto a la prueba ADF, el número óptimo de rezagos, para cada especificación, se va a escoger tomando en cuenta el Criterio de Información de Akaike y el Criterio Bayesiano de Schwartz.

### 1) Prueba ADF

``` r
PIBpc_ur_trend_AIC.df <- ur.df(y=PIBpc, type = c("trend"), lags = 10, selectlags = c("AIC"))
PIBpc_ur_trend_BIC.df <- ur.df(y=PIBpc, type = c("trend"), lags = 10, selectlags = c("BIC"))
C1PIBpc_ur_none_AIC.df <- ur.df(C1PIBpc, type = c("none"), lags = 10, selectlags = c("AIC"))
C1PIBpc_ur_none_BIC.df <- ur.df(C1PIBpc, type = c("none"), lags = 10, selectlags = c("BIC"))

summary(PIBpc_ur_trend_AIC.df)
summary(PIBpc_ur_trend_BIC.df)
summary(C1PIBpc_ur_none_AIC.df)
summary(C1PIBpc_ur_none_BIC.df)
```
Obteniendose

``` r
> summary(PIBpc_ur_trend_AIC.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-9361209  -351413   -32426   587834  2646232 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)   
(Intercept)  4.124e+06  1.355e+06   3.044  0.00393 **
z.lag.1     -8.918e-01  3.137e-01  -2.843  0.00675 **
tt           1.422e+05  6.418e+04   2.216  0.03192 * 
z.diff.lag1  7.413e-01  1.124e+00   0.660  0.51281   
z.diff.lag2  5.557e-01  1.282e+00   0.433  0.66691   
z.diff.lag3  2.676e+00  1.202e+00   2.226  0.03120 * 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1634000 on 44 degrees of freedom
Multiple R-squared:  0.2391,	Adjusted R-squared:  0.1526 
F-statistic: 2.765 on 5 and 44 DF,  p-value: 0.02948


Value of test-statistic is: -2.8429 4.3493 6.0919 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -4.04 -3.45 -3.15
phi2  6.50  4.88  4.16
phi3  8.73  6.49  5.47

> summary(PIBpc_ur_trend_BIC.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
      Min        1Q    Median        3Q       Max 
-10820727   -188559     55160    274597   1802783 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)  
(Intercept)  2.849e+06  1.332e+06   2.139   0.0378 *
z.lag.1     -5.201e-01  2.947e-01  -1.765   0.0842 .
tt           7.674e+04  6.223e+04   1.233   0.2238  
z.diff.lag   1.276e+00  1.042e+00   1.225   0.2268  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 1721000 on 46 degrees of freedom
Multiple R-squared:  0.118,	Adjusted R-squared:  0.06051 
F-statistic: 2.052 on 3 and 46 DF,  p-value: 0.1197


Value of test-statistic is: -1.7651 2.0235 2.9459 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -4.04 -3.45 -3.15
phi2  6.50  4.88  4.16
phi3  8.73  6.49  5.47

> summary(C1PIBpc_ur_none_AIC.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
    Min      1Q  Median      3Q     Max 
-590917 -100077   58096  184819  598045 

Coefficients:
           Estimate Std. Error t value Pr(>|t|)  
z.lag.1    -0.24419    0.10487  -2.329   0.0242 *
z.diff.lag -0.08217    0.14539  -0.565   0.5746  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 228400 on 47 degrees of freedom
Multiple R-squared:  0.1406,	Adjusted R-squared:  0.1041 
F-statistic: 3.845 on 2 and 47 DF,  p-value: 0.0284


Value of test-statistic is: -2.3286 

Critical values for test statistics: 
     1pct  5pct 10pct
tau1 -2.6 -1.95 -1.61

> summary(C1PIBpc_ur_none_BIC.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
    Min      1Q  Median      3Q     Max 
-590917 -100077   58096  184819  598045 

Coefficients:
           Estimate Std. Error t value Pr(>|t|)  
z.lag.1    -0.24419    0.10487  -2.329   0.0242 *
z.diff.lag -0.08217    0.14539  -0.565   0.5746  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 228400 on 47 degrees of freedom
Multiple R-squared:  0.1406,	Adjusted R-squared:  0.1041 
F-statistic: 3.845 on 2 and 47 DF,  p-value: 0.0284


Value of test-statistic is: -2.3286 

Critical values for test statistics: 
     1pct  5pct 10pct
tau1 -2.6 -1.95 -1.61
```

Los resultados de las pruebas ADF son:

| Variable   | Estadístico | Criterio de <br> información | Número de <br> rezagos | Valor     |  1%     |  5%     |  10%    |
|------------|:-----------:|:----------------------------:|:----------------------:|:---------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $\tau_\tau$ |  Akaike                      |   3                    | $-2.8429$ | $-4.04$ | $-3.45$ | $-3.15$ |
| $PIBpc$    | $\phi_2$    |  Akaike                      |   3                    |  $4.3493$ |  $6.50$ |  $4.88$ |  $4.16$ |
| $PIBpc$    | $\phi_3$    |  Akaike                      |   3                    | $ 6.0919$ |  $8.73$ |  $6.49$ |  $5.47$ |
| $PIBpc$    | $\tau_\tau$ |  Schwartz                    |   1                    | $-1.7651$ | $-4.04$ | $-3.45$ | $-3.15$ |
| $PIBpc$    | $\phi_2$    |  Schwartz                    |   1                    |  $2.0235$ |  $6.50$ |  $4.88$ |  $4.16$ |
| $PIBpc$    | $\phi_3$    |  Schwartz                    |   1                    |  $2.9459$ |  $8.73$ |  $6.49$ |  $5.47$ |
| $C1PIBpc$  | $\tau$      |  Akaike                      |   1                    | $-2.3286$ | $-2.60$ | $-1.95$ | $-1.61$ |
| $C1PIBpc$  | $\tau$      |  Schwartz                    |   1                    | $-2.3286$ | $-2.60$ | $-1.95$ | $-1.61$ |

En consecuencia, la variable $PIBpc$ es no estacionaria porque:
* $\tau_\tau$ = -3.15 < **-2.8429 (Akaike)** < **-1.7651 (Shwartz)**, es decir, no se rechaza $\gamma=0$ al 10%, 5% y 1%
* $\phi_2$ = **2.0235 (Shwartz)** < **4.3493 (Akaike)** < 4.88, es decir, no se rechaza $\gamma=a_0=a_2=0$ al 10%, 5% y 1%
* $\phi_3$ = 6.49<**6.1099**<8.73, es decir, no se rechaza $\gamma=a_2=0$ al 1% pero si se rechaza al 5% y 10%

Y la variable $C1PIBpc$ es estacionaria porque
* $\tau$ = -4.308< **-2.3286** <-2.60, es decir, no se rechaza $\gamma=0$ al 1% pero si se rechaza al 5% y 10%

Ahora copie los comandos
``` r
PIBpc_urdfTest<- urdfTest(PIBpc, lags = 3, type = c("ct"), doplot = TRUE)
C1PIBpc_urdfTest<- urdfTest(C1PIBpc, lags = 1, type = c("nc"), doplot = TRUE)
```  
Obteniendose los siguientes gráficos de prueba sobre los errores de estimación en la prueba,

1) Para $PIBpc$
   
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4a7e542c-98a2-42b1-ab94-458640b56352)


2) Para $C1PIBpc$
   
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/6311e9ff-3bc6-4a4e-9802-5ea2fb825685)


En consecuencia, los gráficos $FAC$ y $FACP$ revelan que los errores son ruidos blanco en ambas pruebas, por lo tanto ambas pruebas estan bien hechas.

*****************************************************************************************************************************************************************************

### Prueba DF-GLS

Dado que el $PIBpc$ es una variable que presenta tendencia, entonces se va a interpretar su prueba DF-GLS con tendencia. Dado que el $C1PIBpc$ es una variable que no presenta tendencia, entonces se va a interpretar su prueba DF-GLS  sin tendencia. De todas formas abajo se presentan los resultados con ambas pruebas y el número de rezagos se escoge utilizando el Criterio Bayesiano de Schwartz.

``` r
PIBpc_DFGLSc.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("constant"),lag.max = 4) 
PIBpc_DFGLSt.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("trend"),lag.max = 4)
C1PIBpc_DFGLSc.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("constant"),lag.max = 4) 
C1PIBpc_DFGLSt.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("trend"),lag.max = 4)

summary(PIBpc_DFGLSc.ers)
summary(PIBpc_DFGLSt.ers)
summary(C1PIBpc_DFGLSc.ers)
summary(C1PIBpc_DFGLSt.ers)
```

``` r
> summary(PIBpc_DFGLSc.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept 

Value of test-statistic is: 30.3276 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 1.95 3.11  4.17

> summary(PIBpc_DFGLSt.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept and trend 

Value of test-statistic is: 34.0584 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 4.26 5.64  6.79

> summary(C1PIBpc_DFGLSc.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept 

Value of test-statistic is: 0.319 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 1.95 3.11  4.17

> summary(C1PIBpc_DFGLSt.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept and trend 

Value of test-statistic is: 1.0387 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 4.26 5.64  6.79
```

| Variable   | Estadistico   | Valor     |  10%    |  5%     |   1%    |
|------------|---------------|:---------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $P_\tau$      | $34.0584$ | $6.79$  | $5.64$  | $4.26$  |
| $C1PIBpc$  | $P_\mu$       | $0.319$   | $4.17$  | $3.11$  | $1.95$  |

La variable $PIBpc$ es no estacionaria porque:
* Con el estadistico $P_\tau$: $**34.0584**>6.79$, no se rechaza la hipótesis nula de raíz unitaria  10%, 5% y 1% 

La variable $C1PIBpc$ es estacionaria porque:
* Con el estadistico $P_\mu$: $**0.319**<1.95$, se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%

### Prueba KPSS
Vamos a aplicar las opciones de rezago de R. Sin embargo, no olviden que según  en Newey y West (1994) la longitud de rezago se establece proporcional a $T^{1/3}$, en decir $60^{1/3}=3.92 \sim 4$, en nuestro caso: .

``` r
PIBpc_1t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("short"), use.lag = NULL)
PIBpc_2t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("long"), use.lag = NULL)
PIBpc_3t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("nil"), use.lag = NULL)
PIBpc_4t.kpss <- ur.kpss(PIBpc, type = c("tau"), use.lag = 4)

C1PIBpc_1m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("short"), use.lag = NULL)
C1PIBpc_2m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("long"), use.lag = NULL)
C1PIBpc_3m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("nil"), use.lag = NULL)
C1PIBpc_4m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), use.lag = 4)

summary(PIBpc_1t.kpss)
summary(PIBpc_2t.kpss)
summary(PIBpc_3t.kpss)
summary(PIBpc_4t.kpss)
summary(C1PIBpc_1m.kpss)
summary(C1PIBpc_2m.kpss)
summary(C1PIBpc_3m.kpss)
summary(C1PIBpc_4m.kpss)
```

Obteniendo
``` r
> summary(PIBpc_1t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 3 lags. 

Value of test-statistic is: 0.0642 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_2t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 10 lags. 

Value of test-statistic is: 0.0729 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_3t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 0 lags. 

Value of test-statistic is: 0.0857 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_4t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 4 lags. 

Value of test-statistic is: 0.0619 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(C1PIBpc_1m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 3 lags. 

Value of test-statistic is: 0.2501 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_2m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 10 lags. 

Value of test-statistic is: 0.2202 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_3m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 0 lags. 

Value of test-statistic is: 0.5362 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_4m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 4 lags. 

Value of test-statistic is: 0.2339 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739
```
Dado que el $PIBpc$ es una variable que presenta tendencia, entonces se decide testear dicha prueba. Los resultados de la misma son:

| Variable | Estadistico | Rezagos        | Valor      |  10%    |  5%     |  2.5%   |  1%     |
|----------|-------------|:--------------:|:----------:|:-------:|:-------:|:-------:|:-------:|
| PIBpc    | $KPSS_\tau$ |  nil= $0$      |  $0.0857$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  short= $3$    |  $0.0642$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  NW(1994)= $4$ |  $0.0619$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  long= $10$    |  $0.0729$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| C1PIBpc  | $KPSS_\mu$  |  nil= $0$      |  $0.5362$  | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  short= $3$    |  $0.2501$  | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  NW(1994)= $4$ |  $0.2339$  | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  long= $10$    |  $0.2202$  | $0.347$ | $0.463$ | $0.574$ | $0.739$ |

La variable $PIBpc$ es no estacionaria porque:
* Cuando el número de rezagos es el establecido por la formula de Newey y West (1994)= $4$ se obtiene: $**0.0619**<0.119$, no se rechaza la hipótesis nula de estacionariedad al 1%, 2.5%, 5% y 10%

La variable $C1PIBpc$ es estacionaria porque:
* Cuando el número de rezagos es el establecido por la formula de Newey y West (1994)= $4$ se obtiene: $**0.2339**<0.347$, no se rechaza la hipótesis nula de estacionariedad al 1%, 2.5%, 5% y 10%

### Prueba ZA
``` r
ZA1.za <- ur.za(PIBpc, model = c("intercept"), lag=NULL)
ZA2.za <- ur.za(PIBpc, model = c("trend"), lag=NULL)
ZA3.za <- ur.za(PIBpc, model = c("both"), lag=NULL)

summary(ZA1.za)
summary(ZA2.za)
summary(ZA3.za)
```
Obteniendo
``` r
> summary(ZA1.za)
> summary(ZA1.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
     Min       1Q   Median       3Q      Max 
-1601711  -109518     5583   160822   893657 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.606e+06  4.139e+05   3.881 0.000273 ***
y.l1        6.986e-01  8.273e-02   8.443 1.27e-11 ***
trend       5.026e+04  1.384e+04   3.632 0.000604 ***
du          7.467e+05  2.284e+05   3.269 0.001833 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 325900 on 57 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9923,	Adjusted R-squared:  0.9919 
F-statistic:  2445 on 3 and 57 DF,  p-value: < 2.2e-16


Teststatistic: -3.6435 
Critical values: 0.01= -5.34 0.05= -4.8 0.1= -4.58 

Potential break point at position: 50 

> summary(ZA2.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
     Min       1Q   Median       3Q      Max 
-1773789  -118649    -1243   157165   722589 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.504e+06  4.429e+05   3.396  0.00125 ** 
y.l1        7.280e-01  8.521e-02   8.544 8.66e-12 ***
trend       4.305e+04  1.352e+04   3.183  0.00236 ** 
dt          4.966e+04  1.826e+04   2.719  0.00866 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 334100 on 57 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9919,	Adjusted R-squared:  0.9915 
F-statistic:  2325 on 3 and 57 DF,  p-value: < 2.2e-16


Teststatistic: -3.192 
Critical values: 0.01= -4.93 0.05= -4.42 0.1= -4.11 

Potential break point at position: 42 

> summary(ZA3.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
     Min       1Q   Median       3Q      Max 
-1834422   -94403    -2496   165996   513979 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.908e+06  4.627e+05   4.123 0.000125 ***
y.l1         6.300e-01  9.451e-02   6.666 1.23e-08 ***
trend        6.420e+04  1.694e+04   3.790 0.000370 ***
du          -5.475e+05  2.252e+05  -2.432 0.018255 *  
dt           7.178e+04  2.026e+04   3.543 0.000805 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 323800 on 56 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9925,	Adjusted R-squared:  0.992 
F-statistic:  1858 on 4 and 56 DF,  p-value: < 2.2e-16


Teststatistic: -3.9148 
Critical values: 0.01= -5.57 0.05= -5.08 0.1= -4.82 

Potential break point at position: 39 
```
| Estadistico      | Año del cambio <br> estructural |¿Es estadisticamente significativo <br> el cambio estructural? | Valor    |  10%    |  5%     |  1%     |
|------------------|:-------------------------------:|:---------------------------------------------------------:|:--------:|:-------:|:-------:|:-------:|
| $ZA_{Intercept}$ |  50                             | Si (du al 1%)                                             |$-3.6435$ | $-4.11$ | $-4.42$ | $-4.93$ |
| $ZA_{Trend}$     |  42                             | Si (dt al 1%)                                             |$-3.192$  | $-4.58$ | $-4.80$ | $-5.34$ |
| $ZA_{Both}$      |  39                             | Si (du al 5% y dt al 0.1%)                                |$-3.9148$ | $-4.82$ | $-5.08$ | $-5.57$ |

La variable es no estacionaria porque:
* Con el estadistico $ZA_{Intercept}$: $-3.6435>-4.11$, no se rechaza la hipótesis nula de raíz unitaria
* Con el estadistico $ZA_{Trend}$: $-3.192>-4.58$, no se rechaza la hipótesis nula de raíz unitaria
* Con el estadistico $ZA_{Both}$: $-3.9148>-4.82$, no se rechaza la hipótesis nula de raíz unitaria

Dado que practicamente en todas las pruebas se concluye que la variable $PIBpc$ tiene raíz unitaria, entonces se procerde a sacarle la primera diferencia para obtener la variable $\Delta PIBpc$ y a esta aplicarle las mismas pruebas.

# Referencias

* DICKEY, David y FULLER, Wayne. _Distribution of the Estimators for Autoregressive Time Series With a Unit Root_. Journal of the American Statistical Association, Vol. 74, No. 366 (June, 1979); p. 427-431 
* DICKEY, David y  FULLER, Wayne. _Likelihood Ratio Statistics for Autoregressive Time Series with a Unit Root_. Econometrica, Vol. 49, No. 4 (July, 1981); p. 1057-1072
* ELLIOTT, Graham; ROTHENBERG, Thomas y STOCK, James. _Efficient Tests for an Autoregressive Unit Root_. Econometrica, Vol. 64, No. 4 (July, 1996); p. 813-836
* KWIATKOWSKI, Denis; PHILLIPS, Peter; SCHMIDT, Peter y SHIN, Yongcheol. _Testing the Null Hypothesis of Stationarity against the Alternative of a Unit Root: How Sure are we that Economic Time Series Have a Unit Root?_ Journal of Econometrics, Vol. 54, No. 1–3 (October-December, 1992); p 159-178
* NEWEY, Whitney y WEST, Kenneth. _A Simple, Positive Semi-Definite, Heteroskedasticity and Autocorrelation Consistent Covariance Matrix_. Econometrica, Vol. 55, No. 3 (May, 1987); p. 703-708
* NEWEY, Whitney y WEST, Kenneth. _Automatic Lag Selection in Covariance Matrix Estimation_. The Re-view of Economic Studies, Vol. 61, No. 4 (October, 1994); p. 631–653
* PERRON, Pierre. _The Great Crash, the Oil Price Shock, and the Unit Root Hypothesis_. Econometrica, Vol. 57, No. 6 (November, 1989); p. 1361-1401
* PHILLIPS, Peter. _Time Series Regression with a Unit Root_. Econometrica, Vol. 55, No. 2 (March, 1987); p. 277-301 
* PHILLIPS, Peter y PERRON, Pierre.  _Testing for a Unit Root in Time Series Regression_. Biometrika Vol. 75, No. 2 (June, 1988); p. 335-346
* ZIVOT, Eric y ANDREWS, Donald. _Further Evidence on the Great Crash, the Oil-Price Shock, and the Unit-Root Hypothesis_. Journal of Business & Economic Statistics, Vol. 10, No. 3 (July, 1992); p. 251-270

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>



