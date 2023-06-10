# 02. Los Indicadores de Desarrollo Económico del Banco Mundial
"_Los Indicadores del Desarrollo Mundial son la principal colección de estadísticas internacionales sobre desarrollo que el Banco Mundial recopila de fuentes reconocidas oficialmente e incluyen estimaciones a nivel nacional, regional y mundial. Proporcionan acceso a casi 1600 indicadores para 217 economías, y algunas de las series cronológicas se remontan a más de 50 años. En la base de datos, los usuarios —analistas, encargados de formular políticas, académicos y todas las personas interesadas en la situación del mundo— pueden encontrar información relacionada con todos los aspectos del desarrollo, tanto actuales como históricos._" ([Banco Mundial](https://blogs.worldbank.org/es/opendata/guia-en-linea-para-los-indicadores-del-desarrollo-mundial-una-nueva-manera-de-encontrar-datos-sobre-el-desarrollo#:~:text=Los%20Indicadores%20del%20Desarrollo%20Mundial%20(WDI)%20son%20la%20principal%20colecci%C3%B3n,nivel%20nacional%2C%20regional%20y%20mundial.)) 

La Base de Datos con los Indicadores de Desarrollo del Banco Mundial pueden consultarse en https://datos.bancomundial.org/.
Si se desea tener un acceso rápido a la misma utilizando R se puede descargar el paquete [WDI]():

``` r
install.packages('WDI')
```

_El paquete **WDI** busca y descargue datos de más de 40 bases de datos alojadas por el Banco Mundial, incluidos los Indicadores de desarrollo mundial ('WDI'), Estadísticas de deuda internacional, Doing Business, Índice de capital humano e Índice de pobreza subnacional_

### Buscando los datos
Se pueden buscar datos utilizando palabras clave en **WDIsearch**. Por ejemplo, si se requieren los datos del Producto Interno Bruto copie:
``` r
WDIsearch('gdp')
```

Y obtendrá como respuesta:

``` r
> WDIsearch('gdp')[1:10,]
      indicator              name                                                                      
 [1,] "BG.GSR.NFSV.GD.ZS"    "Trade in services (% of GDP)"                                            
 [2,] "BM.KLT.DINV.GD.ZS"    "Foreign direct investment, net outflows (% of GDP)"                      
 [3,] "BN.CAB.XOKA.GD.ZS"    "Current account balance (% of GDP)"                                      
 [4,] "BN.CUR.GDPM.ZS"       "Current account balance excluding net official capital grants (% of GDP)"
 [5,] "BN.GSR.FCTY.CD.ZS"    "Net income (% of GDP)"                                                   
 [6,] "BN.KLT.DINV.CD.ZS"    "Foreign direct investment (% of GDP)"                                    
 [7,] "BN.KLT.PRVT.GD.ZS"    "Private capital flows, total (% of GDP)"                                 
 [8,] "BN.TRF.CURR.CD.ZS"    "Net current transfers (% of GDP)"                                        
 [9,] "BNCABFUNDCD_"         "Current Account Balance, %GDP"                                           
[10,] "BX.KLT.DINV.WD.GD.ZS" "Foreign direct investment, net inflows (% of GDP)" 
```

**WDIsearch** busca todas las variables en cuya definición este la palabla en cuestión, por lo que se requeriria buscar un concepto más específico. Por ejemplo, si está buscando el PIB per cápita en dólares constantes, mejor busque

``` r
WDIsearch('gdp.*capita.*constant')
     indicator           name                                                 
[1,] "GDPPCKD"           "GDP per Capita, constant US$, millions"             
[2,] "NY.GDP.PCAP.KD"    "GDP per capita (constant 2000 US$)"                 
[3,] "NY.GDP.PCAP.KN"    "GDP per capita (constant LCU)"                      
[4,] "NY.GDP.PCAP.PP.KD" "GDP per capita, PPP (constant 2005 international $)"
```

### Descargando y utilizando los datos

Descargue la serie desee para los países requeridos:

``` r
dat = WDI(indicator='NY.GDP.PCAP.KD', country=c('MX','CA','US'), start=1960, end=2012)
```

Nota: puede usar **country='all'** para descargar datos de todos los países disponibles. También puede descargar varios indicadores a la vez. Por ejemplo,
``` r
WDI(country = "all", indicator = "NY.GDP.PCAP.KD", start = 1960, end = NULL, extra = FALSE, cache = NULL, latest = NULL, language = "es")
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
| [Anterior Sección: 01-01. Series de tiempo](../../Seccion01_01/README.md) | [Inicio](../../Readme.md) | [Siguiente Sección: 02-01. Introducción Análisis Univariado](../Seccion01_02/Readme.md) | 
|---------------------------------------------------------------------------|---------------------------|---------------------------------------------------------------------------------------------------|

