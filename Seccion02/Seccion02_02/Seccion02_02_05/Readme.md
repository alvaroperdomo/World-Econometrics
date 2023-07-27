<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.2.5 
# Caso de estudio utilizando la base de datos _Indicadores de Desarrollo Mundial_ (primera parte)

Para el análisis univariado, se ha escogido estudiar la variable más utilizada en los análisis de desarrollo económico: el PIB per cápita a precios constantes de un país en vías de desarrollo. Más específicamente, se va a analizar la evolución del PIB per cápita de Colombia a pesos constantes. En particular, utilizaremos el **PIB per cápita de Colombia a pesos constantes durante el periodo 1960-2019**  para calcular las proyecciones de esta variable durante el periodo 2020-2025 como si no se hubiera presentado la pandemia del Covid 19. Con ello, lo que se busca es hacer un ejercicio contrafactual para analizar cómo se hubiera comportado esta variable, según su comportamiento histórico, si no hubieran ocurrido los eventos que se dieron desde el año 2020, entre los que se destaca la cuarentena que se dio debido a la pandemia del Covid-19.

En esta subsección se van a hacer pruebas de raíz unitaria tanto a la variable **PIB per cápita de Colombia a pesos constantes en niveles** como a la **primera diferencia del PIB per cápita de Colombia a pesos constantes** porque, tal como se mostrara más adelante, esta variable no es estacionaria en niveles, pero si lo va a ser en las primeras diferencias. Posteriormente, en la sección 2.3.3. se desarrolla el resto del análisis univariado.

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

### Prueba $ADF-GLS$

Dado que el $PIBpc$ es una variable que presenta tendencia, entonces se va a desarrollar su prueba $ADF-GLS$ con tendencia. Dado que el $C1PIBpc$ es una variable que no presenta tendencia, entonces se va a desarrollar su prueba $ADF-GLS$ sin tendencia. El número de rezagos se va a escoger de forma ascendente, tal que los residuos estimados no estés correlacionados. Para el desarrollo de la prueba se copiaron los siguientes comandos:

``` r
# Residuos PIBpc con 1 rezago
urersTest(PIBpc, type = c("DF-GLS"), model = c("trend"), lag.max = 1, doplot = TRUE)
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/1b8a005d-094a-4cf1-9ada-efe164a8f043)

Observe que los residuos no estan correlacionados, entonces la prueba para PIBpc se hará con 1 rezago.

``` r
* Residuos C1PIBpc con 1 rezago
urersTest(C1PIBpc, type = c("DF-GLS"), model = c("constant"), lag.max = 1, doplot = TRUE)
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/da4c7100-2d26-42e9-8073-551ca1a83b95)

Similarmente, observe que los residuos no estan correlacionados, entonces la prueba para C1PIBpc se hará con 1 rezago.

Por lo tanto, digitamos los comandos:

``` r
PIBpc_DFGLSt.ers <- ur.ers(PIBpc, type = c("DF-GLS"), model = c("trend"),lag.max = 1)
C1PIBpc_DFGLSc.ers <- ur.ers(C1PIBpc, type = c("DF-GLS"), model = c("constant"),lag.max = 1) 

summary(PIBpc_DFGLSt.ers)
summary(C1PIBpc_DFGLSc.ers)
```
y obtenemos

``` r
> summary(PIBpc_DFGLSt.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept and trend 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
    Min      1Q  Median      3Q     Max 
-709972  -94205    3638  100824  467202 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
yd.lag       -0.05364    0.03187  -1.683   0.0979 .  
yd.diff.lag1  0.57317    0.11234   5.102 4.15e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 195000 on 56 degrees of freedom
Multiple R-squared:  0.3209,	Adjusted R-squared:  0.2967 
F-statistic: 13.23 on 2 and 56 DF,  p-value: 1.967e-05


Value of test-statistic is: -1.683 

Critical values of DF-GLS are:
                 1pct  5pct 10pct
critical values -3.58 -3.03 -2.74

> summary(C1PIBpc_DFGLSc.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
    Min      1Q  Median      3Q     Max 
-659831  -84615   42646  137049  498871 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
yd.lag       -0.448590   0.127215  -3.526 0.000858 ***
yd.diff.lag1  0.008424   0.135096   0.062 0.950507    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 203100 on 55 degrees of freedom
Multiple R-squared:  0.2222,	Adjusted R-squared:  0.1939 
F-statistic: 7.856 on 2 and 55 DF,  p-value: 0.0009975


Value of test-statistic is: -3.5262 

Critical values of DF-GLS are:
                1pct  5pct 10pct
critical values -2.6 -1.95 -1.62
```

