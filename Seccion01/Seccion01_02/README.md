# 02. R y los Indicadores de Desarrollo Económico del Banco Mundial
"_Los Indicadores del Desarrollo Mundial son la principal colección de estadísticas internacionales sobre desarrollo que el Banco Mundial recopila de fuentes reconocidas oficialmente e incluyen estadísticas a nivel nacional, regional y mundial. Proporcionan acceso a casi 1600 indicadores para 217 economías, y algunas de las series cronológicas se remontan a más de 50 años. En la base de datos, los usuarios —analistas, encargados de formular políticas, académicos y todas las personas interesadas en la situación del mundo— pueden encontrar información relacionada con todos los aspectos del desarrollo, tanto actuales como históricos._" ([Banco Mundial](https://blogs.worldbank.org/es/opendata/guia-en-linea-para-los-indicadores-del-desarrollo-mundial-una-nueva-manera-de-encontrar-datos-sobre-el-desarrollo#:~:text=Los%20Indicadores%20del%20Desarrollo%20Mundial%20(WDI)%20son%20la%20principal%20colecci%C3%B3n,nivel%20nacional%2C%20regional%20y%20mundial.)) 

La Base de Datos con los Indicadores de Desarrollo del Banco Mundial pueden consultarse en https://datos.bancomundial.org/.
Si se desea tener un acceso rápido a la misma utilizando R se puede descargar el paquete [WDI]():

``` r
install.packages('WDI')
```

_El paquete **WDI** busca y descarga información de más de 40 bases de datos alojadas por el Banco Mundial, incluidos los Indicadores de desarrollo mundial ('WDI'), estadísticas de deuda internacional, Doing Business, el índice de capital humano y el índice de pobreza subnacional_

### Buscando los datos
A continuación se presenta un ejemplo didáctico que le sugerira ideas acerca de cómo buscar información en la base de datos Índicadores de Desarrollo Económico.

Asuma que desea buscar los datos del Producto Interno Bruto per cápita por paridad del poder adquisitivo "PPA" a precios constantes. Para ello, primero tiene que identificar el nombre exacto de la variable dentro de la base de datos. Se le proponen dos opciones de busqueda:

1) **Utilizando internet:**

   * Dirijase a la dirección: https://datos.bancomundial.org/indicator
   * Busque la variable deseada: Si utiliza el buscador al comienzo de la página, sea específico.
   
     Por ejemplo,
     
     * si sólo coloca PIB (ver figura de abajo) puede que ninguna de las opciones a escoger incluyan la variable requerida.
       
      ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/888b9892-66a4-4ac1-8e99-0dcb0395a497)

     * si coloca PIB y PPA (ver figura de abajo) resulta más fácil que se le sugiera la variable requerida
   
      ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/ab5ada61-8916-415e-9713-2af78c6f8f2a)

     * Escoja con el _mouse_ la variable en cuestión "PIB, PPA ($ a precios internacionales constantes de 2011)" y será redirigido a la página de Internet donde se encuentra la variable en cuestión
       
     * Escoja con el _mouse_ la opción ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/d615bae3-bd1b-46e8-99cd-42f3566ef4f0) que se encuentra en la esquina superior derecha de la figura, y se desplegará la siguiente información
       
     ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4f57f0e3-d0f2-48c2-a31d-2d293544329b)
   
   * En la parte inferior del desplegable se encuentra el nombre de la variable: **NY.GDP.MKTP.PP.KD**

   
3) **Utilizando R:**

   En este caso, en R se utiliza el comando **WDIsearch**. A partir de la instrucción 

``` r
?WDIsearch
```
o de la instrucción

``` r
help WDIsearch
```
puede obtener información acerca de como utilizar este comando. Sin embargo, a continuación se le ofrecen una serie de consejos que le ayudaran a descubrir de forma ágil el nombre de la variable que se esta buscando   

#### Dentro del comando WDIsearch coloque, separados con un .*, palabras (en inglés) relacionadas con la variable que esta buscando.####

Por ejemplo:

``` r
WDIsearch('gdp.*capita.*constant')
```
Y  se obtiene:
``` r
> WDIsearch('gdp.*capita.*constant')
                 indicator                                                 name
[1,]     6.0.GDPpc_constant GDP per capita, PPP (constant 2011 international $) 
[2,]       NY.GDP.PCAP.KD                   GDP per capita (constant 2015 US$)
[3,]       NY.GDP.PCAP.KN                        GDP per capita (constant LCU)
[4,]    NY.GDP.PCAP.PP.KD  GDP per capita, PPP (constant 2017 international $)
[5,] NY.GDP.PCAP.PP.KD.87  GDP per capita, PPP (constant 1987 international $)
```
Por lo tanto, la variable solicitada es **NY.GDP.PCAP.PP.KD**. Existe también la opción de tener la variable a precios constantes de 1987, pero generalmente se prefieren los precios constantes más recientes

