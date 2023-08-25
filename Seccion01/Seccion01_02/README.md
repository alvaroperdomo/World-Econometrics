<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 1.2: 
# $R$ y los _Indicadores de Desarrollo Mundial_
>"_Los Indicadores del Desarrollo Mundial son la principal colección de estadísticas internacionales sobre desarrollo que el Banco Mundial recopila de fuentes reconocidas oficialmente e incluyen estadísticas a nivel nacional, regional y mundial. Proporcionan acceso a casi 1600 indicadores para 217 economías, y algunas de las series cronológicas se remontan a más de 50 años. En la base de datos, los usuarios —analistas, encargados de formular políticas, académicos y todas las personas interesadas en la situación del mundo— pueden encontrar información relacionada con todos los aspectos del desarrollo, tanto actuales como históricos._" ([Banco Mundial](https://blogs.worldbank.org/es/opendata/guia-en-linea-para-los-indicadores-del-desarrollo-mundial-una-nueva-manera-de-encontrar-datos-sobre-el-desarrollo#:~:text=Los%20Indicadores%20del%20Desarrollo%20Mundial%20(WDI)%20son%20la%20principal%20colecci%C3%B3n,nivel%20nacional%2C%20regional%20y%20mundial.)) 

La Base de Datos con los _Indicadores de Desarrollo Mundial_ pueden consultarse en https://datos.bancomundial.org/ o en https://databank.worldbank.org/source/world-development-indicators.
Si se desea tener un acceso rápido a la misma utilizando $R$ hay que descargar la librería **WDI** (en inglés _World Development Indicators_) escribiendo el siguiente comando en $RStudio$:[^1]

``` r
install.packages('WDI')
```

[^1]:**La librería **WDI** permite descargar información de más de 40 bases de datos alojadas por el Banco Mundial, incluidos los Indicadores de Desarrollo Mundial, estadísticas de deuda internacional, la base Doing Business, el índice de capital humano y el índice de pobreza subnacional, entre otras.**


## Buscando los datos
A continuación se presenta un ejemplo didáctico que le sugerirá ideas acerca de cómo buscar información en la base de datos _Indicadores de Desarrollo Mundial_:.

Asuma que desea buscar los datos del Producto Interno Bruto per cápita por paridad del poder adquisitivo "PPA" a precios constantes de un país, de un grupo de países o de todos los países. Para ello, primero tiene que identificar el nombre exacto de la variable que desea descargar de la base de datos. Se le proponen dos opciones de búsqueda:

1) **Utilizando internet:**

   * Diríjase a la dirección: https://datos.bancomundial.org/indicator
   * Busque la variable deseada: Si utiliza el buscador al comienzo de la página, sea específico.
   
     Por ejemplo,
     
     * si solo coloca PIB (ver figura de abajo) puede que ninguna de las opciones a escoger incluyan la variable requerida.
       
      ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/888b9892-66a4-4ac1-8e99-0dcb0395a497)

     * Si coloca PIB per cápita  PPA,  (ver figura de abajo) resulta más fácil que se le sugiera la variable requerida
   
      ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/fb4214ee-4704-40f6-96e3-69bd90f76bd1)

     * Escoja con el _mouse_ la variable en cuestión "PIB per cápita, PPA ($ a precios internacionales constantes de 2011)" y será redirigido a la página de Internet donde se encuentra la misma [^2]

     * Escoja con el _mouse_ la opción ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d615bae3-bd1b-46e8-99cd-42f3566ef4f0) que se encuentra en la esquina superior derecha de la figura, y se desplegará la siguiente información
       
     ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4fd84b83-79f2-4d78-a721-1af9d3ef69ea)
   
   * En la parte inferior del desplegable se encuentra el código de la variable: **NY.GDP.PCAP.PP.KD**

[^2]: **No se preocupen demasiado por el año base reportado en el nombre de la variable. Noten que cuando ustedes escogen en esta nueva página de Internet que la quieren ver en inglés, el año va a cambiar a 2017, pero los datos siguen siendo los mismos**

2) **Utilizando $RStudio$:**

#### Dentro del comando _WDIsearch_ coloque, separados con los dos signos .*, palabras (en inglés) relacionadas con la variable que esta buscando. 