Los resultados de las pruebas $DF-GLS$ son:

| Variable   | Estadistico   | Rezagos   | Valor       |  10%      |  5%      |   1%    |
|------------|:-------------:|:---------:|:-----------:|:---------:|:--------:|:-------:|
| $PIBpc$    | $DF-GLS_\tau$ | 1         | $-1.683$    | $-2.74$   | $-3.03$  | $-3.58$ |
| $C1PIBpc$  | $DF-GLS_\mu$  | 1         | $-3.5262$***| $-1.62$   | $-1.95$  | $-2.60$ |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**    

La variable $PIBpc$ es no estacionaria porque:
* Con el estadistico $P_\tau$: **34.0584** > 6.79, no se rechaza la hipótesis nula de raíz unitaria  10%, 5% y 1% 

La variable $C1PIBpc$ es estacionaria porque:
* Con el estadistico $P_\mu$: **-3.5262** < -2.6, se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%

Por otra parte, al calcular las pruebas $P$ con el comando:

``` r
PIBpc_Pt.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("trend"),lag.max = 1)
C1PIBpc_Pc.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("constant"),lag.max = 1) 

summary(PIBpc_Pt.ers)
summary(C1PIBpc_Pc.ers)
``` 

Se obtiene, 

``` r
> summary(PIBpc_Pt.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept and trend 

Value of test-statistic is: 15.1326 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 4.26 5.64  6.79

> summary(C1PIBpc_Pc.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept 

Value of test-statistic is: 1.1421 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 1.95 3.11  4.17

```

| Variable   | Estadistico   | Rezagos   | Valor      |  10%    |  5%     |   1%    |
|------------|:-------------:|:---------:|:----------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $P_\tau$      | 1         | $15.1326$  | $6.79$  | $5.64$  | $4.26$  |
| $C1PIBpc$  | $P_\mu$       | 1         | $1.1421$***| $4.17$  | $3.11$  | $1.95$  |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**

La variable $PIBpc$ es no estacionaria porque:
* Con el estadistico $P_\tau$: **15.1326** > 6.79, no se rechaza la hipótesis nula de raíz unitaria  10%, 5% y 1% 

La variable $C1PIBpc$ es estacionaria porque:
* Con el estadistico $P_\mu$: **1.1421** < 1.95, se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%

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

Las pruebas previas parecen indicar que la variable $PIBpc$ es no estacionaria y la variable $C1PIBpc$ es estacionaria. Sin embargo, los dos gráficos iniciales de esta sección parecen indicar un cambio estructural, alrededor de 1998-1999 (cuando Colombia experimentó una crisis económica), que afecta a $PIBpc$ pero no a $C1PIBpc$. Por consiguiente, se va a hacer la prueba de Zivot y Andrews para analizar la presencia de raíz unitaria en la variable $PIBpc$. Para ello, se copian los siguientes comandos [^7]

[^7]: **Para no hacer engorroso el análisis de la prueba de Zivot y Andrews, note que en el código de _R_ sólo se encuentra habilitado el gráfico del cambio estructural de la prueba de Zivot y Andrews en la que ocurre el cambio estructural tanto en intercepto como en tendencia y donde sólo se tiene un rezago. Esto se hace así por dos razones: (1) porque los rezagos superiores a 1 no eran estadisticamente significativos, y (2) la prueba de Zivot y Andrews con cambio estructural tanto en intercepto como en tendencia ubicó la fecha del cambio estructural a finales de los años noventas, conincidiendo con la época de la mayor crisis económica que ha enfrentado la economía colombiana en el perido análizado, mientras que las otras especificaciones de la prueba de Zivot y Andrews no ubican el cambio estructural en fechas tan relevantes.** 

