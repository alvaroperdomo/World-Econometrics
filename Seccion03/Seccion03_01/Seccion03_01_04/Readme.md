<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.1.4:
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

### Selección del número de rezagos
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

### Prueba de causalidad de Granger
La prueba de causalidad de Granger es útil para de una vez establecer el orden de las variables dentro del $VAR$ (de la más exógena a la más endógena) de tal forma que las variables ya esten ordenadas según la descomposición de Choleski y así más adelante tener el $VAR$ estructural (o mejor aún, el $VMA$ estructural) que vaya a ser utilizado en el cálculo de las funciones imulso-respuesta y de las descomposiciones de varianza. 

Las relaciones de causalidad, en el sentido de Granger, entre $\Delta GGOV$ y $\Delta INVP$, tomando en cuenta los dos rezagos que va a tener el $VAR$, se obtienen a partir de los comandos:

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
A partir de la prueba de causalidad de Granger, se revela que es más importante el efecto causal de $\Delta GGOV$ en $\Delta INVP$, que el efecto causal de $\Delta INVP$ en $\Delta GGOV$. Por lo tanto, al establecer las funciones impulso-respuesta se ha estructurado la descomposición de Choleski del $VAR$ estructural de tal forma que $\Delta GGOV$ tiene efectos contemporaneos y rezagados en $\Delta INVP$, pero  $\Delta INVP$ no tiene un efecto contemporaneo en $\Delta GGOV$, sólo efectos rezagados. 

### Estimación del $VAR$

Ahora estimamos el $VAR(2)$ con los siguientes comandos:
``` r
modeloVAR<-VAR(diff(seriesVAR[, c("GGOV", "INVP")]), p = 2, type = "none") # El orden de las variables, de una vez, se establece de la más exógena a la más endogena de acuerdo a la descomposición de Choleski
summary(modeloVAR)
```
Obteniendo
``` r
> modeloVAR <- VAR(diff(seriesVAR[, c("GGOV", "INVP")]), p = 2, type = "none")
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

Observe que en este modelo $VAR$ las cuatro raíces caracteristicas del polinomio que lo resuelven ($0.7304 0.7304 0.4988 0.323$) en valor absoluto son menores que la unidad. Por lo tanto, esto nos lleva a plantear que el $VAR$ es estable [^4]. Por si a caso, No olvide que las raíces caracteristicas de un $VAR$ que se ha estimado, también se pueden recuperar con el comando:

[^4]: **Si hubieramos estimado el _VAR(1)_ en vez del _VAR(2)_, esté hubiera sido más estable con dos raices caracteristicas iguanles a 0.2754.

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

## Funciones impulso-respuesta y análisis de descomposición de varianza

### Funciones impulso-respuesta
Vamos a calcular y dibujar funciones impulso-respuesta utilizando los siguientes comandos:
``` r
modelo.irf1<-irf(modeloVAR,impulse="GGOV", response=NULL)
modelo.irf2<-irf(modeloVAR,impulse="INVP", response=NULL)
modelo.irf1
modelo.irf2

plot(modelo.irf1)
plot(modelo.irf2)
```
Se obtiene, 

``` r
> modelo.irf1<-irf(modeloVAR,impulse="GGOV", response=NULL,)
> modelo.irf2<-irf(modeloVAR,impulse="INVP", response=NULL,)
> 
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


Lower Band, CI= 0.95 
$GGOV
              GGOV        INVP
 [1,]  0.448404357 -0.89193356
 [2,] -0.105700990 -0.64052565
 [3,] -0.182059558  0.09303440
 [4,] -0.119333489  0.00629660
 [5,]  0.007679976 -0.24475296
 [6,] -0.014499518 -0.31440027
 [7,] -0.070333072 -0.17241123
 [8,] -0.058250246 -0.00514314
 [9,] -0.028107333 -0.06778566
[10,] -0.004810087 -0.11950295
[11,] -0.018304354 -0.09324359


