<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.5:
# Caso de estudio de un modelo $VEC$ utilizando la base de datos _Indicadores de Desarrollo Mundial_

Los países que pertenecen a una misma región pueden verse influenciados por choques y tendencias similares. En consecuencia, se han escogido tres países de Latinoamérica (**Brasil, Colombia y México**) para determinar si entre el PIB de estos países existe una relación de largo plazo. La medida del PIB utilizada para hacer el análisis es el **PIB per cápita a dólares constantes de 2015**.

Para el análisis, se va a preparar la base de datos en $R$:
``` r
rm(list = ls())

library(WDI)
library(dplyr)
library(vars)
library(tidyr) 
library(ggplot2)

WDIsearch(string='NY.GDP.PCAP.KD', field='indicator') # Se confirma el nombre de la variable que se va a analizar

# Se obtienen los datos de PIBpc para Brasil, Colombia y México desde 1960 hasta 2021 y se convierten en formato de serie de tiempo
dat <- WDI(indicator = c(PIBpc = "NY.GDP.PCAP.KD"), country = c('BR', 'CO', 'MX'), start = 1960, end = 2021, language = "es")

dat <-  mutate(dat, iso2c=NULL, iso3c=NULL)
dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat <-  mutate(dat, iso2c=NULL, iso3c=NULL) # Se renombran las columnas y se seleccionan solo las columnas necesarias
PIBpc_paises <- spread(dat, key = country, value = PIBpc) # Se utiliza spread() de la libreria tidyr para convertir los datos a formato wide para tener una columna por país
PIBpc_paises_matrix <- as.matrix(PIBpc_paises[, -1])  # Se convierte PIBpc_paises en una matriz
seriesVEC <- ts(PIBpc_paises_matrix, frequency = 1, start = 1960) # Se crean las variables en formato de serie de tiempo
```

Un gráfico de los datos en niveles y en primeras diferencias se puede obtener con el siguiente comando
``` r
ggplot(dat, aes(x = year, y = PIBpc, color = country)) +
  geom_line() +
  labs(x = "Años", y = "Dólares constantes de 2015", color = "País", 
       caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  ggtitle("PIB per cápita para Brasil, Colombia y México") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5)) +
  theme(plot.caption = element_text(size = 7))  

# Los siguientes comandos sirven para crear una base de datos que incluya en la primera columna los años y en las siguientes la primera diferencia de PIBpc para cada uno de los países
first_diff_seriesVEC <- diff(seriesVEC, differences = 1)  # Con este comando se obtienen las primeras diferencias de cada una de las series
time_index <- time(first_diff_seriesVEC)  # Con este comando se crea la variable que representa los años en la nueva base de datos
df_diff <- data.frame(time_index, first_diff_seriesVEC)   # Con este comando se crea la base de datos df_diff
colnames(df_diff) <- c("Año", "Brasil", "Colombia", "México")   # Con este comando se crean los titulos a la base de datos df_diff

ggplot(data = df_diff, aes(x = Año)) +
  geom_line(aes(y = Brasil, color = "Brasil")) +
  geom_line(aes(y = Colombia, color = "Colombia")) +
  geom_line(aes(y = México, color = "México")) +
  labs(x = "Años", y = "Primeras Diferencias", color = "País",
  caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial") +
  ggtitle("Primeras diferencias del PIB per cápita para Brasil, Colombia y México") +
  scale_x_continuous(name = "Años", breaks = seq(1960, 2020, by = 5)) +
  theme(plot.caption = element_text(size = 7))  
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/8b84d20b-77bb-402f-a766-1a528d1556a3)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/cd3994c5-6803-4bed-979d-dd81d26928dc)

Según los dos gráficos de arriba, los tres PIBpc parecen ser no estacionarios y podría existir alguna tendencia común, pero la misma no se puede confirmar hasta hacer las pruebas de cointegración. 

Previamente, aunque no se muestra en esta sección, se hicieron pruebas de raíz unitaria a la variable $PIBpc$ de los tres países considerados y se obtuvó que en todos los casos eran $I(1)$, entonces, ahora se va a hacer la prueba de cointegración de Johansen para ver si existe una relación de cointegración entre los mismos.

### Selección del número de rezagos dentro del $VEC$
En primera instancia se va a determinar el número óptimo de rezagos a utilizar en el modelo multivariado. Como dentro del ejemplo no se ha formulado una teoría formal que nos determine las variables exógenas que interactuan dentro del sistema, entonces se van a utilizar utilizar las tres opciones que permite el comando **_VARselect_** como se ve en la siguiente configuración de comandos [^1]:

[^1]: **Observe que en el comando se utilizó la primera diferencia de cada una de las variables porque el _VEC_ es ante todo un _VAR_ en diferencias al que se le incluye la relación de largo plazo y porque los estadísticos de prueba propuestos por Johansen (1988) se basan en la especificación _VEC_.**

``` r
selected_order1 <- VARselect(first_diff_seriesVEC, lag.max = 8, type = "none") 
selected_order2 <- VARselect(first_diff_seriesVEC, lag.max = 8, type = "const")
selected_order3 <- VARselect(first_diff_seriesVEC, lag.max = 8, type = "trend") 
selected_order4 <- VARselect(first_diff_seriesVEC, lag.max = 8, type = "both") 