``` r
ZA1.za <- ur.za(PIBpc, model = c("intercept"), lag=1)
summary(ZA1.za)
#urzaTest(PIBpc, model = c("intercept"), lag=1, doplot=TRUE)

ZA2.za <- ur.za(PIBpc, model = c("trend"), lag=1)
summary(ZA2.za)
#urzaTest(PIBpc, model = c("trend"), lag=1, doplot=TRUE)

ZA3.za <- ur.za(PIBpc, model = c("both"), lag=1)
summary(ZA3.za)
urzaTest(PIBpc, model = c("both"), lag=1, doplot=TRUE)
```
Obteniendo
``` r
> ZA1.za <- ur.za(PIBpc, model = c("intercept"), lag=1)
> summary(ZA1.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-636203  -67652   -2543  100247  369950 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.114e+06  2.255e+05   4.942 8.15e-06 ***
y.l1        7.847e-01  4.571e-02  17.166  < 2e-16 ***
trend       3.523e+04  7.626e+03   4.620 2.49e-05 ***
y.dl1       5.682e-01  1.019e-01   5.578 8.43e-07 ***
du          5.612e+05  1.240e+05   4.525 3.44e-05 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 168400 on 53 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.9978,	Adjusted R-squared:  0.9976 
F-statistic:  5983 on 4 and 53 DF,  p-value: < 2.2e-16


Teststatistic: -4.7099 
Critical values: 0.01= -5.34 0.05= -4.8 0.1= -4.58 

Potential break point at position: 50 

> #urzaTest(PIBpc, model = c("intercept"), lag=1)
> 
> ZA2.za <- ur.za(PIBpc, model = c("trend"), lag=1)
> summary(ZA2.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-598386  -83698  -16324  122255  376584 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) 1.293e+06  3.101e+05   4.169 0.000114 ***
y.l1        7.545e-01  6.050e-02  12.472  < 2e-16 ***
trend       3.846e+04  9.355e+03   4.111 0.000138 ***
y.dl1       5.572e-01  1.072e-01   5.200 3.27e-06 ***
dt          5.588e+04  1.533e+04   3.644 0.000611 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 177300 on 53 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.9976,	Adjusted R-squared:  0.9974 
F-statistic:  5396 on 4 and 53 DF,  p-value: < 2.2e-16


Teststatistic: -4.0572 
Critical values: 0.01= -4.93 0.05= -4.42 0.1= -4.11 

Potential break point at position: 44 

> #urzaTest(PIBpc, model = c("trend"), lag=1)
> 
> ZA3.za <- ur.za(PIBpc, model = c("both"), lag=1)
> summary(ZA3.za)

################################ 
# Zivot-Andrews Unit Root Test # 
################################ 


Call:
lm(formula = testmat)

Residuals:
    Min      1Q  Median      3Q     Max 
-357621 -112451  -16664   83166  400760 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept)  1.386e+06  2.471e+05   5.610 7.88e-07 ***
y.l1         7.231e-01  5.059e-02  14.295  < 2e-16 ***
trend        4.853e+04  9.092e+03   5.338 2.09e-06 ***
y.dl1        4.625e-01  9.883e-02   4.680 2.09e-05 ***
du          -5.063e+05  1.215e+05  -4.166 0.000117 ***
dt           5.899e+04  1.128e+04   5.229 3.07e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 161800 on 52 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared:  0.998,	Adjusted R-squared:  0.9978 
F-statistic:  5183 on 5 and 52 DF,  p-value: < 2.2e-16


Teststatistic: -5.4732 
Critical values: 0.01= -5.57 0.05= -5.08 0.1= -4.82 

Potential break point at position: 38 

> urzaTest(PIBpc, model = c("both"), lag=1)

```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/91c7efba-f755-4274-aedb-283908b249db)

Los resultados de las pruebas de Zivot-Andrews son:

| Estadistico      | Año del cambio <br> estructural |¿Es estadisticamente significativo <br> el cambio estructural? | Valor     |  10%    |  5%     |  1%     |
|------------------|:-------------------------------:|:-------------------------------------------------------------:|:---------:|:-------:|:-------:|:-------:|
| $ZA_{Intercept}$ |  $2009$ = 1959+50               | Si (du al 1%)                                                 |$-4.7099$* | $-4.58$ | $-4.80$ | $-5.34$ |
| $ZA_{Trend}$     |  $2003$ = 1959+44               | Si (dt al 0.1%)                                               |$-4.0572$  | $-4.11$ | $-4.42$ | $-4.93$ |
| $ZA_{Both}$      |  $1997$ = 1959+38               | Si (du al 0.1% y dt al 0.1%)                                  |$-5.4732$**| $-4.82$ | $-5.08$ | $-5.57$ |

**Niveles de significancia: *** al 1%, ** al 5% y * al 10%**

De los tres estadisticos, y como se muestra en el gráfico de arriba, el que acertó bien el año de la crisis económica de los finales de los 1990s fue $ZA_{Both}$. Por lo tanto, es el estadístico que vamos a analizar:
* Con el estadistico $ZA_{Both}$: -5.57 < **-5.4732** < -2.08, no se rechaza la hipótesis nula de raíz unitaria 1% y se rechaza al 5% y 10%. Por consiguiente, no se puede rechazar de forma contundente que la serie $PIBpc$ tiene raíz unitaria


#### Conclusión:
Todas las pruebas aplicadas parecen indicar la presencia de raíz unitaria en la variable $PIBpc$ y la estacionariedad en la variable $C1PIBpc$

---
---
# Preguntas de selección múltiple

Para esta sección, vamos a hacer preguntas con respecto a las pruebas de raíz unitaria de la variable NE.CON.GOVT.ZS [Gasto de consumo final del gobierno general (% del PIB)](https://datos.bancomundial.org/indicator/NE.CON.GOVT.ZS), a la cual llamaremos (GGOV) cuando la manejemos en niveles y (C1GGOV) cuando la manejemos en su primera diferencia.

El código que se utilizó para descargar y preparar los datos para las pruebas es:

``` r
# Pruebas UNIT ROOT GGOV

rm(list = ls())

library(WDI)         # Esta libreria sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta libreria permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggfortify)   # Esta libreria tienen comando utiles para plantear gráficos de series de tiempo, por ejemplo utilizando el comando autoplot()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(fUnitRoots)  # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(urca)        # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(forecast)    # Esta libreria sirve para hacer pronósticos.

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')

dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS"), country=c('CL'), language = "es")

dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, GGOV_lag1 = lag(GGOV, order_by = year), C1GGOV=GGOV-GGOV_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

ggplot(dat, aes(year, GGOV)) + scale_x_continuous(name="Años", limits=c(1960, 2022)) + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1960-2019", y="Porcentaje", title="Gasto real del Gobierno (%PIB)", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

ggplot(dat, aes(year, C1GGOV)) + scale_x_continuous(name="Años", limits=c(1961, 2022)) + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1960-2019", y="Porcentaje", title="Cambio en el gasto real del Gobierno (%PIB)", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

GGOV__ <- dat
C1GGOV__ <- GGOV__[-c(1),]
GGOV_ = subset(GGOV__, select = c(GGOV))
C1GGOV_ = subset(C1GGOV__, select = c(C1GGOV))

GGOV <- ts(GGOV_, start=1960, end=2022)
C1GGOV <- ts(C1GGOV_, start=1961, end=2022)
```

1. **Para confirmar que estemos manejando los mismos datos, responda ¿Entre 2015 y 2022, en cuál año se dio el mayor valor de la variable _GGOV_ y en qué valor (a dos dígitos)?:**
 
   a) 2022: 14.45.

   b) 2020: 16.01.

   c) 2018: 20.02.

   d) 2015: 15.24.

2. **¿Gráfique las $FAC$ de $GGOV$ y $C1GGOV$ y responda cuál de las siguientes afirmaciones es errónea?:**

   a) El decrecimiento moderado de la $FAC$ de $GGOV$ da una idea de raíz unitaria en esta variable.

   b) El comportamiento de la $FAC$ de $C1GGOV$ da una idea de estacionariedad en esta variable.

   c) El decrecimiento moderado de la $FAC$ de $GGOV$ da una idea no estacionariedad en esta variable.

   d) El comportamiento de la $FAC$ de $C1GGOV$ da una idea de integración de orden $0$ en esta variable.

