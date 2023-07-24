<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SECCIÓN 2.2.5 
# Ejemplo utilizando la base de datos _Indicadores de Desarrollo Mundial_

Para el ejemplo, se ha escogido analizar la variable más utilizada en los análisis de desarrollo económico: el PIB per cápita a precios constantes de un país en vías de desarrollo. 

Más específicamente, se va a analizar la evolución del PIB per cápita de Colombia a pesos constantes. Más específicamente, utilizaremos el **PIB per cápita de Colombia a pesos constantes durante el periodo 1960-2019** (en niveles y en primeras diferencias)[^1] para calcular las proyecciones de esta variable durante el periodo 2020-2025 como si no se hubiera presentado la pandemia del Covid 19. Con ello, lo que se busca es analizar cómo se hubiera comportado esta variable, según su comportamiento histórico, si no hubiera ocurrido la cuarentena que se dio en debido a la pandemia del Covid-19.

[^1]: **Se va a hacer el análisis tanto en niveles como en primeras diferencias porque, tal como se mostrara más adelante, esta variable no es estacionaria en niveles, pero si lo va a ser en las primeras diferencias**

## Preparación de los datos y gráficos

Retomamos parte del código de $R$ que se había utilizado en la sección 1.2, solicitando la activación de algunas librerias adicionales: 
``` r
rm(list = ls())

library(WDI)         # Esta libreria sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta libreria permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggfortify)   # Esta libreria tienen comando utiles para plantear gráficos de series de tiempo, por ejemplo utilizando el comando autoplot()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(fUnitRoots)  # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(urca)        # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(forecast)    # Esta libreria sirve para hacer pronósticos.

WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')

dat = WDI(indicator= c(PIBpc = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, PIBpc_lag1 = lag(PIBpc, order_by = year), C1PIBpc=PIBpc-PIBpc_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

ggplot(dat, aes(year, PIBpc)) + scale_x_continuous(name="Años", limits=c(1960, 2019)) + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1960-2019", y="Pesos constantes", title="PIB per cápita real de Colombia", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

ggplot(dat, aes(year, C1PIBpc)) + scale_x_continuous(name="Años", limits=c(1961, 2019))  + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1961-2019", y="Pesos constantes", title="Cambio en el PIB per cápita real de Colombia", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

```
Obteniendose los siguientes gráficos:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/ed1aee77-0496-46b4-9be0-225a72022b05)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/971d2748-1b9c-4d4c-911f-8e3696749ee1)


Las siguientes cuatrolineas en $R$, permiten armar cuatro subconjuntos de bases de datos:
* uno en el cual se encuentre el PIB[^2] para el periodo 1960-2019 (subconjunto PIBpc_) y
* otro en el cual se encuentre la variación del PIB durante el periodo 1961-2019 (subconjunto C1PIBpc_) [^3]
* los subconjuntos PIBpc__ y C1PIBpc__ son dos subconjuntos iguales a PIBpc_ y a C1PIBpc_, con la diferencia que además incluyen la variale "year". Estos dos subconjuntos son utilizados para estimar una prueba MCO que se explicara más adelante. 

[^2]: **En lo que resta de esta sección, cuando hagamos referencia al PIB, estaremos haciendo referencia de una forma simplificada al PIB de Colombia a precios constantes** 
[^3]: **Se toma el periodo desde 1961 porque el dato de la variación del PIB en 1960 no se tiene porque se desconoce el valor del PIB de 1959** 

``` r
PIBpc__ <- dat[-c(61:63),]
C1PIBpc__ <- PIBpc__[-c(1),]
PIBpc_ = subset(PIBpc__, select = c(PIBpc))
C1PIBpc_ = subset(C1PIBpc__, select = c(C1PIBpc))
```

Para el análisis de raíz unitaria que viene, se va a llamar (desde la base de datos PIBpc_) a una variable llamada $PIBpc$ que representa una serie de tiempo anual (la del PIB) que va desde 1960 hasta 2019. Por otro lado, se va a llamar (desde la base de datos C1PIBpc_) a una variable llamada $C1PIBpc$ que representa una serie de tiempo anual (la del cambio en el PIB) que va desde 1961 hasta 2019.

``` r
PIBpc <- ts(PIBpc_, start=1960, end=2019)
C1PIBpc <- ts(C1PIBpc_, start=1961, end=2019)
```

