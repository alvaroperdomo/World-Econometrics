## SECCIÓN 3.1.4
# Caso de estudio de un modelo $VAR$ utilizando la base de datos _Indicadores de Desarrollo Mundial_

A continuación, se presenta un caso de estudio utilizando la base de datos _Indicadores de Desarrollo Económico del Banco Mundial_. 

## Contexto del problema a analizar
Uno de los debates que existen en economía esta relacionado con el efecto que ejerce el el gasto del gobierno (o la inversión del gobieno o la deuda del gobierno) sobre la inversión privada:
* Por un lado, están quienes establecen que se da un efecto _crowding-out_ (o efecto desplazamiento) en donde el mayor gasto del gobierno, presiona a un incremento de la tasa de interés, perjudicando de esta forma a los proyectos privados de inversión.
* Por otro lado, están quienes establecen que se da un efecto _crowding-in_ (o efecto expansión) en donde un mayor gasto del gobierno reactiva la demanda, lo que hace que aumente la producción (es decir, la oferta) y por esta vía se afecta positivamente a los proyectos de inversión privada. 

Entonces, alguien podría pensar que con una estimación de Mínimos Cuadrados Ordinarios ($MCO$) se podría resolver este dilema. Sin embargo, hay que tener en cuenta que una mayor inversión privada reactiva la economía, y por esta vía el gobierno recauda más impuestos y puede aumentar su gasto. Por lo tanto, estamos ante un problema de doble causalidad en donde una estimación de $MCO$ no sería recomendable por la potencial presencia se estimaciones sesgadas. Una de las formas de resolver este tipo de inconvenientes es con la utilización de estimaciones de modelos $VAR$. 

Después de la Segunda Guerra Mundial, uno de los países exitosos en términos de desarrollo y crecimiento económico fue Corea del Sur. Por lo tanto, valdría la pena analizar el potencial rol que tuvo el gasto del gobierno en este proceso, en particular en cuanto al estimulo o desestimulo que tuvó sobre la inversión privada del país. 

El caso de estudio de esta sección esta relacionado con los párrafos que acabamos de platear. El código de $R$ que se propopone para hacer el análisis es el siguiente:

## Preparando la base de datos y análisis gráfico
En primera instancia, vamos a limpiar el área de trabajo y a llamar las librerías a utilizar. 
``` r
rm(list = ls())

library(WDI)         # Esta librería sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta librería permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(forecast)    # Esta librería sirve para hacer pronósticos.
library(vars)        # Esta librería se utiliza para la estimación de los modelos VAR

```
Después de revisar la base de datos _Indicadores de Desarrollo Mundial_ se decidió que las dos variables que representan al gasto público y a la inversión privada se van a manejar como % del PIB [^1] y se llaman en los _Indicadores de Desarrollo Mundial_ así: [^2]

[^1]: **Para de esta forma, como es usual en el manejo econométrico de variables económicas, poder manejarlas en términos reales y así para aislarlas del efecto de la inflación.**
[^2]: **El gasto de consumo final del Gobierno general incluye todos los gastos corrientes para la adquisición de bienes y servicios (incluida la remuneración de los empleados). También comprende la mayor parte del gasto en defensa y seguridad nacional, pero no incluye los gastos militares del Gobierno que forman parte de la formación de capital del Gobierno. La formación bruta de capital fijo del sector privado se define como la inversión privada que cubre los desembolsos brutos del sector privado (incluidos organismos privados sin fines de lucro) además de sus activos fijos nacionales.**