Upper Band, CI= 0.95 
$GGOV
            GGOV         INVP
 [1,] 0.66616927 -0.173970521
 [2,] 0.14138199  0.254720600
 [3,] 0.08260326  0.711130678
 [4,] 0.05881065  0.501118647
 [5,] 0.14524152  0.153231778
 [6,] 0.09169154  0.007552665
 [7,] 0.04565639  0.143952626
 [8,] 0.01078695  0.215704615
 [9,] 0.03920627  0.137721706
[10,] 0.04261747  0.048914355
[11,] 0.02432740  0.036582845

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


Lower Band, CI= 0.95 
$INVP
               GGOV        INVP
 [1,]  0.0000000000  0.90181734
 [2,] -0.0659576129  0.19005463
 [3,]  0.0882737906 -0.65876327
 [4,] -0.0109366352 -0.63404399
 [5,] -0.1223958677 -0.11534597
 [6,] -0.1207293915  0.02108999
 [7,] -0.0220506125 -0.14628280
 [8,]  0.0008458852 -0.30946785
 [9,] -0.0364828488 -0.26282398
[10,] -0.0622521757 -0.04226737
[11,] -0.0541283572 -0.02302256


Upper Band, CI= 0.95 
$INVP
             GGOV         INVP
 [1,] 0.000000000 1.5624005651
 [2,] 0.228736119 0.7837656718
 [3,] 0.338436869 0.0008434544
 [4,] 0.188913794 0.0033266424
 [5,] 0.026016827 0.3677103850
 [6,] 0.018781874 0.4825373008
 [7,] 0.094339134 0.2524289668
 [8,] 0.094504547 0.0182056996
 [9,] 0.053452908 0.0852337713
[10,] 0.006974911 0.2135474677
[11,] 0.022293212 0.1640822381

> plot(modelo.irf1)
> plot(modelo.irf2)
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/dd9947bc-9f67-4f69-bb41-f077bddcafb3)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/fae26b8e-e4ee-4d7f-bca8-f782d448d149)

Las funciones impulso-respuesta revelan que:[^*]
* Un choque a la variación del gasto del gobierno (es decir, un choque en $\Delta GGOV$) afecta positivamente y de manera significativa a la variación en el gasto del gobierno en algunos de los primeros periodos después del choque: En los periodos $0$ y $4$ después del choque
* Un choque  a la variación del  gasto del gobierno tiene un efecto _crowding-out_ significativo sobre la variación de la inversión privada los primeros periodos, pero tiene un efecto _crowding-in_ significativo sobre ésta en periodos posteriores: Tiene un efecto negativo en el periodo $0$ y positivo en los periodos $2$ y $3$ después del choque.
* Un choque  a la variación de la  inversión privada (es decir, un choque en $\Delta INVP$) tiene un impacto positivo y significativo sobre la variación del gasto gobierno en algunos de los primeros periodos después del choque: En los periodos $2$ y $7$ después del choque
* Un choque a la variación de la  de inversión privada tiene un impacto positivo sobre la variación de la inversión en algunos de los primeros periodos después del choque: En los periodos $0$, $1$ y $4$ después del choque

[^*]: **Para las viñetas que siguen a continuación, fijense en las cotas superiores e inferiores de los intervalos de confianza de la función impulso-respuesta, si son del mismo signo, entonces son significativos en ese periodo con ese signo. Tenga cuidado porque en la salida de _R_, como número, de las funciones impulso-respuesta, tiene que restarle una unidad al número de la izquierda de cada fila para saber bien el periodo al que se hace referencia después del choque**. 

###  Análisis de la descomposición de varianza del error de pronóstico

Las funciones impulso-respuesta ya nos revelaron bastante acerca de la relación entre las variables $\Delta GGOV$ y $\Delta INVP$. Sin embargo, este tipo de análisis se suela acompañar con el análisis de la descomposición de varianza del error de pronóstico para entender mejor la fortaleza de esa relación. 