# Análisis de raíz unitaria
A partir de los dos gráficos de arriba se puede comenzar a inferir que la variable $PIBpc$ no tiene un comportamiento estacionario y que la variable $C1PIBpc$ si lo tiene. Sin embargo, hay que recolectar más evidencia al respecto, para ello primero se visualiza la función de autocorrelación de las variables $PIBpc$ y $C1PIBpc$ utilizando el siguiente comando: 

Para el gráfico de la $FAC$ se ejecuta el comando
``` r
autoplot(acf(PIBpc, plot = FALSE))
autoplot(acf(C1PIBpc, plot = FALSE))
```
Obteniendose
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/e48c24fd-7cc2-481e-a51f-3e81a71e22ae)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/bde961de-2060-425e-9504-d7c71846de8e)

El decaimiento continuo pero moderado de la $FAC$ de la variable $PIBpc$ da una idea de raíz unitaria. Por otro lado, la $FAC$ de la variable $C1PIBpc$ desde el comienzo cae rapidamente, dando así una idea de estacionariedad. 

Antes de desarrollar las diferentes pruebas de raíz unitaria, observe a partir de los dos gráficos del comienzo de la sección, que el PIB tiene una tendencia clara, mientras que la variación del PIB no tiene tendencia y parece tampo tener un intercepto (aunque este último aspecto no es claro. Para visualizar mejor esto, se desarrolla la siguiente prueba sencilla de Mínimos Cuadrados Ordinarios con respecto a la ecuación $y_t=a_0+a_2t+e_t$ donde $e_t$ es el residuo de la ecuación. Los comandos para llevar a cabo la estimación, fuenon:

``` r
PIBpc_MCO <- lm(PIBpc ~ year, data = PIBpc__)
C1PIBpc_MCO <- lm(C1PIBpc ~ year, data = C1PIBpc__)
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
Note que al 1% de significancia la variable $PIBpc$ tiene constante y tendencia significativas y la variable $C1PIBpc$ no las tiene (claro esta que el intercepto de la variable $C1PIBpc$ es significativo al 10%). Por lo tanto, en la medida de lo posible, las pruebas de raíz unitaria de la variable $PIBpc$ se analizaran con intercepto y tendencia, mientras que las de la variable $C1PIBpc$ se analizaran sin intercepto y sin tendencia (o con solo intercepto sino fuera posible hacer la prueba sin intercepto)

A continuación se desarrollan las distintas pruebas de raíz unitaria para las variables $PIBpc$ y $C1PIBpc$.

****************************************************************************************************************************************************************************

### 1) Prueba $ADF$

En cuanto a la prueba $ADF$, el número óptimo de rezagos, para cada especificación, se va a escoger tomando en cuenta el _Criterio de Información de Akaike_ y el _Criterio Bayesiano de Schwartz_. Para ello, se copiaron los siguientes comandos:

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

Los resultados de las pruebas $ADF$ son:

| Variable   | Estadístico | Criterio de <br> información | Número óptimo <br> de rezagos | Valor       |  1%     |  5%     |  10%    | 
|------------|:-----------:|:----------------------------:|:-----------------------------:|:-----------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $\tau_\tau$ |  Akaike                      |   3                           | $-2.8429$   | $-4.04$ | $-3.45$ | $-3.15$ | 
| $PIBpc$    | $\phi_2$    |  Akaike                      |   3                           |  $4.3493$*  |  $6.50$ |  $4.88$ |  $4.16$ | 
| $PIBpc$    | $\phi_3$    |  Akaike                      |   3                           |  $6.0919$*  |  $8.73$ |  $6.49$ |  $5.47$ | 
| $PIBpc$    | $\tau_\tau$ |  Schwartz                    |   1                           | $-1.7651$   | $-4.04$ | $-3.45$ | $-3.15$ | 
| $PIBpc$    | $\phi_2$    |  Schwartz                    |   1                           |  $2.0235$   |  $6.50$ |  $4.88$ |  $4.16$ |
| $PIBpc$    | $\phi_3$    |  Schwartz                    |   1                           |  $2.9459$   |  $8.73$ |  $6.49$ |  $5.47$ |
| $C1PIBpc$  | $\tau$      |  Akaike y Schwartz           |   1                           | $-2.3286$** | $-2.60$ | $-1.95$ | $-1.61$ |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**

En consecuencia, la variable $PIBpc$ es no estacionaria porque:[^4]
* $\tau_\tau$ = -3.15 < **-2.8429 (Akaike)** < **-1.7651 (Schwartz)**, es decir, no se rechaza $\gamma=0$ al 10%, 5% y 1%
* $\phi_2$ = **2.0235 (Schwartz)** < 4.16 < **4.3493 (Akaike)** < 4.88, es decir, no se rechaza $\gamma=a_0=a_2=0$ al 10%, 5% y 1% si se utiliza el Criterio Schwartz (con el Criterio de Akaike sólo se rechazaría al 10%)
* $\phi_3$ =  **2.9459 (Schwartz)** < 5.47 <**6.0919 (Akaike)** < 6.49, es decir, no se rechaza $\gamma=a_2=0$ al 10%, 5% y 1% si se utiliza el Criterio Schwartz (con el Criterio de Akaike sólo se rechazaría al 10%)

[^4]: **La prueba no es totalmente contundente porque para algunos niveles de significancia, en ocasiones se rechaza la hipótesis nula. Sin embargo, al 5% de significancia es clara la existencia de raíz unitaria en esta variable**

Y la variable $C1PIBpc$ es estacionaria porque:[^5] 
* $\tau$ = -4.308< **-2.3286** <-2.60, es decir, no se rechaza $\gamma=0$ al 1% pero si se rechaza al 5% y 10%

[^5]: **Aunque al 1% no se rechaza la hipótesis de raiz unitaria, al 5% y al 10% si se rechaza**

Dado que con el _Criterio de Información de Akaike_ se concluye que la prueba $ADF$ de la variable $PIBpc$ se debe hacer con $3$ rezagos y que la prueba $ADF$ de la variable $C1PIBpc$ se debe hacer con $1$ rezago. Entonces, se van utilizar los siguientes comandos para evaluar la existencia de autocorrelación en los residuos de ambas pruebas (y por consiguiente, si los residuos son ruido blanco): 

``` r
PIBpc_urdfTest<- urdfTest(PIBpc, lags = 3, type = c("ct"), doplot = TRUE)
C1PIBpc_urdfTest<- urdfTest(C1PIBpc, lags = 1, type = c("nc"), doplot = TRUE)
```  
Obteniendose los siguientes gráficos,

1) Para los residuos de la prueba $ADF$ de $PIBpc$
   
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4a7e542c-98a2-42b1-ab94-458640b56352)


2) Para los residuos de la prueba $ADF$ de $C1PIBpc$
   
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/6311e9ff-3bc6-4a4e-9802-5ea2fb825685)


Los gráficos $FAC$ y $FACP$, que se encuentran arriba, revelan que los residuos de ambas pruebas $ADF$ no estan autocorrelacionados (estan dentro de las bandas de confianza)[^6]; por lo tanto, los residuos son ruido blanco y se concluye que ambas pruebas de raíz unitaria estan bien hechas.

[^6]: **Tenga en cuenta que la _FAC_ en el rezago cero siempre es igual a 1. Por lo tanto, la _FAC_ del rezago cero nunca se lee para interpretar la prueba de autocorrelación**

Por úlimo, se utilizó el comando 

``` r
ndiffs(PIBpc, alpha = 0.05, test = c("adf"), type = c("trend"))
```

Obteniendose, 

``` r
> ndiffs(PIBpc, alpha = 0.05, test = c("adf"), type = c("trend"))
[1] 1
```
Es decir, al 5% de significancia, la variable $PIBpc$ tiene raíz unitaria, y la variable $C1PIBpc$ es estacionaria

****************************************************************************************************************************************************************************

### Prueba DF-GLS

Dado que el $PIBpc$ es una variable que presenta tendencia, entonces se va a desarrollar su prueba $DF-GLS$ con tendencia. Dado que el $C1PIBpc$ es una variable que no presenta tendencia, entonces se va a desarrollar su prueba $DF-GLS$  sin tendencia. El número de rezagos se va a escoger utilizando el _Criterio Bayesiano de Schwartz_. Para el desarrollo de la prueba se copiaron los siguientes comandos:

``` r
PIBpc_DFGLSt.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("trend"),lag.max = 4)
C1PIBpc_DFGLSc.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("constant"),lag.max = 4) 