``` r
> WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')
           indicator                                                        name
11014 NE.CON.GOVT.ZS General government final consumption expenditure (% of GDP)
> WDIsearch(string='NE.GDI.FPRV.ZS', field='indicator')
           indicator                                                     name
11117 NE.GDI.FPRV.ZS Gross fixed capital formation, private sector (% of GDP)
```
Con el siguiente comando descargamos ambas variables para Corea del Sur, las renombramos como $GGOV$ (Gasto Público como % del PIB) e $INVP$ (Inversión Privada como % del PIB), y construimos una base de datos llamada "dat":
``` r
dat = WDI(indicator= c(INVP="NE.GDI.FPRV.ZS", GGOV = "NE.CON.GOVT.ZS"), country=c('KR'), language = "es")
```
Al visualizar los datos, se puede ver que ya estan ordenados del más antiguo al más nuevo,

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/94a117f9-c3b7-41e1-a6ca-495f07af6712)

entonces, con los siguientes comandos a procedemos a depurar la base de datos: **(1)** eliminando los años en los cuales no tenemos información para ambas variables (es decir, de 1960 a 1969) y **(2)** eliminando las columnas que no incluyan a las dos variables analizadas:
``` r
dat <- na.omit(dat)                                                       # Esta orden elimina los años en los cuales no tenemos información para ambas variables.
dat <- mutate(dat, year=NULL, country=NULL, iso2c=NULL, iso3c=NULL)       # Esta orden elimina las columnas year, country, iso2c y iso3c de la base de datos.
```
Entonces la base queda así:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/a4e1a02a-fc5c-4aba-ad82-1f8f6cbf43d3)

Ahora vamos a determinar que las dos variables ($GGOV$ e $INVP$) conforman un vector en formato de serie de tiempo anual (llamado $seriesVAR$) que va desde 1970 hasta 2022:
``` r
seriesVAR <-ts(dat,frequency=1,start = 1970)
```
Con el siguiente comando, visualizamos un gráfico de las dos variables:
``` r
ts.plot(seriesVAR, xlab="Años", ylab="% del PIB", col=c("red","blue"), main="Corea del Sur - Series en niveles: 1970 - 2022")
legend("topleft", legend = c("GGOV","INVP"), col = c("red","blue"), lty = 1)
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/022ef39c-d4ea-4f83-aa14-49f4c5afbccd)

En este gráfico, es interesante apreciar que el impacto que tuvó, en ambas variables, la crisis financiera del país de 1997-1998 fue superior al de la crisis financiera internacional de 2008 y al de la crisis del Covid-19 en 2020. Igualmente, es de destacar que las variables analizadas han sido menos fluctuantes este siglo que lo que fueron el siglo pasado.

En un análisis previo, que no mostramos pero que es similar al de la sección 2 (en cuanto a las pruebas de raíz unitaria), se concluyó que ambas series ($GGOV$ e $INVP$) son integradas de orden uno. Es decir, no son estacionarias en niveles, pero si lo son en primeras diferencias. Por lo tanto, el modelo $VAR$ que vamos a estimar va a manejar las variables en primeras diverencias. Las variables a incluir en el $VAR$ las llamaremos $\Delta GGOV$ y $\Delta INVP$ (representando a la primera diferencia de las variables $GGOV$ e $INVP$ respectivamente). 

Antes de comenzar el análisis $VAR$, gráfiquemos las variables a estudiar ($\Delta GGOV$ y $\Delta INVP$), para ello se utiliza el siguiente domando:
``` r
ts.plot(diff(seriesVAR), xlab="Años",ylab="% del PIB",col=c("red","blue"), main="Corea del Sur - Series en diferencias: 1970 - 2022")
legend("topright", legend = c("dif.GGOV","dif.INVP"), col = c("red","blue"), lty = 1)
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/da96898b-e37e-4709-8ac5-5a34c51b0e48)

Lo más destacable del gráfico de arriba es fuerte caida que tuvó la inversión privada a raíz de la crisis financiera de finales de los 1990s.

## Estimación del $VAR$ en primeras diferencias

Dado que el gráfico previo pareciera indicar que $\Delta GGOV$ y $\Delta INVP$ no tienen pendiente y potencialmente no tienen intercepto, entonces el $VAR$ a estimar lo escogeremos entre los modelos $VAR$ sin intercepto ni tendencia, y los modelos $VAR$ con intercepto pero sin tendencia. Por lo tanto, el orden de los rezagos del $VAR$ a estimar se va a escoger con los siguientes comandos:[^3]