selected_order1
selected_order2
selected_order3
selected_order4
```

Obteniendo:
``` r
> selected_order1
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     1      1      1      1 

$criteria
                  1            2            3            4            5            6            7
AIC(n) 3.187085e+01 3.187432e+01 3.210731e+01 3.224132e+01 3.221236e+01 3.225507e+01 3.248687e+01
HQ(n)  3.199951e+01 3.213165e+01 3.249330e+01 3.275597e+01 3.285568e+01 3.302705e+01 3.338751e+01
SC(n)  3.220543e+01 3.254348e+01 3.311104e+01 3.357963e+01 3.388525e+01 3.426254e+01 3.482891e+01
FPE(n) 6.942130e+13 6.984122e+13 8.878821e+13 1.029514e+14 1.024110e+14 1.108648e+14 1.474480e+14
                  8
AIC(n) 3.222407e+01
HQ(n)  3.325337e+01
SC(n)  3.490069e+01
FPE(n) 1.221920e+14

> selected_order2
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     1      1      1      1 

$criteria
                  1            2            3            4            5            6            7
AIC(n) 3.170986e+01 3.182052e+01 3.205912e+01 3.223747e+01 3.215082e+01 3.222792e+01 3.246175e+01
HQ(n)  3.188141e+01 3.212073e+01 3.248800e+01 3.279501e+01 3.283702e+01 3.304278e+01 3.340527e+01
SC(n)  3.215596e+01 3.260120e+01 3.317438e+01 3.368731e+01 3.393523e+01 3.434691e+01 3.491532e+01
FPE(n) 5.912754e+13 6.629745e+13 8.493015e+13 1.032426e+14 9.732359e+13 1.096034e+14 1.470336e+14
                  8
AIC(n) 3.216505e+01
HQ(n)  3.323724e+01
SC(n)  3.495320e+01
FPE(n) 1.187897e+14

> selected_order3
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     1      1      1      1 

$criteria
                  1            2            3            4            5            6            7
AIC(n) 3.172182e+01 3.186432e+01 3.211732e+01 3.224333e+01 3.219521e+01 3.221611e+01 3.243598e+01
HQ(n)  3.189337e+01 3.216453e+01 3.254619e+01 3.280086e+01 3.288141e+01 3.303097e+01 3.337950e+01
SC(n)  3.216792e+01 3.264500e+01 3.323258e+01 3.369316e+01 3.397962e+01 3.433511e+01 3.488955e+01
FPE(n) 5.983928e+13 6.926602e+13 9.001945e+13 1.038485e+14 1.017411e+14 1.083172e+14 1.432934e+14
                  8
AIC(n) 3.216208e+01
HQ(n)  3.323427e+01
SC(n)  3.495023e+01
FPE(n) 1.184375e+14

> selected_order4
$selection
AIC(n)  HQ(n)  SC(n) FPE(n) 
     1      1      1      1 

$criteria
                  1            2            3            4            5            6            7
