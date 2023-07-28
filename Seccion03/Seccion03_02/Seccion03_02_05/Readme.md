## SUBSECCIÓN 3.2.5
# Caso de estudio de un modelo _VEC_ utilizando la base de datos _Indicadores de Desarrollo Mundial_

Los países que tienen un pasado común XXXXX

Vamos a preparar la base de datos en $R$:
``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(vars)
library(tidyr) # esta librería es útil para manipular los datos, por ejemplo con el comando "spread"
library(ggplot2)

WDIsearch(string='NY.GDP.PCAP.KD', field='indicator') # Confirmamos el nombre de la variable que se va a analizar

# Se obtienen los datos de PIBpc para Chile, Colombia y México desde 1960 hasta 2021, y se ordenan
dat <- WDI(indicator = c(PIBpc = "NY.GDP.PCAP.KD"), country = c('CL', 'CO', 'MX'), start = 1960, end = 2021)
dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat <-  mutate(dat, iso2c=NULL, iso3c=NULL) # Se anulan las columnas necesarias

PIBpc_paises <- spread(dat, key = country, value = PIBpc) # Se convierten los datos a formato wide para tener una columna por país

seriesVEC <- ts(PIBpc_paises[, -1], frequency = 1, start = 1960) # Se crea la serie de tiempo
```

Un gráfico de los datos se puede obtener con el siguiente comando
``` r
ggplot(dat, aes(x = year, y = PIBpc, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País") +
  ggtitle("PIB per cápita para Chile, Colombia y México") +
  theme_minimal()
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/2f9cca6d-266f-4df7-b883-057406f658ef)

VARselect(diff(seriesVEC),lag.max=10,type="const")

Johansen <- ca.jo(seriesVEC, type = c("eigen"), ecdet = c("none"), K = 2)
summary(Johansen)
cajorls(Johansen, r = 1, reg.number = NULL)
