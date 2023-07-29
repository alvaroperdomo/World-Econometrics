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

Previamente, aunque no se muestra en esta sección, se hicieron pruebas de raíz unitaria a la variable PIB_pc de los tres países considerados y se obtuvó que en todos los casos era I(1), entonces, ahora se va a hacer la prueba de cointegración de Johansen para ver si existe una relación de cointegración entre los mismos.

Para el análisis con el estadístico de la traza se utilizan los siguientes comandos de $R$:

``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(vars)
library(tidyr) 

# Obtenemos los datos de PIBpc para Chile, Colombia y México desde 1960 hasta 2021
dat <- WDI(indicator = c(PIBpc = "NY.GDP.PCAP.KD"), country = c('CL', 'CO', 'MX'), start = 1960, end = 2021)
dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

# Renombramos las columnas y seleccionamos solo las columnas necesarias
dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
WDIsearch(string='NY.GDP.PCAP.KD', field='indicator') # Confirmamos el nombre de la variable que se va a analizar

ggplot(dat, aes(x = year, y = PIBpc, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País") +
  ggtitle("PIB per cápita para Chile, Colombia y México") +
  theme_minimal()

# Convertimos los datos a formato wide para tener una columna por país
PIBpc_paises <- spread(dat, key = country, value = PIBpc) # Utilizamos spread() de tidyr

# Convertimos PIBpc_paises a una matriz
PIBpc_paises_matrix <- as.matrix(PIBpc_paises[, -1])

# Creamos la serie de tiempo
seriesVEC <- ts(PIBpc_paises_matrix, frequency = 1, start = 1960)

# Seleccionar automáticamente el número de rezagos utilizando el criterio AIC
selected_order <- VARselect(seriesVEC, lag.max = 10, type = "const")

selected_order

# Usar el valor óptimo de "K" en la función ca.jo()
Johansen_traza_none <- ca.jo(seriesVEC, type = "eigen", ecdet = "none", K = 2)
Johansen_traza_const <- ca.jo(seriesVEC, type = "eigen", ecdet = "const", K = 2)
Johansen_traza_trend <- ca.jo(seriesVEC, type = "eigen", ecdet = "trend", K = 2)

summary(Johansen_traza_none)
summary(Johansen_traza_const)
summary(Johansen_traza_trend)

Johansen_eigen_none <- ca.jo(seriesVEC, type = "trace", ecdet = "none", K = 2)
Johansen_eigen_const <- ca.jo(seriesVEC, type = "trace", ecdet = "const", K = 2)
Johansen_eigen_trend <- ca.jo(seriesVEC, type = "trace", ecdet = "trend", K = 2)

summary(Johansen_eigen_none)
summary(Johansen_eigen_const)
summary(Johansen_eigen_trend)

cajorls(x, r = 1, reg.number = NULL)
``` 
| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |
|-------------------------------------------------------------------------------|
