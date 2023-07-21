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
dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS", INVP="NE.GDI.FPRV.ZS"), country=c('KR'), language = "es")
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
|Estadistico                                          | Número de Rezagos del <br> Modelo más Parsimonioso  | Valor       | 
|-----------------------------------------------------|:---------------------------------------------------:|:-----------:|
|$\text{Criterio de Información de Akaike}$: AIC      |2 (con constante)                                    | -0.7302187  | 
|$\text{Criterio de Información de Hannan-Quinn}$: HQ |1 (con constante)                                    | -0.6392294  | 
|$\text{Criterio Bayesiano de Schwartz}$: SC          |1 (con constante)                                    | -0.4819802  | 


Es decir, el modelo $VAR$ tiene uno o dos rezagos (según el criterio de información escogido) y lleva constante. No hay una opción clara acerca de si es mejor con uno o con dos rezagos. Sin embargo, con una brecha relativamente más alta (respecto al resto de criterios de información), el  $\text{Criterio Bayesiano de Schwartz}$ concluye que es mejor un rezago, entonces vamos a seguir dicho criterio.

Ahora estimamos el $VAR(1)$ con los siguientes comandos:
``` r
modeloVAR<-VAR(diff(seriesVAR),p=1,type="const")
summary(modeloVAR)
```
Obteniendo
``` r
> summary(modeloVAR)

VAR Estimation Results:
========================= 
Endogenous variables: GGOV, INVP 
Deterministic variables: const 
Sample size: 51 
Log Likelihood: -133.731 
Roots of the characteristic polynomial:
0.2432 0.2432
Call:
VAR(y = diff(seriesVAR), p = 1, type = "const")


Estimation results for equation GGOV: 
===================================== 
GGOV = GGOV.l1 + INVP.l1 + const 

        Estimate Std. Error t value Pr(>|t|)
GGOV.l1  0.12919    0.15930   0.811    0.421
INVP.l1  0.07382    0.05681   1.299    0.200
const    0.13676    0.09038   1.513    0.137


Residual standard error: 0.6087 on 48 degrees of freedom
Multiple R-Squared: 0.03524,	Adjusted R-squared: -0.004955 
F-statistic: 0.8767 on 2 and 48 DF,  p-value: 0.4227 


Estimation results for equation INVP: 
===================================== 
INVP = GGOV.l1 + INVP.l1 + const 

        Estimate Std. Error t value Pr(>|t|)
GGOV.l1  -0.4960     0.4245  -1.169    0.248
INVP.l1   0.1745     0.1514   1.153    0.255
const     0.2419     0.2409   1.004    0.320


Residual standard error: 1.622 on 48 degrees of freedom
Multiple R-Squared: 0.09242,	Adjusted R-squared: 0.05461 
F-statistic: 2.444 on 2 and 48 DF,  p-value: 0.09754 



Covariance matrix of residuals:
        GGOV    INVP
GGOV  0.3706 -0.4917
INVP -0.4917  2.6315

Correlation matrix of residuals:
       GGOV   INVP
GGOV  1.000 -0.498
INVP -0.498  1.000
```

Observe que en este VAR las dos raíces caracteristicas del polinomio que lo resuelven son iguales 0.2432. Por lo tanto, se encuentran dentro del circulo unitario, lo que nos lleva a que el $VAR$ es estable. No olvide que las raíces caracteristicas, después de estimado el $VAR$ también se pueden recuperar con el comando

```r
roots(modeloVAR)
```
Obteniendo, 

```r
> roots(modeloVAR)
[1] 0.243234 0.243234

```
A partir de los coeficientes del $VAR$ parece como se hubiera un efecto crowding-out y al mismo tiempo la inversión tuviera un impacto positivo sobre el gasto del gobierno.

#FALTA LO DE ABAJO
plot(modeloVAR, names="GGOV")
plot(modelo, names="INVP")

acf(residuals(modelo)[,1])

serial.test(modelo,lags.pt=10)

grangertest(diff(GGOV) ~ diff(INVP),order=1,data=series)

grangertest(diff(INVP) ~ diff(GGOV),order=1,data=series)

modelo.irf1<-irf(modelo,impulse="GGOV", response="GGOV")
modelo.irf2<-irf(modelo,impulse="GGOV", response="INVP")
modelo.irf3<-irf(modelo,impulse="INVP", response="GGOV")
modelo.irf4<-irf(modelo,impulse="INVP", response="INVP")


plot(modelo.irf1)
plot(modelo.irf2)
plot(modelo.irf3)
plot(modelo.irf4)

forecast1<-forecast(modelo, level = c(95), h = 3)
summary(forecast1)
autoplot(forecast1)

forecasts <- predict(modelo)
forecasts
plot(forecasts)



