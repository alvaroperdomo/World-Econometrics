## SECCIÓN 3:

# Análisis Multivariado de Series de Tiempo
Los modelos de series de tiempo univariados pueden ser muy útiles para el análisis descriptivo y para el pronóstico fuera de muestra. Sin embargo, tales modelos univariados son restrictivos, en el sentido de que se basan únicamente en la historia de las series de tiempo y no intentan explotar la información presente en otras variables. 

Dado que a menudo las variables económicas están estrechamente relacionadas con otras variables, dicha información puede ser potencialmente útil y su uso puede llevar a descripciones más realistas del comportamiento de las variables económicas y también a pronósticos más precisos fuera de la muestra. 

De hecho, ignorar las relaciones con otras variables puede dar lugar a complicaciones en la especificación de un modelo apropiado de series de tiempo univariadas. Puede ser que la especificación empírica de un modelo univariado se vea obstaculizada por los valores atípicos y los cambios estructurales, que pueden atribuirse a una o más variables distintas de la variable $y_t$ en cuestión.

Para acceder a cada una de las subsecciones haga _click_ en la tabla de abajo en la subsección respectiva

| Subsecciones                                                                 | Contenido                                                                           | Dedicación,<br> 5.5 horas       | 
|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------|:-------------------------:|
| [**3.1.** Estimación de Modelos VAR](Seccion03_01/Readme.md)                 | Explicación acerca de la metododología para estimar un modelo VAR.                  |              1            | 
| [**3.2.** Cointegración y Estimación de Modelos VEC](Seccion03_02/Readme.md) | Explicación acerca de lo qué es la cointegración y acerca de la metododología para estimar un modelo VEC. |              1            | 

``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)
library(vars)
library(LINselect)
library(MTS)

WDIsearch(string='NY.GDP.PCAP.KD', field='indicator')

dat = WDI(indicator= c(PIBpc = "NY.GDP.PCAP.KD"), country=c('CL', 'CO', 'MX'), language = "es")
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat_CL <- dat[dat$iso2c == 'CL',]
dat_CO <- dat[dat$iso2c == 'CO',]
dat_MX <- dat[dat$iso2c == 'MX',]

dat_CL <- mutate(dat_CL, Chile = PIBpc, PIBpc=NULL, country=NULL, iso2c=NULL, iso3c=NULL)
dat_CO <- mutate(dat_CO, Colombia = PIBpc, PIBpc=NULL, country=NULL, iso2c=NULL, iso3c=NULL)
dat_MX <- mutate(dat_MX, Mexico = PIBpc, PIBpc=NULL, country=NULL, iso2c=NULL, iso3c=NULL)

BT1 <- merge(dat_CL,dat_CO,all = TRUE)
PIBpc_paises <- merge(BT1,dat_MX, year=NULL)
PIBpc_paises <- PIBpc_paises[ -c(1) ]

series <-ts(PIBpc_paises,frequency=1,start = 1960)
# plot(series)

VARorder(series, maxp = 13, output = T)

modelo <- VAR(series, p = 1, type = c("const"), season = NULL, exogen = NULL, lag.max = NULL, ic = c("AIC"))
modelo
summary(modelo, equation="Chile")
summary(modelo, equation="Colombia")
summary(modelo, equation="Mexico")
```

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [:house: Inicio](../README.md)    | [Siguiente Sección: 3.1. Estimación de Modelos VAR](Seccion03_01/Readme.md) |
|-----------------------------------|-----------------------------------------------------------------------------|