Por ejemplo, dado que le interesa el Producto Interno Bruto per cápita por paridad del poder adquisitivo "PPA" a precios constantes, utilice las palabras gdp, capita y constant (en español, PIB, per cápita y constante):

``` r
WDIsearch('gdp.*capita.*constant')
```
Obteniendo:
``` r
> WDIsearch('gdp.*capita.*constant')
                 indicator                                                 name
[1,]     6.0.GDPpc_constant GDP per capita, PPP (constant 2011 international $) 
[2,]       NY.GDP.PCAP.KD                   GDP per capita (constant 2015 US$)
[3,]       NY.GDP.PCAP.KN                        GDP per capita (constant LCU)
[4,]    NY.GDP.PCAP.PP.KD  GDP per capita, PPP (constant 2017 international $)
[5,] NY.GDP.PCAP.PP.KD.87  GDP per capita, PPP (constant 1987 international $)
```
Por lo tanto, el código de la variable solicitada es **NY.GDP.PCAP.PP.KD** (observen que se llegó a la misma variable que con la primera opción de búsqueda). Existe también la opción de tener la variable a precios constantes de 1987 o de 2011, pero generalmente las personas prefieren utilizar la variable con los precios constantes más recientes (es decir, 2017) 

Observe que si solo copia:

``` r
WDIsearch('gdp')
```

Obtendrá como respuesta todas las variables que tienen dentro de su definición (name) la palabra 'gdp'. El problema con esta forma de visualizar los datos es que muchas variables incluyen "gdp" dentro de su nombre, por lo tanto la visualización de la información es demasiado engorrosa. Una forma más sencilla de ver la información es solo visualizando un determinado número de variables de la lista, por ejemplo si solamente se desea visualizar las 10 primeras variables de la lista, utilice el comando:

``` r
WDIsearch('gdp')[1:10,]
```
Y obtiene:

``` r

> WDIsearch('gdp')[1:10,]
                indicator                                                 name
[1,]        5.51.01.10.gdp                                Per capita GDP growth
[2,]       6.0.GDP_current                                      GDP (current $)
[3,]        6.0.GDP_growth                                GDP growth (annual %)
[4,]           6.0.GDP_usd                                GDP (constant 2005 $)
[5,]    6.0.GDPpc_constant GDP per capita, PPP (constant 2011 international $) 
[6,]     BG.GSR.NFSV.GD.ZS                         Trade in services (% of GDP)
[7,]  BG.KAC.FNEI.GD.PP.ZS          Gross private capital flows (% of GDP, PPP)
[8,]     BG.KAC.FNEI.GD.ZS               Gross private capital flows (% of GDP)
[9,]  BG.KLT.DINV.GD.PP.ZS      Gross foreign direct investment (% of GDP, PPP)
[10,]    BG.KLT.DINV.GD.ZS           Gross foreign direct investment (% of GDP)
```

Lamentablemente, observe que con esta forma de búsqueda dentro de las 10 primeras variables no se encuentra la variable requerida. 

A partir de la instrucción 

``` r
?WDIsearch
```
o de la instrucción

``` r
help WDIsearch
```
puede obtener más información acerca de como utilizar este comando el comando **WDIsearch**.    

## Descargando y utilizando los datos

Descargue la serie que desee para los países requeridos utilizando el comando **WDI()**.

WDI tiene el siguiente formato: **WDI(country = "xxx", indicator = "xxx", start = xxx, end = xxx, extra = xxx, cache = xxx, latest = xxx, language = "xxx")***