Para el análisis de descomposición de varianza 10 años hacia adelante, copiamos los comandos:
``` r
Desc_var<-fevd(modeloVAR, n.ahead=10)
Desc_var
```
Y obtenemos
``` r
> Desc_var<-fevd(modeloVAR, n.ahead=10)
> Desc_var
$GGOV
           GGOV        INVP
 [1,] 1.0000000 0.000000000
 [2,] 0.9905887 0.009411345
 [3,] 0.8697516 0.130248392
 [4,] 0.8548025 0.145197533
 [5,] 0.8511409 0.148859115
 [6,] 0.8473351 0.152664912
 [7,] 0.8464771 0.153522915
 [8,] 0.8441432 0.155856774
 [9,] 0.8441407 0.155859315
[10,] 0.8436112 0.156388777

$INVP
           GGOV      INVP
 [1,] 0.1210003 0.8789997
 [2,] 0.1203981 0.8796019
 [3,] 0.1734713 0.8265287
 [4,] 0.1872590 0.8127410
 [5,] 0.1891218 0.8108782
 [6,] 0.1927696 0.8072304
 [7,] 0.1927686 0.8072314
 [8,] 0.1942706 0.8057294
 [9,] 0.1943170 0.8056830
[10,] 0.1946133 0.8053867
```

Según la descomposición de varianza, más del 80% de la varianza del error de las variables $\Delta GGOV$ y $\Delta INVP$ son explicados por su propio comportamiento. Es decir, menos del 20% de la varianza del error de estas variables es explicado por el comportamiento de la otra variable. Por lo tanto, la relación etre estas dos variables, aunque existe, no es que sea demasiado fuerte.

## Pronosticos

Por último, alejandonos del ejercicio original vamos a hacer un pronóstico de 3 años hacia adelante de las variables $\Delta GGOV$ y $\Delta INVP$. Para ello, operamos el siguiente comando:
``` r
forecast3<-forecast(modeloVAR, level = c(95), h = 3)
forecast3
autoplot(forecast3)
```
Y obtenemos

``` r
GGOV
     Point Forecast     Lo 95    Hi 95
2023     0.15322791 -1.004413 1.310869
2024     0.11119942 -1.053103 1.275501
2025     0.02401758 -1.224606 1.272641

INVP
     Point Forecast     Lo 95    Hi 95
2023     0.04601959 -2.750839 2.842878
2024     0.07244331 -2.948292 3.093178
2025     0.08604130 -3.119753 3.291836
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/2f6d7218-d82c-45e1-85a5-f5950f23888c)

---
---
# Preguntas de selección múltiple

Uno de los acuerdos de libre comercio más importantes es el Tratado de Libre Comercio de América del Norte que firmaron Canadá, Estados Unidos y México el cual entró en vigencia en 1994 y tuvo un nuevo impulso en 2020. Este tratado fortaleció las relaciones económicas de los tres países. En consecuencia, a partir de un análisis $VAR$ basado en el comportamiento del PIB real de cada uno los tres países se va a analizar cómo es la interrelación entre estas tres economías. La variable que se va a utilizar para el análisis es el **PIB a dólares constantes de 2015 (variable NY.GDP.MKTP.KD en los _Indicadores de Desarrollo Mundial_)**

En primera instancia, vamos a limpiar el área de trabajo, llamar las librerías a utilizar y confirmar el nombre de la variable a utilizar 
``` r
rm(list = ls())

library(WDI)         # Esta librería sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta librería permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(tidyr)       # Esta librería permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos spread()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(forecast)    # Esta librería sirve para hacer pronósticos.
library(vars)        # Esta librería se utiliza para la estimación de los modelos VAR