[^3]: **Observe que el vector estimado dentro del _VAR_ es _diff(seriesVAR)_. Es decir, las variables de nuestro vector _seriesVAR_ se han incluido en primeras diferencias dentro del _VAR_.**
``` r
VARselect(diff(seriesVAR),lag.max=10,type="const")
VARselect(diff(seriesVAR),lag.max=10,type="none")
```
Obteniendo
``` r
> VARselect(diff(seriesVAR),lag.max=10,type="const")
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     2      1      1      2 

$criteria
                1          2           3          4          5           6          7          8          9         10
AIC(n) -0.7302187 -0.7653847 -0.59593383 -0.6070840 -0.4428946 -0.33467465 -0.2332242 -0.1308129 -0.1054703 -0.2299714
HQ(n)  -0.6392294 -0.6137360 -0.38362561 -0.3341163 -0.1092675  0.05961203  0.2217220  0.3847928  0.4707948  0.4069532
SC(n)  -0.4819802 -0.3516538 -0.01671062  0.1376315  0.4673133  0.74102559  1.0079684  1.2758721  1.4667069  1.5076982
FPE(n)  0.4820385  0.4662116  0.55451799  0.5523404  0.6584253  0.74624958  0.8458893  0.9679864  1.0364124  0.9677835

> VARselect(diff(seriesVAR),lag.max=10,type="none")
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     2      1      1      2 

$criteria
                1          2          3           4          5           6          7          8           9         10
AIC(n) -0.7346617 -0.7943845 -0.6353181 -0.62088202 -0.4729811 -0.35380577 -0.2372432 -0.1080725 -0.07073884 -0.1365550
HQ(n)  -0.6740022 -0.6730655 -0.4533397 -0.37824407 -0.1696836  0.01015116  0.1873732  0.3772034  0.47519656  0.4700399
SC(n)  -0.5691693 -0.4633998 -0.1388411  0.04108735  0.3544806  0.63914829  0.9212032  1.2158663  1.41869226  1.5183685
FPE(n)  0.4797369  0.4523829  0.5318560  0.54255717  0.6348628  0.72535827  0.8317011  0.9731772  1.04870588  1.0313483
```
|Estadistico                                          | Modelo más Parsimonioso <br> sin constante  | Valor       | Modelo más Parsimonioso <br> con constante  | Valor       | 
|-----------------------------------------------------|:-------------------------------------------:|:-----------:|:-------------------------------------------:|:-----------:|
|$\text{Criterio de Información de Akaike}$: AIC      |2 rezagos                                    | -0.7943845  |2 rezagos                                    | -0.7653847  |    
|$\text{Criterio de Información de Hannan-Quinn}$: HQ |1 rezago                                     | -0.6740022  |1 rezago                                     | -0.6392294  | 
|$\text{Criterio Bayesiano de Schwartz}$: SC          |1 rezago                                     | -0.5691693  |1 rezago                                     | -0.4819802  | 


Es decir, el modelo $VAR$ más parsimonioso tiene uno o dos rezagos (según el criterio de información escogido) y no lleva constante. No hay una opción clara acerca de si es mejor con uno o con dos rezagos. Sin embargo, al hacer las pruebas de diagnóstico, la $FAC$ del modelo con un rezago mostraba algunos problemas de autocorrelación de los residuos estimados. Esto no pasaba con el modelo con dos rezagos, entonces se decidio estimar un modelo $VAR(2)$ sin término constante. 