| **Argumentos**          | **Descripción**                                                                                                                            | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| **country**             | Esta opción le permite descargar la información para los países (identificados por códigos de caracteres [ISO-2](https://www.datosmundial.com/codigos-de-pais.php) por ejemplo "BR", "US", "CA") para los que se necesitan los datos. El uso de la opción **"all"** en lugar de los códigos ISO individuales extrae datos para cada país disponible.              |
| **indicator**           | Esta opción le permite escoger las variables que se desea descargar. Recuerde que el nombre de las variables se puede obtener utilizando el comando **WDIsearch()**. Si al descargar una variable le coloca un nombre, los indicadores se renombrarán automáticamente, por ejemplo: 'c('mujeres_sector_privado' = 'BI.PWK.PRVS.FE.ZS')'                         |
| **start**               | Fecha de inicio de la información que desea descargar, normalmente es un año en formato de número entero. Debe ser de 1960 o superior.     |
| **end**                 | Fecha de finalización de la información que desea descargar, normalmente es un año en formato de número entero. _Debe ser mayor que la fecha de 'start'_. Si escoge la opción **'NULL'**, la fecha de finalización se establece 5 más adelante de lo que escogió en la opción **start**.                                                                            | 
| **extra**               | Si escoge **TRUE**, entonces obtiene variables extra. Por ejemplo: region, iso3c code, incomeLevel.                                        |
| **cache**               | Si escoge **NULL** (opcional) una lista creada por **WDIcache()** es utilizada para complementar el argumento **extra=TRUE**.              |
| **latest**              | Al escoger un número entero se le señala el número de valores no-NA más recientes que se obtendrán. El valor predeterminado es **NULL**. Si se especifica, anula las fechas de inicio y finalización.                                                                                                                                                          |
| **language**            | Al copiar el código ISO-2 de un idioma en minúsculas indica el idioma en que se deben proporcionar los caracteres. La lista de idiomas disponibles se puede consultar copiando en R el comando **WDI::languages_supported()**. **El valor predeterminado es inglés**.                                                                                                 |

_Es factible solo especificar los argumentos **'indicador'** y **'country**, en cuyo caso **WDI()** devolverá datos desde 1960 hasta el último año disponible en el sitio web del Banco Mundial. También es posible obtener solo los valores no-NA más recientes, con **'latest'**._

Por ejemplo, vamos a descargar el PIB PPA a dólares constantes de 2017 y la población de México, Canadá y los Estados Unidos en una base de datos llamada "dat", la cual continuaremos manejando en el resto de la sección:

``` r
dat = WDI(indicator= c("PIB_per_cápita_PPA_2017US" = "NY.GDP.PCAP.PP.KD", "poblacion" = "SP.POP.TOTL"), country=c('MX','CA','US'), start=1960, end=2012, language = "es")
```

### Visualice los datos

Note que en la pantalla _Environment_ de $Rstudio$ aparece la base "dat" (ver figura de abajo) cuando esta ya se ha construido

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/6d3547d6-7a8e-40ee-9ffb-c76d5e31e543)

Si le da _click_ a "dat" con el _mouse_ podrá ver los datos en una ventana $RStudio$ (ver figura de abajo), otra opción es copiando el comando **View()**:
``` r
View(dat)
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/0b7a37c1-1133-4dda-b301-47714fd5161a)

Igualmente, puede visualizar los primeros seis datos de la base de datos utilizando el comando **head()**:
``` r
head(dat)
```

Y obtiene:
``` r
  country iso2c iso3c year PIB_per_cápita_PPA_2017US poblacion
1  Canadá    CA   CAN 1960                        NA  17909356
2  Canadá    CA   CAN 1961                        NA  18271000
3  Canadá    CA   CAN 1962                        NA  18614000
4  Canadá    CA   CAN 1963                        NA  18964000
5  Canadá    CA   CAN 1964                        NA  19325000
6  Canadá    CA   CAN 1965                        NA  19678000
```

Existen varios paquetes en $R$ para graficar los datos, a continuación utilizamos uno de ellos:
``` r
library(ggplot2)
# Como los datos de la variable "PIB_per_cápita_PPA_2017US" sólo están disponibles desde 1990 hasta 2012, entonces vamos construirla base de datos "dat2" que solo abarca ese periodo de tiempo para hacer el gráfico de forma ágil
dat2 = WDI(indicator= c("PIB_per_cápita_PPA_2017US" = "NY.GDP.PCAP.PP.KD", "poblacion" = "SP.POP.TOTL"), country=c('MX','CA','US'), start=1990, end=2012, language = "es")
ggplot(dat2, aes(year, PIB_per_cápita_PPA_2017US, color=country)) + geom_line (size = 1) + theme(plot.caption = element_text(size=7)) +
labs(subtitle="US$ de 2017", y="Dólares constante de 2017", x="Años", title="PIB per cápita PPA real", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") 
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5c3cf20b-9f13-4b9b-a898-efec63a96a60)

**ggplot** es una comando que tiene muchas opciones de ser utilizado, a continuación les comparto varias direcciones de internet donde a partir de la réplica podrán obtener el gráfico que desean:

* http://r-statistics.co/Top50-Ggplot2-Visualizations-MasterList-R-Code.html
* https://r-graph-gallery.com/ggplot2-package.html
* https://r-graph-gallery.com/279-plotting-time-series-with-ggplot2.html
* https://r-charts.com/evolution/time-series-ggplot2/

## Ejercicio utilizando el PIB per cápita[^3] de Colombia a precios constantes en pesos
[^3]:El PIB per cápita es el producto interno bruto dividido por la población a mitad de año.

En la sección 2 del curso se va a hacer buena parte del análisis utilizando el PIB per cápita de Colombia a precios constantes en moneda local (es decir, en pesos). Por lo tanto, a continuación se les muestra como manipular dicha información.

Al comienzo de cualquier programación en $R$ le recomendamos copiar los siguientes comandos para limpiar el área de trabajo:
``` r
rm(list = ls())
```
Llamamos a las librerías que se van a utilizar en el resto de este ejemplo
``` r
library(WDI)       # Esta librería sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(ggplot2)   # Esta librería sirve para construir gráficos interesantes
library(dplyr)     # Esta librería permite manipular las bases de datos de R de una forma sencilla
```
El siguiente comando lo utilizo para confirmar que la variable "NY.GDP.PCAP.KN" representa el PIB per cápita a precios constantes en moneda local
``` r
WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')
```
Dando como resultado
``` r
    indicator                          name
1 NY.GDP.PCAP.KN GDP per capita (constant LCU)
```
Se llama a la variable "NY.GDP.PCAP.KN" para Colombia y se crea la base de datos "dat" a partir de la información del Banco Mundial:
``` r
dat = WDI(indicator= c(PIB_per_capita = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
```
Se hace una visualización rápida de los datos:
``` r
head(dat)
```
Obteniendo:
``` r
> head(dat)
   country iso2c iso3c year    PIBpc
1 Colombia    CO   COL 2022 18802572
2 Colombia    CO   COL 2021 17612821
3 Colombia    CO   COL 2020 16047602
4 Colombia    CO   COL 2019 17558668
5 Colombia    CO   COL 2018 17330777
6 Colombia    CO   COL 2017 17220832
```
Observe que para esta variable los datos están organizados en orden descendente (es decir, desde el último año hasta el primero). 

Para organizar los datos en orden ascendente, copie los siguientes comandos.
``` r
dat = WDI(indicator= c(PIBpc = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es") # Obtenemos los datos de PIBpc para Colombia desde 1960 hasta 2019
dat <- dat %>% arrange(year) # Ordenamos los datos desde el más antiguo hasta el más nuevo
dat <- na.omit(dat)       # Este comando sirve para eliminar una fila para la cual no existe información
head(dat)
```
Obteniendo:
``` r
> head(dat)
   country iso2c iso3c year   PIBpc
1 Colombia    CO   COL 1960 5349334
2 Colombia    CO   COL 1961 5449711
3 Colombia    CO   COL 1962 5569506
4 Colombia    CO   COL 1963 5578865
5 Colombia    CO   COL 1964 5746356
6 Colombia    CO   COL 1965 5778607
```
Se van a crear las variables PIBpc y C1PIBpc como formatos de series de tiempo anuales que comienzan en 1960 y vamos a construir dos bases de datos, 
* una, llamada df1, con las variables Año y PIBpc y
* otra, llamada df2, con la variable Año y C1PIBpc 
``` r
PIBpc <- ts(dat$NY.GDP.PCAP.KN, frequency = 1, start = c(1960)) # Creamos la variable PIBpc como serie de tiempo

C1PIBpc <- diff(PIBpc, differences = 1) # Creamos la variable C1PIBpc  # Creamos la variable C1PIBpc como serie de tiempo

df1 <- data.frame(Año = time(PIBpc), C1PIBpc = as.numeric(PIBpc)) # Convertimos PIBpc a un dataframe
df2 <- data.frame(Año = time(C1PIBpc), C1PIBpc = as.numeric(C1PIBpc)) # Convertimos C1PIBpc a un dataframe
```

Grafique PIBpc y C1PIBpc utilizando los siguientes comandos:
``` r
# Gráfico de PIBpc
ggplot(df1, aes(x = Año, y = PIBpc)) + 
  geom_line(linewidth = 0.2) + 
  labs(subtitle = "1960-2019", y = "Pesos constantes", 
       title = "PIB per cápita real de Colombia", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5))

# Gráfico de C1PIBpc
ggplot(df2, aes(x = Año, y = C1PIBpc)) + 
  geom_line(linewidth = 0.2) + 
  labs(subtitle = "1961-2019", y = "Pesos constantes", 
       title = "Cambio en el PIB per cápita real de Colombia", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5))
```

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/63141ce0-dbc9-4de4-9aa2-4873f000a28e)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/08edcadd-11e5-4846-b313-cef6a07070e4)

