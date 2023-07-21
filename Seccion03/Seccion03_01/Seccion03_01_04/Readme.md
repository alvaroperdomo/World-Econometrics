## SECCIÓN 3.1.4
# Ejemplo de un $VAR$ utilizando la base de datos _Indicadores de Desarrollo Mundial_

A continuación, se presenta un ejemplo utilizando la base de datos _Indicadores de Desarrollo Económico del Banco Mundial_. 

Después de la Segunda Guerra Mundial, uno de los países exitosos en términos de desarrollo y crecimiento económico fue Corea del Sur. Por otra parte uno de los debates que se tienen en economía esta relacionado con el efecto que ejerce el  el gasto del gobierno (o la inversión del gobieno o la deuda del gobierno) sobre la inversión privada. Por un lado, están quienes establecen que se da un efecto desplazamiento (o efecto _crowding-out_) en donde el mayor gasto del gobierno, presiona a un incremento de la tasa de interés, perjudicando de esta forma a los proyectos privados de inversión. Por otro lado, están quienes afirman que un mayor gasto del gobierno reactiva la demanda, lo que hace que aumente la producción (es decir, la oferta) y por esta vía se afectan positivamente los proyectos de inversión. 

Igualmente, hay que tener en cuenta que una mayor inversión privada reactiva la economía, y por esta vía el gobierno recauda más impuestos y puede aumentar su gasto. Dado el tipo de trelación de doble vía que hemos establecido, una estimación de Mínimos Cuadrados en este contexto no sería recomendable por la potencial presencia se estimaciones sesgadas. Una de las formas de resolver este tipo de dilemas en con la utilización de estimaciones $VAR$

A continuación, se propone analizar la relación econométrica entre el gasto del gobierno y la inversión privada en Corea del Sur.

En primera instancia, vamos a limpiar el área de trabajo y a llamar las librerias a utilizar. 

``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(vars)
library(MTS)
```
Las dos variables que van a representar el gasto público y la inversión privada se van a manejar como % del PIB y son definidas en los _Indicadores de Desarrollo Mundial_ como:
``` r
> WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')
           indicator                                                        name
11014 NE.CON.GOVT.ZS General government final consumption expenditure (% of GDP)
> WDIsearch(string='NE.GDI.FPRV.ZS', field='indicator')
           indicator                                                     name
11117 NE.GDI.FPRV.ZS Gross fixed capital formation, private sector (% of GDP)
```
Llamamos los datos de las dos variables para Corea del Sur, y las denominamos como GGOV (Gasto Público como % del PIB) y INVP (Inversión Privada como % del PIB)
``` r
dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS", INVP="NE.GDI.FPRV.ZS"), country=c('KR'), language = "es")
```
Al visualizar los datos, se puede ver que ya estan ordenados del más antiguo al más nuevo

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/94a117f9-c3b7-41e1-a6ca-495f07af6712)

Entonces, con los siguientes comandos a procedemos depurar la base de datos: (1) eliminando los años en los cuales no tenemos información para ambas variables (es decir, de 1960 a 1969) y (2) eliminando las columnas que no incluyan a las dos variables análizadas:
``` r
dat <- na.omit(dat)
dat <- mutate(dat, year=NULL, country=NULL, iso2c=NULL, iso3c=NULL)
```
Ahora la base esta así:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/a4e1a02a-fc5c-4aba-ad82-1f8f6cbf43d3)

Ahora se determina que las dos variables conforman un vector en formato de serie de tiempo anual (llamado seriesVAR) que va desde 1970 hasta 2022
``` r
seriesVAR <-ts(dat,frequency=1,start = 1970)
```
Con el siguiente comando, visualizamos las dos variables:
``` r
ts.plot(seriesVAR, xlab="Años", ylab="% del PIB", col=c("red","blue"), main="Corea del Sur - Series en niveles: 1970 - 2022")
legend("topleft", legend = c("GGOV","INVP"), col = c("red","blue"), lty = 1)
```
Obteniendo

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/022ef39c-d4ea-4f83-aa14-49f4c5afbccd)

En un análisis previo, que no se muestra en esta sección, se concluye que ambas series (GGOV e INVP) son integradas de orden uno. Es decir, en niveles no son estacionarias en niveles, pero si en primeras diferencias. Por lo que el modelo VAR que se va a estimar es con las variables en primeras diverencias. Antes de comenzar el análisis VAR, gráfiquemos las variables a estudiar en primeras diferencias
``` r
ts.plot(diff(seriesVAR), xlab="Años",ylab="% del PIB",col=c("red","blue"), main="Corea del Sur - Series en diferencias: 1970 - 2022")
legend("topright", legend = c("dif.GGOV","dif.INVP"), col = c("red","blue"), lty = 1)
```
Obteniendo

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/da96898b-e37e-4709-8ac5-5a34c51b0e48)
