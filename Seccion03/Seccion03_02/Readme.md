## SECCIÓN 3.2
# Cointegración y Modelos de Vectores de Corrección de Errores (_VEC_)

A continuación, dando click en la X de la segunda columna de la siguiente tabla se redirigira a la explicación de cada uno de los aspectos teóricos que se encuentran en la primera columna de la tabla. Por otra parte, dando click en la X de la tercera columna de la tabla podra ver las aplicaciones en R de cada uno de estos temas:

| Temas                                                                                               | Explicación teórica                         |  Aplicación en R   |
|-----------------------------------------------------------------------------------------------------|:-------------------------------------------:|:------------------:|
|  **3.2.1.** ¿Qué es y para qué sirve la Cointegración?                                              |  [X](Seccion03_02_01_T/Readme.md)                     | 
|  **3.2.2.** La Metodología de Engle y Granger (**2 variables**): <br> ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$? |  [X](Seccion03_02_02_T/Readme.md)     | [X](Seccion02_02_ZA_R/Readme.md)     |
| **3.2.3.** La Metodología de Johansen (**2 o más variables**): <br> ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$?   |  [X](Seccion03_02_03_T/Readme.md) | [X](Seccion03_02_03_R/Readme.md) |
| **3.2.4.** Pruebas de hipótesis adicionales para implementar en un modelo $VEC$                     |  [X](Seccion03_02_04_T/Readme.md)           | [X](Seccion02_02_ADFGLS_R/Readme.md) |


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

Johansen <- ca.jo(series, type = c("eigen"), ecdet = c("none"), K = 2)
summary(Johansen)
cajorls(Johansen, r = 1, reg.number = NULL)

```

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