Observe que si sólo copia:

``` r
WDIsearch('gdp')
```

Obtendrá como respuesta todas las variables que tienen dentro dentro de su definición (name) la palabra 'gdp'. El problema con esta forma de visualizar los datos es que muchas las variables que incluyen "gdp" dentro de su nombre, por lo tanto la visualización de la información es demasiado engorrosa.

Una forma más sencilla de ver la información es sólo visualizando un determinado número de variables de la lista, por ejemplo si sólo se desea visulaizar las 10 primeras variables de la lista, se utiliza el comando:

``` r
WDIsearch('gdp')[1:10,]
```
Y se obtiene:

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

El problema es que no dentro de estas 10 primeras variables no se encuentra la variable requerida. 

Por lo tanto, como se comento anteriormente, lo mejor es que entro del comando WDIsearch coloque, separados con un .*, palabras (en inglés) relacionadas con la variable que esta buscando. Por ejemplo, 

``` r
> WDIsearch('gdp.*capita.*constant')
                 indicator                                                 name
[1,]     6.0.GDPpc_constant GDP per capita, PPP (constant 2011 international $) 
[2,]       NY.GDP.PCAP.KD                   GDP per capita (constant 2015 US$)
[3,]       NY.GDP.PCAP.KN                        GDP per capita (constant LCU)
[4,]    NY.GDP.PCAP.PP.KD  GDP per capita, PPP (constant 2017 international $)
[5,] NY.GDP.PCAP.PP.KD.87  GDP per capita, PPP (constant 1987 international $)
```
Observe como las variables de arriba incluyen en su definición los conceptos: "gdp", "capita" y "constant"

Cuando se utiliza el comando **WDIsearch**, las principales opciones para incluir en el comando son



### Descargando y utilizando los datos

Descargue la serie desee para los países requeridos (por ejemplo, el PIB per cápita en dólares constantes de 2015 en México, Canada y los Estados Unidos):

``` r
dat = WDI(indicator='NY.GDP.PCAP.KD', country=c('MX','CA','US'), start=1960, end=2012, language = "es")
```
Observe que con la orden language = "es" puede descargar la descripción de las variables en español, si no introdujera esta orden le saldría la información en inglés de forma predeterminada.

Nota: puede usar **country='all'** para descargar datos de todos los países disponibles. También puede descargar varios indicadores a la vez. Por ejemplo,
``` r
dat = WDI(country = "all", indicator = "NY.GDP.PCAP.KD", start = 1960, end = NULL, extra = FALSE, cache = NULL, latest = NULL, language = "es")
```

* end = NULL_ significa que sólo se van a tomar en cuenta 5 años

Visualice los datos:
``` r
head(dat)
  iso2c country NY.GDP.PCAP.KD year
1    CA  Canada       9374.883 1960
2    CA  Canada       9479.824 1961
3    CA  Canada       9967.366 1962
4    CA  Canada      10290.362 1963
5    CA  Canada      10774.653 1964
6    CA  Canada      11283.606 1965
```

Grafique los datos:
``` r
library(ggplot2)
ggplot(dat, aes(year, NY.GDP.PCAP.KD, color=country)) + geom_line() +
    xlab('Años') + ylab('PIB per cápita')
```
Otra opción (HAY QUE REVISAR)

``` r
plot.ts(NY.GDP.PCAP.KD)
plot(NY.GDP.PCAP.KD)
```

### Renombramiento Automático
Si el vector que proporciona a WDI tiene nombre, la función cambiará automáticamente el nombre de las columnas cuando sea posible.
``` r
dat <- WDI(indicator = c("pib_per_capita" = "NY.GDP.PCAP.KD", "poblacion" = "SP.POP.TOTL"))
head(dat)
iso2c    country year pib_per_capita poblacion
1    1A Arab World 2005       5378.379  316264728
2    1A Arab World 2006       5594.899  323773264
3    1A Arab World 2007       5711.663  331653797
4    1A Arab World 2008       5898.516  339825483
5    1A Arab World 2009       5782.422  348145094
6    1A Arab World 2010       5916.330  356508908
```
| [Anterior Sección: 01-01. Series de tiempo](../../Seccion01_01/README.md) | [Inicio](../../Readme.md) | [Siguiente Sección: 02-01. Introducción Análisis Univariado](../Seccion01_02/README.md) | 
|---------------------------------------------------------------------------|---------------------------|-----------------------------------------------------------------------------------------|

