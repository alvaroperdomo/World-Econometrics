# 02. Los Indicadores de Desarrollo Económico del Banco Mundial
"_Los Indicadores del Desarrollo Mundial son la principal colección de estadísticas internacionales sobre desarrollo que el Banco Mundial recopila de fuentes reconocidas oficialmente e incluyen estimaciones a nivel nacional, regional y mundial. Proporcionan acceso a casi 1600 indicadores para 217 economías, y algunas de las series cronológicas se remontan a más de 50 años. En la base de datos, los usuarios —analistas, encargados de formular políticas, académicos y todas las personas interesadas en la situación del mundo— pueden encontrar información relacionada con todos los aspectos del desarrollo, tanto actuales como históricos._" ([Banco Mundial](https://blogs.worldbank.org/es/opendata/guia-en-linea-para-los-indicadores-del-desarrollo-mundial-una-nueva-manera-de-encontrar-datos-sobre-el-desarrollo#:~:text=Los%20Indicadores%20del%20Desarrollo%20Mundial%20(WDI)%20son%20la%20principal%20colecci%C3%B3n,nivel%20nacional%2C%20regional%20y%20mundial.)) 

La Base de Datos con los Indicadores de Desarrollo del Banco Mundial pueden consultarse en https://datos.bancomundial.org/.
Si se desea tener un acceso rápido a la misma utilizando R se puede descargar el paquete [WDI]():

``` r
install.packages('WDI')
```

Se pueden buscar datos utilizando palabras clave en **WDIsearch**. Por ejemplo, si se requieren los datos del Producto Interno Bruto copie:
``` r
WDIsearch('gdp')
```

Y obtendrá como respuuesta:

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