En ambos gráficos se visualizan los siguientes hechos estilizados que afectaron al comportamiento del PIB per cápita de Colombia:
1) La crisis bancaria de comienzos de la década de 1980
2) La crisis económica de 1998/1999
3) No se alcanza a gráficar la crisis económica del Covid-19 en 2020 porque los gráficos sólo van hasta 2019

```

El código completo de $R$, con los principales comandos, utilizados en este ejemplo es:
``` r
###################
# Código Completo #
###################

rm(list = ls())

library(WDI)       # Esta librería sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(ggplot2)   # Esta librería sirve para construir gráficos interesantes
library(dplyr)     # Esta librería permite manipular las bases de datos de R de una forma sencilla

WDIsearch(string='NY.GDP.PCAP.KN', field='indicator')

# Obtenemos los datos de PIBpc para Colombia desde 1960 hasta 2019
dat <- WDI(indicator = "NY.GDP.PCAP.KN", country = "CO", start = 1960, end = 2019)
dat <- dat %>% arrange(year)
dat <- na.omit(dat)

PIBpc <- ts(dat$NY.GDP.PCAP.KN, frequency = 1, start = c(1960)) # Creamos la variable PIBpc

C1PIBpc <- diff(PIBpc, differences = 1) # Creamos la variable C1PIBpc # Creamos la variable C1PIBpc como serie de tiempo