WDIsearch(string='NY.GDP.MKTP.KD', field='indicator')
```
1. ¿Cuál de las siguientes variables no es reportada al copiar los comandos de arriba?

   a) NY.GDP.MKTP.KD: PIB a precios dólares constantes de 2015.

   b) NY.GDP.MKTP.KD.00: PIB a precios dólares constantes de 2000.

   c) NY.GDP.MKTP.KD.87: PIB a precios dólares constantes de 1987.

   d) NY.GDP.MKTP.KD.ZG: Crecimiento anual del PIB.

Con el siguiente comando se descarga la información, se renombra como $PIB$ y se construye una base de datos llamada "TLCAN":
``` r
TLCAN <- WDI(c(PIB =  "NY.GDP.MKTP.KD"), country = c('CA', 'US', 'MX'), start = 1960, end = 2022, language = "es")
TLCAN <-  mutate(TLCAN, iso2c=NULL, iso3c=NULL) # Se seleccionan solo las columnas necesarias
TLCAN <- TLCAN %>% arrange(country, year)
TLCAN_paises <- spread(TLCAN, key = country, value = PIB) # Se utiliza spread() de tidyr para convertir los datos a formato wide para tener una columna por país
TLCAN_paises <- na.omit(TLCAN_paises)  # Se eliminan las filas en las que todos los poises no tienen información
TLCAN_paises_matrix <- as.matrix(TLCAN_paises[, -1])  # Se convierte TLCAN_paises en una matriz
```
2. ¿Cuál es el PIB real (a dólares constantes de 2015) de los tres países en 1994?

   a) Canadá: 7.005270e+11, Estados Unidos: 7.260460e+12 y México: 5.670936e+11

   b) Canadá: 9.305451e+11, Estados Unidos: 1.084483e+13 y México: 7.229405e+11

   c) Canadá: 8.780163e+11, Estados Unidos: 9.811055e+12 y México: 6.206751e+11

   d) Canadá: 1.163710e+12, Estados Unidos: 1.375430e+13 y México: 8.764394e+11

Los datos del PIB de cada país se establecen como una serie de tiempo:
``` r
seriesVAR <- ts(TLCAN_paises_matrix, frequency = 1, start = 1981) # Se crean las variables en formato de serie de tiempo
```
No se les va a preguntar sobre el nivel de integración del PIB de los tres países. Sin embargo si hacen pruebas de raíz unitaria van a encontrar que el PIB de los tres países no es estacionario, pero su primera diferencia si lo es. 

Les recomiendo graficar el PIB real de los tres países en niveles y en primeras diferencias para darse una perspectiva de las variables que están manejando. Una forma de hacerlo es utilizando los siguientes comandos:

``` r
ggplot(TLCAN, aes(x = year, y = PIB, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País") +
  ggtitle("PIB para Canadá, Estados Unidos y México") +
  theme_minimal()

# Los siguientes comandos sirven para crear una base de datos (a la que se ha llamado _df_diff_) que incluya en la primera columna los años y en las siguientes la primera diferencia del PIB para cada uno de los países
first_diff_seriesVAR <- diff(seriesVAR, differences = 1)  # Con este comando se obtienen las primeras diferencias de cada una de las series
time_index <- time(first_diff_seriesVAR)  # Con este comando se crea la variable que representa los años en la nueva base de datos
df_diff <- data.frame(time_index, first_diff_seriesVAR)   # Con este comando se crea la base de datos df_diff
colnames(df_diff) <- c("Año", "Canadá", "Estados Unidos", "México")   # Con este comando se crean los titluos a la base de datos df_diff

ggplot(data = df_diff, aes(x = Año)) +
  geom_line(aes(y = Canadá, color = "Canadá")) +
  geom_line(aes(y = `Estados Unidos`, color = "Estados Unidos")) +
  geom_line(aes(y = México, color = "México")) +
  labs(x = "Año", y = "Primera Diferencia del PIB", color = "País") +
  ggtitle("Primera Diferencia del PIB para Canadá, Estados Unidos y México") +
  theme_minimal()

```
3. ¿Cuál es la primera diferencia del PIB real (a dólares constantes de 2015) de los tres países en 1994?

   a) Canadá: -22327630623, Estados Unidos: -130897915000 y México: -43004816

   b) Canadá: 40024162643, Estados Unidos: 419994983000 y México: 34039170809

   c) Canadá: 57286567365, Estados Unidos: 538816362000 y México: 41277489385

   d) Canadá: 1443474284, Estados Unidos: 181607993000 y México: 30727042305

Dado que al gráficar la variable $\Delta PIB$ de los tres países, esta pareciera no tener pendiente en ninguno de loa tres casos y potencialmente no tener intercepto, entonces el $VAR$ a estimar se escogera entre los modelos $VAR$ sin intercepto ni tendencia, y los modelos $VAR$ con intercepto, 

4. ¿Según los _Criterios de Información de Akaike_, los _Criterios de Información de Hannan-Quinn_ y los _Criterios Bayesianos de Schwartz_ cuál es el modelo $VAR$ más parsimonioso?

   a) Un modelo $VAR$ con un rezago y con intercepto

   b) Un modelo $VAR$ con dos rezagos y con intercepto

   c) Un modelo $VAR$ con un rezago y sin intercepto

   d) Un modelo $VAR$ con dos rezagos y sin intercepto
   
``` r
VARselect(diff(seriesVAR),lag.max=10,type="const")
VARselect(diff(seriesVAR),lag.max=10,type="none")
```

5. Estime el modelo $VAR$ más parsimonioso e identifique las tres raíces características del polinomio que lo resuelven. ¿Cuáles son estas raíces?

   a) 0.6170692, 0.2337025 y 0.2337025

   b) 0.9247295, 0.5116255 y 0.5116255

   c) 0.5660535, 0.5492523 y 0.5492523

   d) 0.2900364, 0.2900364 y 0.1871655

``` r
modeloVAR<-VAR(diff(seriesVAR),p=1,type="const")
summary(modeloVAR)
roots(modeloVAR)
```
A partir de las raíces características enontradas, observe que el $VAR$ es estable. 

6.Grafique la $FAC$ y la $FACP$ de los residuos y comente: ¿Más alla del rezago $0$, cuántas autocorrelaciones totales y parciales, respectivamente, estan por fuera de la banda de confianza en la $FAC$ y la $FACP$ de la ecuación estimada para los Estados Unidos?

   a) 0 y 0.

   b) 0 y 1.

   c) 1 y 0.

   d) 1 y 1.

```r
acf(residuals(modeloVAR)[,1])
pacf(residuals(modeloVAR)[,1])
acf(residuals(modeloVAR)[,2]) # esta es la FAC a la que hace referencia la pregunta
pacf(residuals(modeloVAR)[,2]) # esta es la FACP a la que hace referencia la pregunta
acf(residuals(modeloVAR)[,3])
pacf(residuals(modeloVAR)[,3])
```

7.Calcule el _p-value_ de la Prueba de Portmanteau y escoja la respuesta correcta: 

   a) 0.9414, entonces se rechaza la hipótesis nula de no autocorrelación de los residuos estimados.

   b) 0.9414, entonces se rechaza la hipótesis nula de autocorrelación de los residuos estimados.

   c) 0.9414, entonces no se rechaza la hipótesis nula de no autocorrelación de los residuos estimados.

   d) 0.9414, entonces no se rechaza la hipótesis nula de autocorrelación de los residuos estimados.

```r
serial.test(modeloVAR,lags.pt=10)
```

8. Calcule el _p-value_ de la Prueba de Portmanteau y escoja la respuesta correcta:

   a) 0.9414, entonces se rechaza la hipótesis nula de no autocorrelación de los residuos estimados.

   b) 0.9414, entonces se rechaza la hipótesis nula de autocorrelación de los residuos estimados.

   c) 0.9414, entonces no se rechaza la hipótesis nula de no autocorrelación de los residuos estimados.

   d) 0.9414, entonces no se rechaza la hipótesis nula de autocorrelación de los residuos estimados.

9. ¿Según la prueba de causalidad de Granger al 10% de significancia, de los países considerados, el $\Delta PIB$ de cuál país afecta el  $\Delta PIB$ de otro país?:
    
   a) El de Estados Unidos afecta al de México.

   b) El de Estados Unidos afecta al de Canadá.

   c) El de Canadá afecta al de Estados Unidos.

   d) El de México afecta al de Estados Unidos.

```r
grangertest(diff(Canadá) ~ diff(`Estados Unidos`), order = 1, data=seriesVAR)
grangertest(diff(Canadá) ~ diff(México), order = 1, data=seriesVAR)
grangertest(diff(`Estados Unidos`) ~ diff(Canadá), order = 1, data=seriesVAR)
grangertest(diff(`Estados Unidos`) ~ diff(México), order = 1, data=seriesVAR)
grangertest(diff(México) ~ diff(Canadá), order = 1, data=seriesVAR)
grangertest(diff(México) ~ diff(`Estados Unidos`), order = 1, data=seriesVAR)
```

| [Subsección: 3.1. Estimación de Modelos _VAR_](../Readme.md) |
|--------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>