``` r
autoplot(acf(GGOV, plot = FALSE))
autoplot(acf(C1GGOV, plot = FALSE))  
```

3. **Las pruebas $ADF$ para las variables $GGOV$ y $C1GGOV$ se van a hacer con intercepto y tendencia para la primera, y sin intercepto ni tendencia para la segunda. Responda ¿cuál de las siguientes afirmaciones es correcta según los resultados de las pruebas $ADF$?:**

   **Primera afirmación: Para la variable $GGOV$, sin importar si se utiliza el Criterio de Información de Akaike o el Criterio Bayesiano de Schwartz, al 10% de significancia no se rechaza la hipótesis nula de raíz unitaria***

   **Segunda afirmación: Para la variable $C1GGOV$, sin importar si se utiliza el Criterio de Información de Akaike o el Criterio Bayesiano de Schwartz, al 1% de significancia se rechaza la hipótesis nula de raíz unitaria***

   a) La primera y la segunda afirmación son verdaderas.

   b) La primera y la segunda afirmación son falsas.

   c) La primera afirmación es verdadera y la segunda afirmación es falsa.

   d) La primera afirmación es falsa y la segunda afirmación es verdadera.

``` r
GGOV_ur_trend_AIC.df <- ur.df(y=GGOV, type = c("trend"), lags = 10, selectlags = c("AIC"))
GGOV_ur_trend_BIC.df <- ur.df(y=GGOV, type = c("trend"), lags = 10, selectlags = c("BIC"))
C1GGOV_ur_none_AIC.df <- ur.df(C1GGOV, type = c("none"), lags = 10, selectlags = c("AIC"))
C1GGOV_ur_none_BIC.df <- ur.df(C1GGOV, type = c("none"), lags = 10, selectlags = c("BIC"))

summary(GGOV_ur_trend_AIC.df)
summary(GGOV_ur_trend_BIC.df)
summary(C1GGOV_ur_none_AIC.df)
summary(C1GGOV_ur_none_BIC.df)

GGOV_urdfTest<- urdfTest(GGOV, lags = 1, type = c("ct"), doplot = TRUE)
C1GGOV_urdfTest<- urdfTest(C1GGOV, lags = 1, type = c("nc"), doplot = TRUE)

ndiffs(GGOV, alpha = 0.05, test = c("adf"), type = c("trend"))
```

4. **Las pruebas $ADF-GLS$ para las variables $GGOV$ y $C1GGOV$ se van a hacer con intercepto y tendencia para la primera, y con intercepto para la segunda. Responda ¿cuál de las siguientes afirmaciones es correcta según los resultados de las pruebas $ADF-GLS$ a un rezago?:**

   **Primera afirmación: Para la variable $GGOV$, sin importar si se utiliza la opción type("DF-GLS) o la opción type("P-test"), al 10% de significancia no se rechaza la hipótesis nula de raíz unitaria***

   **Segunda afirmación: Para la variable $C1GGOV$, sin importar si se utiliza la opción type("DF-GLS) o la opción type("P-test"), al 1% de significancia se rechaza la hipótesis nula de raíz unitaria***


   a) La primera y la segunda afirmación son verdaderas.

   b) La primera y la segunda afirmación son falsas.

   c) La primera afirmación es verdadera y la segunda afirmación es falsa.

   d) La primera afirmación es falsa y la segunda afirmación es verdadera.