Ahora estimamos el $VAR(2)$ con los siguientes comandos:
``` r
modeloVAR<-VAR(diff(seriesVAR),p=1,type="none")
summary(modeloVAR)
```
Obteniendo
``` r
> modeloVAR<-VAR(diff(seriesVAR),p=2,type="none")
> summary(modeloVAR)

VAR Estimation Results:
========================= 
Endogenous variables: GGOV, INVP 
Deterministic variables: none 
Sample size: 50 
Log Likelihood: -125.952 
Roots of the characteristic polynomial:
0.7304 0.7304 0.4988 0.323
Call:
VAR(y = diff(seriesVAR), p = 2, type = "none")


Estimation results for equation GGOV: 
===================================== 
GGOV = GGOV.l1 + INVP.l1 + GGOV.l2 + INVP.l2 

        Estimate Std. Error t value Pr(>|t|)  
GGOV.l1  0.08115    0.15503   0.523   0.6032  
INVP.l1  0.04308    0.05859   0.735   0.4659  
GGOV.l2  0.03386    0.14919   0.227   0.8214  
INVP.l2  0.14525    0.05830   2.492   0.0164 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1


Residual standard error: 0.5906 on 46 degrees of freedom
Multiple R-Squared: 0.189,	Adjusted R-squared: 0.1185 
F-statistic:  2.68 on 4 and 46 DF,  p-value: 0.0432 


Estimation results for equation INVP: 
===================================== 
INVP = GGOV.l1 + INVP.l1 + GGOV.l2 + INVP.l2 

         Estimate Std. Error t value Pr(>|t|)   
GGOV.l1  0.006853   0.374551   0.018  0.98548   
INVP.l1  0.409020   0.141552   2.890  0.00587 **
GGOV.l2  0.491871   0.360431   1.365  0.17899   
INVP.l2 -0.428513   0.140848  -3.042  0.00387 **
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1


Residual standard error: 1.427 on 46 degrees of freedom
Multiple R-Squared: 0.3297,	Adjusted R-squared: 0.2714 
F-statistic: 5.657 on 4 and 46 DF,  p-value: 0.0008661 



Covariance matrix of residuals:
        GGOV    INVP
GGOV  0.3330 -0.3077
INVP -0.3077  2.0230

Correlation matrix of residuals:
        GGOV    INVP
GGOV  1.0000 -0.3749
INVP -0.3749  1.0000
```

Observe que en este VAR las cuatro raíces caracteristicas del polinomio que lo resuelven ($0.7304 0.7304 0.4988 0.323$) en valor absoluto son menores que la unidad. Por lo tanto, esto nos lleva a plantear que el $VAR$ es estable [^4]. Por si alcaso, No olvide que las raíces caracteristicas de un $VAR$ que se ha estimado, también se pueden recuperar con el comando

[^4]: **Si hubieramos estimado el _VAR(1)_ en vez del _VAR(2)_, esté hubiera sido más estable con dor raices caracteristicas iguanles a 0.2754.

```r
roots(modeloVAR)
```
Obteniendo, 

```r
> roots(modeloVAR)
[1] 0.7304 0.7304 0.4988 0.323
```

## Pruebas sobre los residuos estimados

Con los siguientes comandos podemos visualizar la $FAC$ y la $FACP$ de los residuos estimados en cada una de las ecuaciones estimadas
```r
acf(residuals(modeloVAR)[,1])
pacf(residuals(modeloVAR)[,1])
acf(residuals(modeloVAR)[,2])
pacf(residuals(modeloVAR)[,2])
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/03ee25ee-ffbd-4702-b385-941346b738e4)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/f1c89158-7f90-4993-a7ab-8a3c0f0c4fd3)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/c1aa7df6-4183-4437-bf7a-7be8734dc092)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/b775d203-7a1c-4ba8-ac91-85f32dca1165)

De los cuatro gráficos de arriba se puede apreciar que los errores estimados de la ecuación de $\Delta GGOV$ no estan autocorrelacionados, y en buena medida los errores estimados de la ecuación de $\Delta INVP$ sno estan autocorrelacionados, aunque la información del rezago 8 si muestra una leve autocorrelación.  

También se llevó a cabo una prueba Portmanteau de correlación, la cual se calcula con el siguiente comando:
```r
serial.test(modeloVAR,lags.pt=10)
```
Obteniendose el siguiente resultado:
```r
	Portmanteau Test (asymptotic)


