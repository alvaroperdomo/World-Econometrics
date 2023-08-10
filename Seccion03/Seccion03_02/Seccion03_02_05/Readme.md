<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.5
# Caso de estudio de un modelo _VEC_ utilizando la base de datos _Indicadores de Desarrollo Mundial_

Los países que pertenecen a una misma región pueden verse influenciados por choques y tendencias similares. En consecuencia, se han escogido tres países de Latinoamérica (**Brasil, Colombia y México**) para determinar si entre el PIB de estos países existe una relación de largo plazo. La medida del PIB utilizada para hacer el análisis es el **PIB per cápita a dólares constantes de 2015**.

Vamos a preparar la base de datos en $R$:
``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(vars)
library(tidyr) 

WDIsearch(string='NY.GDP.PCAP.KD', field='indicator') # Se confirma el nombre de la variable que se va a analizar

# Obtenemos los datos de PIBpc para Brasil, Colombia y México desde 1960 hasta 2021 y se convierten en formato de serie de tiempo
dat <- WDI(indicator = c(PIBpc = "NY.GDP.PCAP.KD"), country = c('BR', 'CO', 'MX'), start = 1960, end = 2021)
dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat <-  mutate(dat, iso2c=NULL, iso3c=NULL) # Se renombran las columnas y se seleccionan solo las columnas necesarias
PIBpc_paises <- spread(dat, key = country, value = PIBpc) # Se utiliza spread() de tidyr para convertir los datos a formato wide para tener una columna por país
PIBpc_paises_matrix <- as.matrix(PIBpc_paises[, -1])  # Se convierte PIBpc_paises en una matriz
seriesVEC <- ts(PIBpc_paises_matrix, frequency = 1, start = 1960) # Se crean las variables en formato de serie de tiempo
```

Un gráfico de los datos se puede obtener con el siguiente comando
``` r
ggplot(dat, aes(x = year, y = PIBpc, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País") +
  ggtitle("PIB per cápita para Brasil, Colombia y México") +
  theme_minimal()
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/b26b3b4b-3105-4b69-8b28-4eef28b7ab83)

Previamente, aunque no se muestra en esta sección, se hicieron pruebas de raíz unitaria a la variable PIB_pc de los tres países considerados y se obtuvó que en todos los casos era I(1), entonces, ahora se va a hacer la prueba de cointegración de Johansen para ver si existe una relación de cointegración entre los mismos.

Para el análisis con el estadístico de la traza se utilizan los siguientes comandos de $R$:
``` r
selected_order <- VARselect(seriesVEC, lag.max = 10, type = "const") # Se selecciona automáticamente el número de rezagos utilizando el criterio AIC
selected_order

```

``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(vars)
library(tidyr) 

WDIsearch(string='NY.GDP.PCAP.KD', field='indicator') # Se confirma el nombre de la variable que se va a analizar

# Obtenemos los datos de PIBpc para Brasil, Colombia y México desde 1960 hasta 2021 y se convierten en formato de serie de tiempo
dat <- WDI(indicator = c(PIBpc = "NY.GDP.PCAP.KD"), country = c('BR', 'CO', 'MX'), start = 1960, end = 2021)
dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat <-  mutate(dat, iso2c=NULL, iso3c=NULL) # Se renombran las columnas y seleccionamos solo las columnas necesarias
PIBpc_paises <- spread(dat, key = country, value = PIBpc) # Utilizamos spread() de tidyr para convertir los datos a formato wide para tener una columna por país
PIBpc_paises_matrix <- as.matrix(PIBpc_paises[, -1])  # Se convierte PIBpc_paises en una matriz
seriesVEC <- ts(PIBpc_paises_matrix, frequency = 1, start = 1960) se crea la serie de tiempo

# Se grafican los datos
ggplot(dat, aes(x = year, y = PIBpc, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País") +
  ggtitle("PIB per cápita para Brasil, Colombia y México") +
  theme_minimal()

# Selecciona automáticamente el número de rezagos utilizando el criterio AIC
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

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