df1 <- data.frame(Año = time(PIBpc), C1PIBpc = as.numeric(PIBpc)) # Convertimos PIBpc a un dataframe
df2 <- data.frame(Año = time(C1PIBpc), C1PIBpc = as.numeric(C1PIBpc)) # Convertimos C1PIBpc a un dataframe

# Gráfico de PIBpc
ggplot(df1, aes(x = Año, y = PIBpc)) + 
  geom_line(linewidth = 0.2) + 
  labs(subtitle = "1960-2019", y = "Pesos constantes", 
       title = "PIB per cápita real de Colombia", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5))

# Gráfico de C1PIBpc
ggplot(df2, aes(x = Año, y = C1PIBpc)) + 
  geom_line(linewidth = 0.2) + 
  labs(subtitle = "1961-2019", y = "Pesos constantes", 
       title = "Cambio en el PIB per cápita real de Colombia", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5))
```
---
---
# Preguntas de selección múltiple

1. **Para un estudio que está llevando a cabo, necesita la variable "Gasto en consumo final del gobierno general como porcentaje del PIB". Ya sea utilizando una búsqueda por Internet o por $RStudio$, encuentre cuál es el código de la variable que en los _Indicadores de Desarrollo Mundial_ se acerca más a la variable requerida:**
 
   a) NE.CON.TOTL.ZS.

   b) NE.CON.TOTL.CN.

   c) NE.GDI.FPRV.ZS.

   d) NE.CON.GOVT.ZS.

2. **¿Según los _Indicadores de Desarrollo Mundial_, cuánto vale el índice de Gini de Angola del año 2000?**
 
   a) 63.1.

   b) 52.0.

   c) 45.4.

   d) 43.7.
---
---

| [Anterior Subsección: 1.1. Series de tiempo](../Seccion01_01/Readme.md) | [Sección 1 - Las Series de Tiempo, _R_ y los _Indicadores de Desarrollo Mundial_](../Readme.md) |
|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>