summary(PIBpc_DFGLSt.ers)
summary(C1PIBpc_DFGLSt.ers)
```

``` r
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

```
Los resultados de las pruebas $DF-GLS$ son:

| Variable   | Estadistico   | Valor     |  10%    |  5%     |   1%    |
|------------|---------------|:---------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $P_\tau$      | $34.0584$ | $6.79$  | $5.64$  | $4.26$  |
| $C1PIBpc$  | $P_\mu$       | $0.319$***| $4.17$  | $3.11$  | $1.95$  |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**

La variable $PIBpc$ es no estacionaria porque:
* Con el estadistico $P_\tau$: **34.0584** > 6.79, no se rechaza la hipótesis nula de raíz unitaria  10%, 5% y 1% 

La variable $C1PIBpc$ es estacionaria porque:
* Con el estadistico $P_\mu$: **0.319** < 1.95, se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%

****************************************************************************************************************************************************************************

### Prueba KPSS
Vamos a aplicar las opciones de rezago de $R$. Sin embargo, no olviden que según Newey y West (1994) la longitud de rezago se debe establecer proporcional a $T^{1/3}$, en decir $60^{1/3}=3.91\sim 4$. Por otro lado, dado que el $PIBpc$ es una variable que presenta tendencia, entonces se decide testear dicha prueba con tendencia; y como el el $C1PIBpc$ es una variable que no presenta tendencia, entonces se decide testear dicha prueba sólo con el intercepto. Los comandos para las dos pruebas son:

``` r
PIBpc_1t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("nil"), use.lag = NULL)
PIBpc_2t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("short"), use.lag = NULL)
PIBpc_3t.kpss <- ur.kpss(PIBpc, type = c("tau"), use.lag = 4)
PIBpc_4t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("long"), use.lag = NULL)