AIC(n) 3.168564e+01 3.183854e+01 3.206547e+01 3.223045e+01 3.218027e+01 3.225216e+01 3.248721e+01
HQ(n)  3.190007e+01 3.218164e+01 3.253723e+01 3.283087e+01 3.290936e+01 3.310991e+01 3.347362e+01
SC(n)  3.224327e+01 3.273075e+01 3.329225e+01 3.379181e+01 3.407622e+01 3.448268e+01 3.505231e+01
FPE(n) 5.776034e+13 6.766010e+13 8.586725e+13 1.033285e+14 1.014589e+14 1.143022e+14 1.546282e+14
                  8
AIC(n) 3.220989e+01
HQ(n)  3.332497e+01
SC(n)  3.510957e+01
FPE(n) 1.285486e+14
```

En todos los casos, se puede observar que es mejor utilizar un rezago o en su defecto dos rezagos. Por lo tanto, dado que en $R$ la prueba de Johansen tiene que incluir más de un rezago, entonces se van a hacer lan prueba de Johanhes con dos rezagos. 

### Pruebas de Causalidad de Granger
En la sección 3.2.4.(T) se afirmo que Sims, Stock y Watson (1990) consideraban que las pruebas de causalidad de Granger en sistemas cointegrados no eran recomendadas. Sin embargo, dado que no tenemos elaborado un contexto teórico que nos establezca cuál de los tres $\Delta PIBpc$ analizados es más exógeno o cuál es más endógeno; entonces vamos a apoyarnos en la prueba de causalidad de Granger para establecer ese orden. Para ello se van a utilizar los siguientes comandos:
``` r
grangertest(diff(Brasil) ~ diff(Colombia),order=2,data=seriesVEC)
grangertest(diff(Colombia) ~ diff(Brasil),order=2,data=seriesVEC)

grangertest(diff(Brasil) ~ diff(México),order=2,data=seriesVEC)
grangertest(diff(México) ~ diff(Brasil),order=2,data=seriesVEC)

