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