data:  Residuals of VAR object modeloVAR
Chi-squared = 25.257, df = 32, p-value = 0.7955
```
Es decir, no se rechaza la hipótesis nula de no correlación de los residuos estimados en el $VAR$

## Prueba de causalidad de granger, funciones impulso-respuesta y análisis de descomposición de varianza
Las relaciones de causalidad, en el sentido de Granger, entre $\Delta GGOV$ y $\Delta INVP$, tomando en cuenta los dos rezagos que tiene el $VAR$, se obtienen a partir de los comandos:

``` r
grangertest(diff(GGOV) ~ diff(INVP),order=2,data=seriesVAR)
grangertest(diff(INVP) ~ diff(GGOV),order=2,data=seriesVAR)
```
En donde se llega a
``` r
> grangertest(diff(GGOV) ~ diff(INVP),order=2,data=seriesVAR)
Granger causality test

Model 1: diff(GGOV) ~ Lags(diff(GGOV), 1:2) + Lags(diff(INVP), 1:2)
Model 2: diff(GGOV) ~ Lags(diff(GGOV), 1:2)
  Res.Df Df      F  Pr(>F)  
1     45                    
2     47 -2 3.3972 0.04226 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
> 
> grangertest(diff(INVP) ~ diff(GGOV),order=2,data=seriesVAR)
Granger causality test

Model 1: diff(INVP) ~ Lags(diff(INVP), 1:2) + Lags(diff(GGOV), 1:2)
Model 2: diff(INVP) ~ Lags(diff(INVP), 1:2)
  Res.Df Df      F Pr(>F)
1     45                 
2     47 -2 0.6134  0.546
```
A partir de la prueba de causalidad de Granger, se revela que va es más importante el efecto causal de $\Delta GGOV$ en $\Delta INVP$, que el efecto causal de $\Delta INVP$ en $\Delta GGOV$. Por lo tanto, al establecer las funciones impulso-respuesta se ha estructurado la descomposición de Choleski del $VAR$ de tal forma que $\Delta GGOV$ tiene efectos contemporaneos y rezagados en $\Delta INVP$, pero  $\Delta INVP$ no tiene un efecto contemporaneo en $\Delta GGOV$, sólo efectos rezagados 

Para calcular y dibujar las funciones impulso-respuesta, se copian los comandos
``` r
modelo.irf1<-irf(modeloVAR,impulse="GGOV", response=NULL, ci=0.9)
modelo.irf2<-irf(modeloVAR,impulse="INVP", response=NULL, ci=0.9)
modelo.irf1
modelo.irf2

plot(modelo.irf1)
plot(modelo.irf2)
```
Se obtiene, 

``` r
> modelo.irf1<-irf(modeloVAR,impulse="GGOV", response=NULL, ci=0.9)
> modelo.irf2<-irf(modeloVAR,impulse="INVP", response=NULL, ci=0.9)
> modelo.irf1

Impulse response coefficients
$GGOV
              GGOV        INVP
 [1,]  0.590644104 -0.49638162
 [2,]  0.026550555 -0.19898230
 [3,] -0.058516195  0.42202107
 [4,] -0.014573444  0.27054023
 [5,]  0.069788388 -0.09906743
 [6,]  0.040198906 -0.16314064
 [7,] -0.015791542  0.01032624
 [8,] -0.023171847  0.09379602
 [9,]  0.003124989  0.02601335
[10,]  0.014213452 -0.04092899
[11,]  0.003274718 -0.02625336