``` r
urersTest(GGOV, type = c("DF-GLS"), model = c("trend"), lag.max = 1, doplot = TRUE)
urersTest(C1GGOV, type = c("DF-GLS"), model = c("constant"), lag.max = 1, doplot = TRUE)

# Como los gráficos de las dos variables no mostraron ningun problema de autocorrelación con los residuos a un rezago, entonces se decidió hacer las pruebas ADF-GLS a un rezago

GGOV_DFGLSt.ers <- ur.ers(GGOV, type = c("DF-GLS"), model = c("trend"),lag.max = 1)
C1GGOV_DFGLSc.ers <- ur.ers(C1GGOV, type = c("DF-GLS"), model = c("constant"),lag.max = 1)

summary(GGOV_DFGLSt.ers)
summary(C1GGOV_DFGLSc.ers)

GGOV_Pt.ers <- ur.ers(GGOV, type = c("P-test"), model = c("trend"),lag.max = 1)
C1GGOV_Pc.ers <- ur.ers(C1GGOV, type = c("P-test"), model = c("constant"),lag.max = 1)

summary(GGOV_Pt.ers)
summary(C1GGOV_Pc.ers)
```

5. **Las pruebas $KPSS$ para las variables $GGOV$ y $C1GGOV$ se van a hacer con intercepto y tendencia para la primera, y con intercepto para la segunda. Responda ¿cuál de las siguientes afirmaciones es correcta según los resultados de las pruebas $KPSS$ con $4$ rezagos [^8]?:**

[^8]: **Recuerden que este es el número óptimo de rezagos para una serie de 63 o 62 datos según el criterio de  Newey y West (1994) que se comentó en la sección 2.2.3.(T).**

   **Primera afirmación: Para la variable $GGOV$ al 1% de significancia se rechaza la hipótesis nula de estacionariedad***

   **Segunda afirmación: Para la variable $C1GGOV$ al 10% de significancia no se rechaza la hipótesis nula de estacionariedad***

   a) La primera y la segunda afirmación son verdaderas.

   b) La primera y la segunda afirmación son falsas.

   c) La primera afirmación es verdadera y la segunda afirmación es falsa.

   d) La primera afirmación es falsa y la segunda afirmación es verdadera.

``` r
GGOV_1t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("nil"), use.lag = NULL)
GGOV_2t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("short"), use.lag = NULL)
GGOV_3t.kpss <- ur.kpss(GGOV, type = c("tau"), use.lag = 4)
GGOV_4t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("long"), use.lag = NULL)

C1GGOV_1m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("nil"), use.lag = NULL)
C1GGOV_2m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("short"), use.lag = NULL)
C1GGOV_3m.kpss <- ur.kpss(C1GGOV, type = c("mu"), use.lag = 4)
C1GGOV_4m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("long"), use.lag = NULL)

summary(GGOV_1t.kpss)
summary(GGOV_2t.kpss)
summary(GGOV_3t.kpss)
summary(GGOV_4t.kpss)
summary(C1GGOV_1m.kpss)
summary(C1GGOV_2m.kpss)
summary(C1GGOV_3m.kpss)
summary(C1GGOV_4m.kpss)

ndiffs(GGOV, alpha = 0.05, test = c("kpss"), type = c("trend"))
```

6. **Para esta pregunta, asuma que tiene razones económicas para considerar que la prueba $ZA$ apropiada para la variable $GGOV$  tiene 4 rezagos y cambio estructural en el intercepto pero no en la tendencia. Haga la prueba $ZA$ con esa especificación y responda ¿cuál de las siguientes afirmaciones es correcta según los resultados de la prueba $ZA$ ?:**[^9]

[^9]: Para determinar bien el año del cambio estructural compare la fila de la base de datos "dat" con la columna "year"

a) Para la variable $GGOV$, si se asume un cambio estructural en el intercepto, este ocurre en el año 1984 y la serie presenta raíz unitaria
   
b) Para la variable $GGOV$, si se asume un cambio estructural en el intercepto, este ocurre en el año 1984 y la serie es estacionaria

c) Para la variable $GGOV$, si se asume un cambio estructural en el intercepto, este ocurre en el año 1992 y la serie presenta raíz unitaria

d) Para la variable $GGOV$, si se asume un cambio estructural en el intercepto, este ocurre en el año 1992 y la serie es estacionaria

``` r
ZA1.za <- ur.za(GGOV, model = c("intercept"), lag=4)
summary(ZA1.za)
urzaTest(GGOV, model = c("intercept"))

```

---
---


| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) |
|---------------------------------------------------------|--------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>