grangertest(diff(Colombia) ~ diff(México),order=2,data=seriesVEC)
grangertest(diff(México) ~ diff(Colombia),order=2,data=seriesVEC)
```

Dando como resultado
``` r
> grangertest(diff(Brasil) ~ diff(Colombia),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(Brasil) ~ Lags(diff(Brasil), 1:2) + Lags(diff(Colombia), 1:2)
Model 2: diff(Brasil) ~ Lags(diff(Brasil), 1:2)
  Res.Df Df      F Pr(>F)
1     54                 
2     56 -2 1.7048 0.1915
> grangertest(diff(Colombia) ~ diff(Brasil),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(Colombia) ~ Lags(diff(Colombia), 1:2) + Lags(diff(Brasil), 1:2)
Model 2: diff(Colombia) ~ Lags(diff(Colombia), 1:2)
  Res.Df Df      F Pr(>F)
1     54                 
2     56 -2 1.5569 0.2201
> 
> grangertest(diff(Brasil) ~ diff(México),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(Brasil) ~ Lags(diff(Brasil), 1:2) + Lags(diff(México), 1:2)
Model 2: diff(Brasil) ~ Lags(diff(Brasil), 1:2)
  Res.Df Df      F Pr(>F)
1     54                 
2     56 -2 2.0783  0.135
> grangertest(diff(México) ~ diff(Brasil),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(México) ~ Lags(diff(México), 1:2) + Lags(diff(Brasil), 1:2)
Model 2: diff(México) ~ Lags(diff(México), 1:2)
  Res.Df Df      F Pr(>F)
1     54                 
2     56 -2 0.4505 0.6397
> 
> grangertest(diff(Colombia) ~ diff(México),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(Colombia) ~ Lags(diff(Colombia), 1:2) + Lags(diff(México), 1:2)
Model 2: diff(Colombia) ~ Lags(diff(Colombia), 1:2)
  Res.Df Df      F  Pr(>F)  
1     54                    
2     56 -2 3.5712 0.03495 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
> grangertest(diff(México) ~ diff(Colombia),order=2,data=seriesVEC)
Granger causality test

Model 1: diff(México) ~ Lags(diff(México), 1:2) + Lags(diff(Colombia), 1:2)
Model 2: diff(México) ~ Lags(diff(México), 1:2)
  Res.Df Df      F Pr(>F)
1     54                 
2     56 -2 1.0161 0.3688
```
Tomando en consideración los _p-values_ más bajos se establece que:
* el $PIBpc$ de México (de manera significativa) explica en el sentido de Granger al $PIBpc$ de Colombia
* el $PIBpc$ de Colombia (de manera no significativa) explica en el sentido de Granger al $PIBpc$ de Brasil
* el $PIBpc$ de México (de manera no significativa) explica en el sentido de Granger al $PIBpc$ de Brasil

Por lo tanto, la variable más exógena es el $PIBpc$ de México, luego el $PIBpc$ de Colombia y por último el $PIBpc$ de Brasil

Establecemos el nuevo orden del VAR, para de una vez tenerlos listos para cuando se calculen las funciones impulso respuesta y la descomposición de varianza del error de pronóstico:

``` r
nueva_matriz <- PIBpc_paises_matrix[, c("México", "Colombia", "Brasil")]
seriesVEC <- ts(nueva_matriz, frequency = 1, start = 1960) # Se reescriben las variables en formato de serie de tiempo
```


### Pruebas de Cointegración de Johansen
Recuerden que ya se había establecido que el número de rezagos para manejar en la prueba de Johansen es de 2 rezagos. Por otro lado, como la presencia de una constante es suficiente para que series $I(1)$ presenten una tendencia creciente como se ve en los diferentes $PIBpc$ que se encuentran en el primer gráfico de esta sección, entonces se van a hacer las pruebas de Johansen con intercepto, para ello se utilizan los siguientes comandos:

``` r
Johansen_traza_const <- ca.jo(seriesVEC, type = "eigen", ecdet = "const", K = 2)
Johansen_eigen_const <- ca.jo(seriesVEC, type = "trace", ecdet = "const", K = 2)

summary(Johansen_traza_const)
summary(Johansen_eigen_const)
``` 
Obteniendo 

``` r
> summary(Johansen_traza_const)

###################### 
# Johansen-Procedure # 
###################### 

Test type: maximal eigenvalue statistic (lambda max) , without linear trend and constant in cointegration 

Eigenvalues (lambda):
[1] 3.609066e-01 1.557082e-01 1.365868e-01 1.221245e-15

Values of teststatistic and critical values of test:

          test 10pct  5pct  1pct
r <= 2 |  8.81  7.52  9.24 12.97
r <= 1 | 10.16 13.75 15.67 20.20
r = 0  | 26.86 19.77 22.00 26.81

Eigenvectors, normalised to first column:
(These are the cointegration relations)

                México.l2  Colombia.l2     Brasil.l2     constant
México.l2       1.0000000   1.00000000  1.000000e+00     1.000000
Colombia.l2     0.2175845   0.01230674 -4.737442e-02    -5.695307
Brasil.l2      -1.0673808  -1.13561842 -4.064931e-01     2.681903
constant    -2402.6937213 -52.52095359 -4.601228e+03 -2129.743048

Weights W:
(This is the loading matrix)

             México.l2 Colombia.l2    Brasil.l2      constant
México.d   -0.21710400  0.02204905 -0.058391502  4.130565e-16
Colombia.d -0.12003776  0.01951143  0.023547318  2.668445e-16
Brasil.d   -0.05569437  0.09980301  0.006138462 -7.402360e-16

> summary(Johansen_eigen_const)

###################### 
# Johansen-Procedure # 
###################### 

Test type: trace statistic , without linear trend and constant in cointegration 

Eigenvalues (lambda):
[1] 3.609066e-01 1.557082e-01 1.365868e-01 1.221245e-15

Values of teststatistic and critical values of test:

          test 10pct  5pct  1pct
r <= 2 |  8.81  7.52  9.24 12.97
r <= 1 | 18.97 17.85 19.96 24.60
r = 0  | 45.83 32.00 34.91 41.07

Eigenvectors, normalised to first column:
(These are the cointegration relations)

                México.l2  Colombia.l2     Brasil.l2     constant
México.l2       1.0000000   1.00000000  1.000000e+00     1.000000
Colombia.l2     0.2175845   0.01230674 -4.737442e-02    -5.695307
Brasil.l2      -1.0673808  -1.13561842 -4.064931e-01     2.681903
constant    -2402.6937213 -52.52095359 -4.601228e+03 -2129.743048

Weights W:
(This is the loading matrix)

             México.l2 Colombia.l2    Brasil.l2      constant
México.d   -0.21710400  0.02204905 -0.058391502  4.130565e-16
Colombia.d -0.12003776  0.01951143  0.023547318  2.668445e-16
Brasil.d   -0.05569437  0.09980301  0.006138462 -7.402360e-16
```

Observen que con ambas pruebas de Johansen al 5% de significancia:
* Con $r=0$: $\lambda_{traza}=26.86>22.00$ y $\lambda_{max}=45.83>34.91$, rechazamos la hipótesis nula de $r = 0$. Esto sugiere que hay al menos una relación de cointegración en el sistema
* Con $r=1$: $\lambda_{traza}=10.16<15.67$ y $\lambda_{max}=18.97<19.96$, no tenemos suficiente evidencia para rechazar la hipótesis nula de $r \le 1$. En otras palabras, no podemos afirmar que haya más de una relación de cointegración en el sistema. Sin embargo, esto no descarta la posibilidad de que haya al menos una relación de cointegración en el sistema.

En conclusión, combinando los resultados de $r=0$ y de $r=1$, al 5% de significancia en nuestro sistema existe una única relación de largo plazo. Teniendo esta información se procede a estimar un $VEC$ con una única relación de cointegración

``` r
modeloVEC <- cajorls(Johansen_traza_const, r = 1, reg.number = NULL)
modeloVEC
```
Obteniendo

``` r
> modeloVEC
$rlm

Call:
lm(formula = substitute(form1), data = data.mat)

Coefficients:
              México.d  Colombia.d  Brasil.d
ect1          -0.21710  -0.12004    -0.05569
México.dl1     0.01999  -0.14988    -0.12567
Colombia.dl1  -0.64707  -0.27881    -0.25184
Brasil.dl1     0.12388   0.22195     0.48306


$beta
                     ect1
México.l2       1.0000000
Colombia.l2     0.2175845
Brasil.l2      -1.0673808
constant    -2402.6937213
```

Esta salida, implica que cada una de las ecuaciones del $VEC$ que se ha estimado adoptan la siguiente forma:

1) $\Delta PIB_{Mexico,t}=-0.22(PIB_{México,t-2}+0.22PIB_{Colombia,t-2}-1.07PIB_{Brasil,t-2}-2403)+0.02\Delta PIB_{Mexico,t-1}-0.65\Delta PIB_{Colombia,t-1}+0.12\Delta PIB_{Brasil,t-1}+\varepsilon_{Mexico,t}$
2) $\Delta PIB_{Colombia,t}=-0.12(PIB_{México,t-2}+0.22PIB_{Colombia,t-2}-1.07PIB_{Brasil,t-2}-2403)-0.15\Delta PIB_{Mexico,t-1}-0.28\Delta PIB_{Colombia,t-1}+0.22\Delta PIB_{Brasil,t-1}+\varepsilon_{Colombia,t}$
3) $\Delta PIB_{Brasil,t}=-0.05(PIB_{México,t-2}+0.22PIB_{Colombia,t-2}-1.07PIB_{Brasil,t-2}-2403)-0.12\Delta PIB_{Mexico,t-1}-0.25\Delta PIB_{Colombia,t-1}+0.48\Delta PIB_{Brasil,t-1}+\varepsilon_{Brasil,t}$

Con el siguiente comando se puede obtener mayor información de la estimación a corto plazo del $VEC$. 
``` r
summary(modeloVEC$rlm)
```

Dando como resultado:

``` r
> summary(modeloVEC$rlm)
Response México.d :

Call:
lm(formula = México.d ~ ect1 + México.dl1 + Colombia.dl1 + 
    Brasil.dl1 - 1, data = data.mat)

Residuals:
    Min      1Q  Median      3Q     Max 
-823.56  -63.33   23.19  147.67  417.97 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
ect1         -0.21710    0.05106  -4.252 8.12e-05 ***
México.dl1    0.01999    0.13705   0.146   0.8846    
Colombia.dl1 -0.64707    0.35228  -1.837   0.0715 .  
Brasil.dl1    0.12388    0.16606   0.746   0.4588    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 246 on 56 degrees of freedom
Multiple R-squared:  0.2653,	Adjusted R-squared:  0.2129 
F-statistic: 5.057 on 4 and 56 DF,  p-value: 0.0015


Response Colombia.d :

Call:
lm(formula = Colombia.d ~ ect1 + México.dl1 + Colombia.dl1 + 
    Brasil.dl1 - 1, data = data.mat)

Residuals:
    Min      1Q  Median      3Q     Max 
-556.91  -41.04   22.58   63.66  342.04 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
ect1         -0.12004    0.02641  -4.545 2.98e-05 ***
México.dl1   -0.14988    0.07089  -2.114   0.0390 *  
Colombia.dl1 -0.27881    0.18222  -1.530   0.1316    
Brasil.dl1    0.22195    0.08590   2.584   0.0124 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 127.2 on 56 degrees of freedom
Multiple R-squared:  0.3435,	Adjusted R-squared:  0.2966 
F-statistic: 7.326 on 4 and 56 DF,  p-value: 8.09e-05


Response Brasil.d :

Call:
lm(formula = Brasil.d ~ ect1 + México.dl1 + Colombia.dl1 + Brasil.dl1 - 
    1, data = data.mat)

Residuals:
    Min      1Q  Median      3Q     Max 
-589.54  -68.49   71.59  197.61  453.24 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)   
ect1         -0.05569    0.04584  -1.215  0.22950   
México.dl1   -0.12567    0.12304  -1.021  0.31145   
Colombia.dl1 -0.25184    0.31626  -0.796  0.42922   
Brasil.dl1    0.48306    0.14908   3.240  0.00201 **
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 220.8 on 56 degrees of freedom
Multiple R-squared:  0.1975,	Adjusted R-squared:  0.1402 
F-statistic: 3.446 on 4 and 56 DF,  p-value: 0.01378

```

Por lo tanto,

* EL $PIBpc$ de corto plazo de México se ve afectado tanto por la relación de cointegración como por el $PIBpc$ de corto plazo de Colombia.
* EL $PIBpc$ de corto plazo de Colombia se ve afectado tanto por la relación de cointegración como por el $PIBpc$ de corto plazo de Brasil y México.
* El $PIBpc$ de corto plazo de Brasil está mucho más afectado por su propio comportamiento que por el comportamiento del $PIBpc$ de corto plazo de Colombia y México.

Estos resultados parecen indicar de que el $PIBpc$ de corto plazo de Brasil fuera exógeno dentro del sistema. En consecuencia vamos a hacer pruebas de exogeneidad débil a cada uno de los $PIBpc$ de corto plazo, para ello utilizaremos el siguiente código:[^1]

[^1]: **La matriz A1 representa al PIB de Brasil exógeno dentro del sistema VEC, la matriz A2 representa al PIB de Colombia exógeno dentro del sistema VEC y la matriz A32 representa al PIB de Colombia exógeno dentro del sistema VEC**

```r
A1 <- t(matrix(c(1, 0, 0, 1, 0, 0), nrow = 2, ncol = 3))
A2 <- t(matrix(c(1, 0, 0, 0, 0, 1), nrow = 2, ncol = 3))
A3 <- t(matrix(c(0, 0, 1, 0, 0, 1), nrow = 2, ncol = 3))

VECrest1 <- (alrtest(z = Johansen_traza_const, A = A1, r = 1))
summary(VECrest1)
VECrest2 <- (alrtest(z = Johansen_traza_const, A = A2, r = 1))
summary(VECrest2)
VECrest3 <- (alrtest(z = Johansen_traza_const, A = A3, r = 1))
summary(VECrest3)
```

Obteniendo:
```r
> summary(VECrest1)

###################### 
# Johansen-Procedure # 
###################### 

Estimation and testing under linear restrictions on beta 

The VECM has been estimated subject to: 
beta=H*phi and/or alpha=A*psi

     [,1] [,2]
[1,]    1    0
[2,]    0    1
[3,]    0    0

Eigenvalues of restricted VAR (lambda):
[1] 0.3497 0.1367 0.0000 0.0000

The value of the likelihood ratio test statistic:
1.05 distributed as chi square with 1 df.
The p-value of the test statistic is: 0.31 

Eigenvectors, normalised to first column
of the restricted VAR:

                     [,1]
RK.México.l2       1.0000
RK.Colombia.l2     0.1970
RK.Brasil.l2      -1.0703
RK.constant    -2194.0809

Weights W of the restricted VAR:

        [,1]
[1,] -0.2178
[2,] -0.1134
[3,]  0.0000

> VECrest2 <- (alrtest(z = Johansen_traza_const, A = A2, r = 1))
> summary(VECrest2)

###################### 
# Johansen-Procedure # 
###################### 

Estimation and testing under linear restrictions on beta 

The VECM has been estimated subject to: 
beta=H*phi and/or alpha=A*psi

     [,1] [,2]
[1,]    1    0
[2,]    0    0
[3,]    0    1

Eigenvalues of restricted VAR (lambda):
[1] 0.2117 0.1489 0.0000 0.0000

The value of the likelihood ratio test statistic:
12.59 distributed as chi square with 1 df.
The p-value of the test statistic is: 0 

Eigenvectors, normalised to first column
of the restricted VAR:

                     [,1]
RK.México.l2       1.0000
RK.Colombia.l2     0.1122
RK.Brasil.l2      -0.9423
RK.constant    -2314.2595

Weights W of the restricted VAR:

        [,1]
[1,] -0.1979
[2,]  0.0000
[3,]  0.1095

> VECrest3 <- (alrtest(z = Johansen_traza_const, A = A3, r = 1))
> summary(VECrest3)

###################### 
# Johansen-Procedure # 
###################### 

Estimation and testing under linear restrictions on beta 

The VECM has been estimated subject to: 
beta=H*phi and/or alpha=A*psi

     [,1] [,2]
[1,]    0    0
[2,]    1    0
[3,]    0    1

Eigenvalues of restricted VAR (lambda):
[1] 0.2258 0.1538 0.0000 0.0000

The value of the likelihood ratio test statistic:
11.5 distributed as chi square with 1 df.
The p-value of the test statistic is: 0 

Eigenvectors, normalised to first column
of the restricted VAR:

                    [,1]
RK.México.l2      1.0000
RK.Colombia.l2    0.3034
RK.Brasil.l2     -1.4254
RK.constant    -685.2594

Weights W of the restricted VAR:

        [,1]
[1,]  0.0000
[2,] -0.0549
[3,]  0.0087
```

La siguiente tabla resume los resultados de la prueba de exogeneidad débil:

| Variable exógena    | Estadístico de la prueba | P-Value |¿La variable es débilmente exógena |
|:-------------------:|:------------------------:|:-------:|:---------------------------------:|
| $PIB_{Brasil,t}$    |$1.05$                    |$0.31$   | Si                                |
| $PIB_{Colombia,t}$  |$12.59$                   |$0$      | No                                |
| $PIB_{México,t}$    |$11.50$                   |$0$      | No                                |

Es decir, se podría sacar a Brasil de la relación de cointegración y sólo hacer el análisis con Colombia y México.

```r
vec_modelo <- vec2var(Johansen_traza_const, r = 1)

modelo.irf1<-irf(vec_modelo ,impulse="México", response=NULL)
modelo.irf2<-irf(vec_modelo,impulse="Colombia", response=NULL)
modelo.irf3<-irf(vec_modelo,impulse="Brasil", response=NULL)
modelo.irf1
modelo.irf2
modelo.irf3

plot(modelo.irf1)
plot(modelo.irf2)
plot(modelo.irf3)

Desc_var<-fevd(vec_modelo, n.ahead=10)
Desc_var
```

| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |
|-------------------------------------------------------------------------------|

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