Lower Band, CI= 0.9 
$GGOV
               GGOV         INVP
 [1,]  0.4612368951 -0.987808718
 [2,] -0.0835777644 -0.629472957
 [3,] -0.2039733304  0.076090377
 [4,] -0.0891649486  0.051665578
 [5,]  0.0030164323 -0.306284608
 [6,] -0.0064741203 -0.373988037
 [7,] -0.0708172039 -0.118914232
 [8,] -0.0588894533 -0.001703019
 [9,] -0.0221869019 -0.084583356
[10,] -0.0009911329 -0.155253431
[11,] -0.0178927448 -0.105496351


Upper Band, CI= 0.9 
$GGOV
             GGOV         INVP
 [1,] 0.658037030 -0.132598916
 [2,] 0.154333929  0.164863053
 [3,] 0.068698394  0.747162496
 [4,] 0.058348092  0.501556489
 [5,] 0.150799284  0.086158020
 [6,] 0.077163453 -0.002673337
 [7,] 0.022234893  0.179499557
 [8,] 0.003237764  0.262758484
 [9,] 0.036539478  0.129397281
[10,] 0.046330565  0.018938981
[11,] 0.019897417  0.038489435

> modelo.irf2

Impulse response coefficients
$INVP
              GGOV        INVP
 [1,]  0.000000000  1.33787875
 [2,]  0.057629306  0.54721982
 [3,]  0.222576503 -0.34907966
 [4,]  0.084461626 -0.34740004
 [5,] -0.051277350  0.11754935
 [6,] -0.046698002  0.23813842
 [7,]  0.021805998  0.02149015
 [8,]  0.035703860 -0.11607551
 [9,]  0.001757344 -0.04571564
[10,] -0.017477678  0.04861501
[11,] -0.005904993  0.04021889


Lower Band, CI= 0.9 
$INVP
               GGOV        INVP
 [1,]  0.0000000000  1.02848211
 [2,] -0.1013898380  0.26770527
 [3,]  0.0931850408 -0.53622437
 [4,] -0.0036425660 -0.60558698
 [5,] -0.1112843776 -0.09066513
 [6,] -0.0954135688  0.06063382
 [7,] -0.0111818086 -0.13910903
 [8,]  0.0008186713 -0.22483071
 [9,] -0.0330256555 -0.14949646
[10,] -0.0476310847 -0.03057174
[11,] -0.0274938627 -0.02331717


Upper Band, CI= 0.9 
$INVP
             GGOV        INVP
 [1,] 0.000000000  1.49576063
 [2,] 0.148687841  0.82222463
 [3,] 0.340868890 -0.01979331
 [4,] 0.152582871 -0.05531108
 [5,] 0.037158017  0.33643610
 [6,] 0.023002660  0.44254760
 [7,] 0.078366068  0.25145458
 [8,] 0.078782877  0.01556297
 [9,] 0.033873465  0.07058687
[10,] 0.008663633  0.14420700
[11,] 0.018597136  0.16645116

> 
> plot(modelo.irf1)
> plot(modelo.irf2)
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5b4f3d89-abde-48e6-9809-c7672a282fba)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/1b7e0faa-650b-409c-801e-564b463d178f)


Las funciones impulso-respuesta revelan que:
* Un choque de gasto del gobierno afecta positivamente y de manera significativa al gasto del gobierno en el corto plazo.
* Un choque de gasto del gobierno tiene un efecto crowding-out significativo sobre la inversión privada en el corto plazo, pero tiene un efecto crowding-in significativo sobre la inversión privada en el mediano plazo.
* Un choque de inversión privada tiene un impacto positivo y significativo sobre el gasto gobierno y la inversión privada en el corto plazo, siendo más importante el efecto sobre la inversión.

## Pronosticos

Para hacer un pronóstico de 3 años hacia adelante, operamos el siguiente comando:
``` r
forecast3<-forecast(modeloVAR, level = c(95), h = 3)
autoplot(forecast3)
```
Obteniendose,
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/2f6d7218-d82c-45e1-85a5-f5950f23888c)


forecasts <- predict(modelo)
forecasts
plot(forecasts)







