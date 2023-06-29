# Pruebas de Raíz Unitaria

Para esta sección hay que instalar los paquetes
* [fUnitRoots](https://cran.r-project.org/web/packages/fUnitRoots/index.html)
* [urca](https://cran.r-project.org/web/packages/urca/index.html)

``` r
install.packages('fUnitRoots')
install.packages('urca')
```

| Pruebas                                                                                 | Explicación general de la prueba   |  Aplicación en R |
|-----------------------------------------------------------------------------------------|:----------------------------------:|:----------------:|
| Las Pruebas DF y ADF: _Pruebas de Dickey Fuller y Aumentada de Dickey Fuller_           |  [X](Seccion02_02_ADF_T/README.md) |             X    | 
| La Prueba ADF-GLS: _Prueba de Elliott, Rothenberg y Stock_                              |  X                                 |             X    |
| La Prueba KPSS: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_                       |  X                                 |             X    |   
| Pruebas de Cambio Estructural - P: _Prueba de Perron y ZA: _Prueba de Zivot y Andrews_  |  X                                 |             X    |



## Ejemplo utilizando el PIB per cápita de Colombia a precios constantes en pesos

La prueba se va a hacer con respecto al **PIB per cápita de Colombia** en niveles y en primeras diferencias utilizando el siguiente código:

Retomamos parte del código de R que se había utilizado previamente 
``` r
rm(list = ls())
library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)


dat = WDI(indicator= c(PIB_per_capita = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
ggplot(dat, aes(year, PIB_per_capita)) + labs(subtitle="$", y="Pesos constantes", x="Años", title="PIB per cápita real de Colombia", caption = "Fuente: 
Construcción propia a partir de los Indicadores de Desarrollo Económico del Banco Mundial")
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/681c14d8-e78a-48b9-b6a9-9bc12aea2e84)

A partir del gráfico se puede comenzar a inferir que la variable no tiene un comportamiento estacionario. Sin embargo, hay que recolectar más evidencia al respecto, para ello primero se visualiza la función de autocorrelación de las variables $PIBpc$ y $C1PIBpc$ utilizando el siguiente comando: 

Para el gráfico de la $FAC$ se ejecuta el comando
``` r
dat_ <- dat %>% arrange(year)
dat <- na.omit(dat_)
PIBpc_ = subset(dat, select = c(PIB_per_capita))
PIBpc <- ts(PIBpc_, start=1960)
C1PIBpc <- diff(PIBpc, differences = 1)

autoplot(acf(PIBpc, plot = FALSE))
autoplot(acf(C1PIBpc, plot = FALSE))
```
Obteniendose

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/ef5fe48f-b32e-448d-854f-876589b76e9e)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/6183adb4-ec1e-451e-82e1-0077ae8ee28c)

El decaimiento continuo pero moderado de la $FAC$ de la variable $PIBpc$ da una idea de raíz unitaria. Por otro lado, la variable $C1PIBpc$ tiene un $FAC$ bajo desde el comienzo, dando así una idea de estacionariedad. 

A continuación se desarrollan pruebas de raíz unitaria. Primero, para la variable $PIBpc$ y luego para la variable $C1PIBpc$:

### Prueba ADF
``` r
PIBpc_ur_trend.df <- ur.df(y=PIBpc, type = c("trend"), lags = 10, selectlags = c("AIC"))
PIBpc_ur_drift.df <- ur.df(PIBpc, type = c("drift"), lags = 10, selectlags = c("AIC"))
PIBpc_ur_none.df <- ur.df(PIBpc, type = c("none"), lags = 10, selectlags = c("AIC"))
C1PIBpc_ur_trend.df <- ur.df(y=C1PIBpc, type = c("trend"), lags = 10, selectlags = c("AIC"))
C1PIBpc_ur_drift.df <- ur.df(C1PIBpc, type = c("drift"), lags = 10, selectlags = c("AIC"))
C1PIBpc_ur_none.df <- ur.df(C1PIBpc, type = c("none"), lags = 10, selectlags = c("AIC"))
summary(PIBpc_ur_trend.df)
summary(PIBpc_ur_drift.df)
summary(PIBpc_ur_none.df)
summary(C1PIBpc_ur_trend.df)
summary(C1PIBpc_ur_drift.df)
summary(C1PIBpc_ur_none.df)
```
Obteniendose

``` r
> summary(PIBpc_ur_trend.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1456590  -140786    16594   176377   898299 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)  
(Intercept)  6.721e+05  2.968e+05   2.265   0.0283 *
z.lag.1     -1.416e-01  6.714e-02  -2.109   0.0404 *
tt           2.959e+04  1.394e+04   2.123   0.0392 *
z.diff.lag1 -1.927e-01  1.679e-01  -1.148   0.2570  
z.diff.lag2  5.828e-01  2.527e-01   2.307   0.0256 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 372600 on 46 degrees of freedom
Multiple R-squared:  0.155,	Adjusted R-squared:  0.08154 
F-statistic:  2.11 on 4 and 46 DF,  p-value: 0.09484


Value of test-statistic is: -2.1093 3.2214 2.2795 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -4.04 -3.45 -3.15
phi2  6.50  4.88  4.16
phi3  8.73  6.49  5.47

> summary(PIBpc_ur_drift.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression drift 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1607110  -137422     -202   180687   938891 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)  
(Intercept)  2.057e+05  2.068e+05   0.995   0.3249  
z.lag.1     -4.082e-03  1.821e-02  -0.224   0.8236  
z.diff.lag1 -2.266e-01  1.732e-01  -1.308   0.1973  
z.diff.lag2  4.451e-01  2.531e-01   1.758   0.0852 .
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 386200 on 47 degrees of freedom
Multiple R-squared:  0.07226,	Adjusted R-squared:  0.01304 
F-statistic:  1.22 on 3 and 47 DF,  p-value: 0.3128


Value of test-statistic is: -0.2241 2.4006 

Critical values for test statistics: 
      1pct  5pct 10pct
tau2 -3.51 -2.89 -2.58
phi1  6.70  4.71  3.86

> summary(PIBpc_ur_none.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1700811  -129976    31119   202167   914060 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
z.lag.1      0.012818   0.006565   1.953   0.0567 .
z.diff.lag1 -0.202519   0.171513  -1.181   0.2435  
z.diff.lag2  0.419725   0.251809   1.667   0.1021  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 386200 on 48 degrees of freedom
Multiple R-squared:  0.2787,	Adjusted R-squared:  0.2336 
F-statistic: 6.183 on 3 and 48 DF,  p-value: 0.001222


Value of test-statistic is: 1.9526 

Critical values for test statistics: 
     1pct  5pct 10pct
tau1 -2.6 -1.95 -1.61

> summary(C1PIBpc_ur_trend.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1668276  -153839     6537   182618   902756 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)   
(Intercept)  1.171e+05  1.503e+05   0.779  0.44006   
z.lag.1     -8.103e-01  2.433e-01  -3.330  0.00172 **
tt           1.360e+03  3.938e+03   0.345  0.73141   
z.diff.lag  -4.070e-01  2.502e-01  -1.626  0.11072   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 390100 on 46 degrees of freedom
Multiple R-squared:  0.5222,	Adjusted R-squared:  0.491 
F-statistic: 16.76 on 3 and 46 DF,  p-value: 1.697e-07


Value of test-statistic is: -3.33 3.7229 5.5705 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -4.04 -3.45 -3.15
phi2  6.50  4.88  4.16
phi3  8.73  6.49  5.47

> summary(C1PIBpc_ur_drift.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression drift 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1633724  -152346     4888   175537   925463 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)   
(Intercept)  1.619e+05  7.499e+04   2.159  0.03600 * 
z.lag.1     -7.959e-01  2.375e-01  -3.351  0.00159 **
z.diff.lag  -4.277e-01  2.407e-01  -1.777  0.08204 . 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 386400 on 47 degrees of freedom
Multiple R-squared:  0.521,	Adjusted R-squared:  0.5006 
F-statistic: 25.56 on 2 and 47 DF,  p-value: 3.08e-08


Value of test-statistic is: -3.3514 5.6302 

Critical values for test statistics: 
      1pct  5pct 10pct
tau2 -3.51 -2.89 -2.58
phi1  6.70  4.71  3.86

> summary(C1PIBpc_ur_none.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1517487   -70050   120941   273898  1113094 

Coefficients:
           Estimate Std. Error t value Pr(>|t|)   
z.lag.1     -0.4468     0.1804  -2.476  0.01685 * 
z.diff.lag  -0.7150     0.2080  -3.437  0.00123 **
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 400900 on 48 degrees of freedom
Multiple R-squared:  0.4747,	Adjusted R-squared:  0.4528 
F-statistic: 21.68 on 2 and 48 DF,  p-value: 1.953e-07


Value of test-statistic is: -2.4763 

Critical values for test statistics: 
     1pct  5pct 10pct
tau1 -2.6 -1.95 -1.61

```

Dado que en la prueba ADF con intercepto y tendencia de la variable $PIBpc$, ambos terminos dieron significativos, entonces se decidio testear la prueba ADF con intercepto y con tendencia. Dado que en la prueba ADF con intercepto de la variable $C1PIBpc$, el intercepto dio significativo, entonces se decidio testear la prueba ADF con intercepto. 

Los resultados de la misma son:

| Variable   | Estadistico | Valor      |  1%     |  5%     |  10%    |
|------------|-------------|:----------:|:-------:|:-------:|:-------:|
| $PIBpc$    | $\tau_\tau$ |  $-2.1093$ | $-4.04$ | $-3.45$ | $-3.15$ |
| $PIBpc$    | $\phi_2$    |  $3.2214$  |  $6.50$ |  $4.88$ |  $4.16$ |
| $PIBpc$    | $\phi_3$    |  $2.2795$  |  $8.73$ |  $6.49$ |  $5.47$ |
| $C1PIBpc$  | $\tau_\mu$  |  $-3.3514$ | $-3.51$ | $-2.89$ | $-2.58$ |
| $C1PIBpc$  | $\phi_1$    |  $5.6302$  |  $6.70$ |  $4.71$ |  $3.86$ |

En consecuencia, la variable $PIBpc$ es no estacionaria porque:
* $ \tau_\tau = **-2.1093**>-3.15$, es decir, no se rechaza $\gamma=0$ al 10%, 5% y 1%
* $\phi_2 = **3.2214**<4.16$, es decir, no se rechaza $\gamma=a_0=a_2=0$ al 10%, 5% y 1%
* $\phi_3 =**2.2795**<5.47$, es decir, no se rechaza $a_0=\gamma=a_2=0$ al 10%, 5% y 1%

Y la variable $C1PIBpc$ es estacionaria porque
* $\tau_\mu=-3.51<**-3.3514**<-3.15$, es decir, se rechaza $\gamma=0$ al 1% y se rechaza al 5% y 10%
* $\phi_1=4.71<**5.6302**<6.70$, es decir, $\gamma=a_0=0$ no se rechaza al 1%  y se rechaza al se rechaza
  
### Prueba DF-GLS
``` r
PIBpc_DFGLS1c.ers <- ur.ers(PIBpc, type = c("DF-GLS"), model = c("constant"),lag.max = 4)
PIBpc_DFGLS1t.ers <- ur.ers(PIBpc, type = c("DF-GLS"), model = c("trend"),lag.max = 4)
PIBpc_DFGLS2c.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("constant"),lag.max = 4)
PIBpc_DFGLS2t.ers <- ur.ers(PIBpc, type = c("P-test"), model = c("trend"),lag.max = 4)
C1PIBpc_DFGLS1c.ers <- ur.ers(C1PIBpc, type = c("DF-GLS"), model = c("constant"),lag.max = 4)
C1PIBpc_DFGLS1t.ers <- ur.ers(C1PIBpc, type = c("DF-GLS"), model = c("trend"),lag.max = 4)
C1PIBpc_DFGLS2c.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("constant"),lag.max = 4)
C1PIBpc_DFGLS2t.ers <- ur.ers(C1PIBpc, type = c("P-test"), model = c("trend"),lag.max = 4)
summary(PIBpc_DFGLS1c.ers)
summary(PIBpc_DFGLS1t.ers)
summary(PIBpc_DFGLS2c.ers)
summary(PIBpc_DFGLS2t.ers)
summary(C1PIBpc_DFGLS1c.ers)
summary(C1PIBpc_DFGLS1t.ers)
summary(C1PIBpc_DFGLS2c.ers)
summary(C1PIBpc_DFGLS2t.ers)
```

Obteniendfo
``` r
> summary(PIBpc_DFGLS1c.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
     Min       1Q   Median       3Q      Max 
-1533672   -78416   122458   213693  1081509 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
yd.lag        0.00747    0.01692   0.441   0.6607  
yd.diff.lag1 -0.15035    0.17071  -0.881   0.3825  
yd.diff.lag2  0.50297    0.26788   1.878   0.0661 .
yd.diff.lag3  0.32225    0.28992   1.112   0.2715  
yd.diff.lag4 -0.13269    0.26043  -0.510   0.6125  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 381300 on 52 degrees of freedom
Multiple R-squared:  0.2501,	Adjusted R-squared:  0.178 
F-statistic: 3.469 on 5 and 52 DF,  p-value: 0.008844


Value of test-statistic is: 0.4415 

Critical values of DF-GLS are:
                1pct  5pct 10pct
critical values -2.6 -1.95 -1.62

> summary(PIBpc_DFGLS1t.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept and trend 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
     Min       1Q   Median       3Q      Max 
-1368613  -154555   -11937   122735   937826 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
yd.lag       -0.14125    0.06638  -2.128   0.0381 *
yd.diff.lag1 -0.19729    0.15594  -1.265   0.2115  
yd.diff.lag2  0.45223    0.24830   1.821   0.0743 .
yd.diff.lag3  0.32346    0.26771   1.208   0.2324  
yd.diff.lag4 -0.07243    0.25251  -0.287   0.7754  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 348400 on 52 degrees of freedom
Multiple R-squared:  0.1728,	Adjusted R-squared:  0.09326 
F-statistic: 2.172 on 5 and 52 DF,  p-value: 0.07132


Value of test-statistic is: -2.128 

Critical values of DF-GLS are:
                 1pct  5pct 10pct
critical values -3.58 -3.03 -2.74

> summary(PIBpc_DFGLS2c.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept 

Value of test-statistic is: 175.4555 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 1.95 3.11  4.17

> summary(PIBpc_DFGLS2t.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept and trend 

Value of test-statistic is: 12.9039 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 4.26 5.64  6.79

> summary(C1PIBpc_DFGLS1c.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
     Min       1Q   Median       3Q      Max 
-1574633  -143511   -11175   121083   898811 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)   
yd.lag        -0.9802     0.2956  -3.316  0.00169 **
yd.diff.lag1  -0.2375     0.2998  -0.792  0.43178   
yd.diff.lag2   0.1356     0.2924   0.464  0.64483   
yd.diff.lag3   0.3857     0.2685   1.436  0.15703   
yd.diff.lag4   0.2513     0.2477   1.014  0.31514   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 363200 on 51 degrees of freedom
Multiple R-squared:  0.5425,	Adjusted R-squared:  0.4977 
F-statistic:  12.1 on 5 and 51 DF,  p-value: 9.52e-08


Value of test-statistic is: -3.3158 

Critical values of DF-GLS are:
                1pct  5pct 10pct
critical values -2.6 -1.95 -1.62

> summary(C1PIBpc_DFGLS1t.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type DF-GLS 
detrending of series with intercept and trend 


Call:
lm(formula = dfgls.form, data = data.dfgls)

Residuals:
     Min       1Q   Median       3Q      Max 
-1766113  -192022    -3470    97875   891789 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
yd.lag       -0.73413    0.29287  -2.507   0.0154 *
yd.diff.lag1 -0.37093    0.32444  -1.143   0.2583  
yd.diff.lag2  0.04344    0.31115   0.140   0.8895  
yd.diff.lag3  0.33414    0.28654   1.166   0.2490  
yd.diff.lag4  0.20620    0.26144   0.789   0.4339  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 378600 on 51 degrees of freedom
Multiple R-squared:  0.5023,	Adjusted R-squared:  0.4535 
F-statistic: 10.29 on 5 and 51 DF,  p-value: 7.348e-07


Value of test-statistic is: -2.5067 

Critical values of DF-GLS are:
                 1pct  5pct 10pct
critical values -3.58 -3.03 -2.74

> summary(C1PIBpc_DFGLS2c.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept 

Value of test-statistic is: 0.4162 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 1.95 3.11  4.17

> summary(C1PIBpc_DFGLS2t.ers)

############################################### 
# Elliot, Rothenberg and Stock Unit Root Test # 
############################################### 

Test of type P-test 
detrending of series with intercept and trend 

Value of test-statistic is: 0.8719 

Critical values of P-test are:
                1pct 5pct 10pct
critical values 4.26 5.64  6.79
```

Dado que el $PIBpc$ es una variable que presenta tendencia, entonces se decide hacer su pruba con tendencia. Dado que el $C1PIBpc$ es una variable que no presenta tendencia, entonces se decide hacer su pruba sin tendencia. Los resultados de las misma son:

| Variable   | Estadistico   | Valor     |  10%    |  5%     |   1%    |
|------------|---------------|:---------:|:-------:|:-------:| :------:|
| $PIBpc$    | $DF-GLS_\tau$ | $-2.128$  | $-2.74$ | $-3.03$ | $-3.58$ |
| $PIBpc$    | $P_\tau$      | $25.8748$ | $6.79$  | $5.64$  | $4.26$  |
| $C1PIBpc$  | $DF-GLS_\mu$  | $-3.3158$ | $-1.62$ | $-1.95$ | $-2.60$ |
| $C1PIBpc$  | $P_\mu$       | $0.4162$  | $4.17$  | $3.11$  | $1.95$  |

La variable $PIBpc$ es no estacionaria porque:
* Con el estadistico $DF-GLS_\tau$: $**-2.128**>-2.74$, no se rechaza la hipótesis nula de raíz unitaria al 10%, 5% y 1% 
* Con el estadistico $P_\tau$: $**25.8748**>6.79$, no se rechaza la hipótesis nula de raíz unitaria  10%, 5% y 1% 

La variable $C1PIBpc$ es estacionaria porque:
* Con el estadistico $DF-GLS_\mu$: $**-3.3158**<-2.60$, se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%
* Con el estadistico $P_\mu$: $**0.4162**>1.95$, no se rechaza la hipótesis nula de raíz unitaria al 1%, 5% y 10%

### Prueba KPSS
Vamos a aplicar las opciones de rezago de R. Sin embargo, no olviden que según  en Newey y West (1994) la longitud de rezago se establece proporcional a $T^{1/3}$, en decir $62^{1/3}=3.96=4$, en nuestro caso: .

``` r
kpss1m.kpss <- ur.kpss(PIBpc, type = c("mu"), lags = c("short"), use.lag = NULL)
kpss2m.kpss <- ur.kpss(PIBpc, type = c("mu"), lags = c("long"), use.lag = NULL)
kpss3m.kpss <- ur.kpss(PIBpc, type = c("mu"), lags = c("nil"), use.lag = NULL)
kpss4m.kpss <- ur.kpss(PIBpc, type = c("mu"), use.lag = 4)
kpss1t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("short"), use.lag = NULL)
kpss2t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("long"), use.lag = NULL)
kpss3t.kpss <- ur.kpss(PIBpc, type = c("tau"), lags = c("nil"), use.lag = NULL)
kpss4t.kpss <- ur.kpss(PIBpc, type = c("tau"), use.lag = 4)

summary(kpss1m.kpss)
summary(kpss2m.kpss)
summary(kpss3m.kpss)
summary(kpss4m.kpss)
summary(kpss1t.kpss)
summary(kpss2t.kpss)
summary(kpss3t.kpss)
summary(kpss4t.kpss)
```
Obteniendo
``` r
> summary(kpss1m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 3 lags. 

Value of test-statistic is: 1.555 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(kpss2m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 10 lags. 

Value of test-statistic is: 0.6611 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(kpss3m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 0 lags. 

Value of test-statistic is: 5.8294 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(kpss4m.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: mu with 4 lags. 

Value of test-statistic is: 1.269 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.347 0.463  0.574 0.739

> summary(kpss1t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 3 lags. 

Value of test-statistic is: 0.2378 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(kpss2t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 10 lags. 

Value of test-statistic is: 0.1297 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(kpss3t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 0 lags. 

Value of test-statistic is: 0.8286 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

> summary(kpss4t.kpss)

####################### 
# KPSS Unit Root Test # 
####################### 

Test is of type: tau with 4 lags. 

Value of test-statistic is: 0.1994 

Critical value for a significance level of: 
                10pct  5pct 2.5pct  1pct
critical values 0.119 0.146  0.176 0.216

```
Dado que el PIBpc es una variable que presenta tendencia, entonces se decide testear dicha prueba. Los resultados de la misma son:

| Estadistico | Rezagos        | Valor      |  10%    |  5%     |  2.5%   |  1%     |
|-------------|:--------------:|:----------:|:-------:|:-------:|:-------:|:-------:|
| $KPSS_\tau$ |  short= $3$    |  $0.2378$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| $KPSS_\tau$ |  long= $10$    |  $0.1297$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| $KPSS_\tau$ |  nil= $0$      |  $0.8286$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |
| $KPSS_\tau$ |  NW(1994)= $4$ |  $0.1994$  | $0.119$ | $0.146$ | $0.176$ | $0.216$ |

La variable es no estacionaria porque:
* Cuando el número de rezagos es short= $3$: $0.2378>0.216$, se rechaza la hipótesis nula de estacionariedad
* Cuando el número de rezagos es short= $10$: $0.146>0.1297>0.119$, se rechaza la hipótesis nula de estacionariedad al 10% pero no al 5%, 2.% y 1%
* Cuando el número de rezagos es nil= $0$: $0.8286>0.216$, se rechaza la hipótesis nula de estacionariedad
* Cuando el número de rezagosa es el establecido por la formula de Newey y West (1994)= $4$: $0.216>0.1994>0.176$, se rechaza la hipótesis nula de estacionariedad al 2.5%, 5% y 10% pero no al 1%

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