C1PIBpc_1m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("nil"), use.lag = NULL)
C1PIBpc_2m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("short"), use.lag = NULL)
C1PIBpc_3m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), use.lag = 4)
C1PIBpc_4m.kpss <- ur.kpss(C1PIBpc, type = c("mu"), lags = c("long"), use.lag = NULL)

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
####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 0 lags. 

Value of test-statistic is: 0.8236 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_2t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 3 lags. 

Value of test-statistic is: 0.2324 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_3t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 4 lags. 

Value of test-statistic is: 0.1955 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(PIBpc_4t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 10 lags. 

Value of test-statistic is: 0.1302 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(C1PIBpc_1m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 0 lags. 

Value of test-statistic is: 0.5859 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_2m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 3 lags. 

Value of test-statistic is: 0.2736 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_3m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 4 lags. 

Value of test-statistic is: 0.2562 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(C1PIBpc_4m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 10 lags. 

Value of test-statistic is: 0.2381 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739
```
Los resultados de las pruebas $KPSS$ son:

| Variable | Estadistico | Rezagos        | Valor         |  10%    |  5%     |  2.5%   |  1%     |
|----------|-------------|:--------------:|:-------------:|:-------:|:-------:|:-------:|:-------:|
| PIBpc    | $KPSS_\tau$ |  nil= $0$      |  $0.8236$**** | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  short= $3$    |  $0.2324$**** | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  NW(1994)= $4$ |  $0.1955$***  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| PIBpc    | $KPSS_\tau$ |  long= $10$    |  $0.1302$*    | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| C1PIBpc  | $KPSS_\mu$  |  nil= $0$      |  $0.5859$***  | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  short= $3$    |  $0.2736$     | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  NW(1994)= $4$ |  $0.2562$     | $0.347$ | $0.463$ | $0.574$ | $0.739$ |
| C1PIBpc  | $KPSS_\mu$  |  long= $10$    |  $0.2381$     | $0.347$ | $0.463$ | $0.574$ | $0.739$ |

**Niveles de significancia: **** al 1%, *** al 2.5%, ** al 5% y * al 10%**

La variable $PIBpc$ es no estacionaria porque:
* Cuando el número de rezagos es el establecido por la formula de Newey y West (1994)= $4$ se obtiene: 0.176 < **0.1955** <0.216, se rechaza la hipótesis nula de estacionariedad al 10%, 5% y 2.5% y 5%, pero no se rechaza al 1%

La variable $C1PIBpc$ es estacionaria porque:
* Cuando el número de rezagos es el establecido por la formula de Newey y West (1994)= $4$ se obtiene: **0.2562**<0.347, no se rechaza la hipótesis nula de estacionariedad al 1%, 2.5%, 5% y 10%.

Por último, se utilizó el comando 

``` r
ndiffs(PIBpc, alpha = 0.05, test = c("kpss"), type = c("trend"))
```

Obteniendose, 

``` r
> ndiffs(PIBpc, alpha = 0.05, test = c("kpss"), type = c("trend"))
[1] 1
```
Es decir, al 5% de significancia, la variable $PIBpc$ tiene raíz unitaria, y la variable $C1PIBpc$ es estacionaria

****************************************************************************************************************************************************************************

### Prueba ZA

Las pruebas previas parecen indicar que la variable $PIBpc$ es no estacionaria y la variable $C1PIBpc$ es estacionaria. Sin embargo, los dos gráficos iniciales de esta sección parecen indicar un cambio estructural, alrededor de 1998-1999 (cuando Colombia experimentó una crisis económica), que afecta a $PIBpc$ pero no a $C1PIBpc$. Por consiguiente, se va a hacer la prueba de Zivot y Andrews para analizar la presencia de raíz unitaria en la variable $PIBpc$. Para ello, se copian los siguientes comandos

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

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-779772  -99685    6135  124243  358601 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 7.850e+05  2.099e+05   3.740  0.00044 ***
y.l1        8.744e-01  4.163e-02  21.004  < 2e-16 ***
trend       1.933e+04  7.156e+03   2.701  0.00918 ** 
du          4.745e+05  1.139e+05   4.166  0.00011 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 199600 on 55 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9969,	Adjusted R-squared:  0.9967 
F-statistic:  5887 on 3 and 55 DF,  p-value: < 2.2e-16


Teststatistic: -3.0165 
Critical values: 0.01= -5.34 0.05= -4.8 0.1= -4.58 

Potential break point at position: 46 

> summary(ZA2.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-770090 -119124   -6756  129557  429068 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.076e+06  3.165e+05   3.398  0.00127 ** 
y.l1        8.160e-01  6.084e-02  13.412  < 2e-16 ***
trend       2.862e+04  9.421e+03   3.038  0.00364 ** 
dt          4.613e+04  1.407e+04   3.279  0.00181 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 209300 on 55 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9966,	Adjusted R-squared:  0.9964 
F-statistic:  5348 on 3 and 55 DF,  p-value: < 2.2e-16


Teststatistic: -3.024 
Critical values: 0.01= -4.93 0.05= -4.42 0.1= -4.11 

Potential break point at position: 42 

> summary(ZA3.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-333653 -116006    1991  118176  386430 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.788e+06  3.403e+05   5.254 2.59e-06 ***
y.l1         6.552e-01  7.031e-02   9.319 7.81e-13 ***
trend        5.986e+04  1.240e+04   4.827 1.18e-05 ***
du          -6.453e+05  1.620e+05  -3.984 0.000205 ***
dt           8.533e+04  1.643e+04   5.193 3.23e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 188400 on 54 degrees of freedom
  (1 observation deleted due to missingness)
Multiple R-squared:  0.9973,	Adjusted R-squared:  0.9971 
F-statistic:  4958 on 4 and 54 DF,  p-value: < 2.2e-16


Teststatistic: -4.9037 
Critical values: 0.01= -5.57 0.05= -5.08 0.1= -4.82 

Potential break point at position: 39 
```

Los resultados de las pruebas de Zivot-Andrews son:

| Estadistico      | Año del cambio <br> estructural |¿Es estadisticamente significativo <br> el cambio estructural? | Valor    |  10%    |  5%     |  1%     |
|------------------|:-------------------------------:|:-------------------------------------------------------------:|:--------:|:-------:|:-------:|:-------:|
| $ZA_{Intercept}$ |  $2006$ = 1960+46               | Si (du al 1%)                                                 |$-3.6435$ | $-4.58$ | $-4.80$ | $-5.34$ |
| $ZA_{Trend}$     |  $2004$ = 1960+44               | Si (dt al 1%)                                                 |$-3.024$  | $-4.11$ | $-4.42$ | $-4.93$ |
| $ZA_{Both}$      |  $1999$ = 1960+39               | Si (du al 0.1% y dt al 0.1%)                                  |$-4.9037$*| $-4.82$ | $-5.08$ | $-5.57$ |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**

De los tres estadisticos, el que cogio bien el año de la crisis económica de los finales de los 1990s fue $ZA_{Both}$. Por lo tanto, es el estadístico que vamos a analizar:
* Con el estadistico $ZA_{Both}$: -5.08 < **-4.9037** < -4.82, no se rechaza la hipótesis nula de raíz unitaria 1% y 5%, y se rechaza al 10%. Por consiguiente, se concluye que hay raíz unitaria

Copiando los siguientes comandos se puede visualizar gráficamente el comportamiento de los estadisticos, de acuerdo a la especificación escogida del modelo:
``` r
urzaTest(PIBpc, model = c("both"))
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/1ba0c5be-0e21-43b6-aee6-038d594c5eb7)

Es decir, hay un cambio estructural en pendiente y tendencia en el año 1999.

#### Conclusión:
Todas las pruebas aplicadas parecen indicar la presencia de raíz unitaria en la variable $PIBpc$ y la estacionariedad en la variable $C1PIBpc$

| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) |
|---------------------------------------------------------|--------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>

