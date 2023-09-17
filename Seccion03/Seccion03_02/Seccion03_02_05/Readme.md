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

Es decir, se podría sacar a Brasil de la relación de cointegración y hacer sólo  el análisis con Colombia y México como se propone en el ejercicio que esta en el apartado de preguntas múltiples.

Para poder calcular las funciones impulso-respuesta y las descomposición de varianza de varianza de los errores de pronóstico, recuerde que se tiene primero que copiar el siguiente comando:

```r
vec_modelo <- vec2var(Johansen_traza_const, r = 1)
```

Hecho esto, a continuación se procede a calcular las funciones impulso-respuesta 50 periodos hacia adelante, con choques de una desviación estándar, utilizando la siguiente serie de comandos:

```r
modelo.irf1<-irf(vec_modelo ,impulse="México", response=NULL, n.ahead = 50)
modelo.irf2<-irf(vec_modelo,impulse="Colombia", response=NULL, n.ahead = 50)
modelo.irf3<-irf(vec_modelo,impulse="Brasil", response=NULL, n.ahead = 50)
modelo.irf1
modelo.irf2
modelo.irf3

plot(modelo.irf1)
plot(modelo.irf2)
plot(modelo.irf3)
```
Obteniendo el siguiente resultado
```r
> modelo.irf1<-irf(vec_modelo ,impulse="México", response=NULL, n.ahead = 50)
> modelo.irf2<-irf(vec_modelo,impulse="Colombia", response=NULL, n.ahead = 50)
> modelo.irf3<-irf(vec_modelo,impulse="Brasil", response=NULL, n.ahead = 50)
> modelo.irf1

Impulse response coefficients
$México
         México    Colombia   Brasil
 [1,] 237.66042  45.4687874 65.19239
 [2,] 221.06510  11.6416465 55.36568
 [3,] 202.76701   0.0160816 51.31145
 [4,] 173.70760 -14.6464662 45.41852
 [5,] 149.75273 -25.2768662 41.67360
 [6,] 129.19285 -34.2035973 38.75516
 [7,] 112.53586 -41.2573974 36.62059
 [8,]  99.05108 -46.9171655 34.98227
 [9,]  88.24401 -51.4206875 33.72023
[10,]  79.60420 -55.0077936 32.73447
[11,]  72.71549 -57.8607234 31.96045
[12,]  67.22970 -60.1293073 31.34981
[13,]  62.86505 -61.9325650 30.86685
[14,]  59.39417 -63.3657376 30.48420
[15,]  56.63496 -64.5046426 30.18071
[16,]  54.44194 -65.4096429 29.93985
[17,]  52.69914 -66.1287461 29.74860
[18,]  51.31426 -66.7001227 29.59671
[19,]  50.21383 -67.1541131 29.47606
[20,]  49.33946 -67.5148300 29.38022
[21,]  48.64472 -67.8014349 29.30408
[22,]  48.09271 -68.0291537 29.24359
[23,]  47.65412 -68.2100847 29.19552
[24,]  47.30564 -68.3538410 29.15734
[25,]  47.02876 -68.4680604 29.12700
[26,]  46.80877 -68.5588117 29.10289
[27,]  46.63398 -68.6309168 29.08374
[28,]  46.49510 -68.6882067 29.06852
[29,]  46.38475 -68.7337255 29.05643
[30,]  46.29708 -68.7698918 29.04682
[31,]  46.22743 -68.7986271 29.03919
[32,]  46.17208 -68.8214583 29.03313
[33,]  46.12811 -68.8395985 29.02831
[34,]  46.09317 -68.8540115 29.02448
[35,]  46.06541 -68.8654631 29.02144
[36,]  46.04335 -68.8745618 29.01902
[37,]  46.02583 -68.8817910 29.01710
[38,]  46.01190 -68.8875349 29.01557
[39,]  46.00084 -68.8920986 29.01436
[40,]  45.99205 -68.8957246 29.01340
[41,]  45.98507 -68.8986056 29.01263
[42,]  45.97952 -68.9008946 29.01203
[43,]  45.97511 -68.9027133 29.01154
[44,]  45.97161 -68.9041584 29.01116
[45,]  45.96882 -68.9053065 29.01085
[46,]  45.96661 -68.9062187 29.01061
[47,]  45.96485 -68.9069435 29.01042
[48,]  45.96346 -68.9075194 29.01027
[49,]  45.96235 -68.9079769 29.01014
[50,]  45.96147 -68.9083405 29.01005
[51,]  45.96077 -68.9086293 29.00997


Lower Band, CI= 0.95 
$México
           México    Colombia      Brasil
 [1,]  169.878466   -8.774494    7.282294
 [2,]  131.385043  -41.620909  -15.699795
 [3,]  117.341082  -64.128744  -46.298936
 [4,]   87.388365  -88.626559  -61.312033
 [5,]   62.345113 -104.880014  -76.281464
 [6,]   29.327304 -120.968946  -90.523095
 [7,]    6.336976 -135.141968 -103.236440
 [8,]   -8.300652 -149.982808 -116.324499
 [9,]  -26.407513 -162.784394 -128.246653
[10,]  -36.025367 -173.944532 -138.904954
[11,]  -41.591496 -183.708935 -147.047173
[12,]  -52.840726 -192.214370 -158.885009
[13,]  -64.986324 -199.623776 -169.469080
[14,]  -78.454352 -206.082064 -178.427357
[15,]  -92.496705 -211.715422 -186.681922
[16,] -103.462213 -216.633669 -194.290448
[17,] -109.962778 -220.931831 -201.303840
[18,] -115.301560 -224.692035 -207.769793
[19,] -122.894660 -229.055524 -213.731723
[20,] -133.223119 -234.699807 -219.229711
[21,] -140.582798 -239.958662 -224.300518
[22,] -144.963311 -244.862654 -228.977970
[23,] -147.516102 -249.439619 -233.293153
[24,] -149.695650 -253.714923 -237.274656
[25,] -151.558638 -257.797677 -240.948768
[26,] -153.152688 -261.715129 -245.805249
[27,] -154.517900 -265.391860 -250.909334
[28,] -155.688114 -268.845024 -255.732814
[29,] -156.691945 -272.090366 -260.363614
[30,] -157.553636 -275.142345 -264.822981
[31,] -158.293764 -278.014256 -269.042716
[32,] -158.929826 -280.718332 -272.923193
[33,] -159.476718 -283.265839 -274.159236
[34,] -159.947144 -285.667168 -275.302153
[35,] -160.364684 -287.931903 -276.359507
[36,] -161.335132 -290.068899 -277.338178
[37,] -162.193265 -292.086343 -278.244433
[38,] -162.952318 -293.991811 -279.122681
[39,] -163.623939 -295.792321 -279.972022
[40,] -164.218381 -297.494379 -280.760141
[41,] -164.744671 -299.104023 -281.491667
[42,] -165.210762 -300.626862 -282.170849
[43,] -165.623662 -302.068112 -282.801593
[44,] -165.989545 -303.432624 -283.387493
[45,] -166.313859 -304.724919 -283.931858
[46,] -166.601406 -305.949210 -284.437738
[47,] -166.856425 -307.109428 -284.907942
[48,] -167.082658 -308.209243 -285.345066
[49,] -167.283406 -309.252087 -285.751504
[50,] -167.461587 -310.241169 -286.129469
[51,] -167.619779 -311.179490 -286.481007


Upper Band, CI= 0.95 
$México
        México Colombia   Brasil
 [1,] 288.1503 88.10600 119.5318
 [2,] 276.2080 64.87179 137.5119
 [3,] 276.2458 60.17534 154.6117
 [4,] 262.8833 57.44105 171.3091
 [5,] 252.0857 59.80442 183.7022
 [6,] 247.3768 62.08475 185.0898
 [7,] 245.1764 65.11693 188.4300
 [8,] 244.1335 67.23986 190.9916
 [9,] 244.1389 68.55767 192.3918
[10,] 244.6167 68.96075 192.9659
[11,] 245.3249 68.85876 193.0850
[12,] 246.0570 69.01830 192.9432
[13,] 246.7263 68.77478 192.6897
[14,] 247.2923 68.33741 192.4032
[15,] 247.7521 67.87042 192.1312
[16,] 248.1170 67.47851 191.8939
[17,] 248.4045 67.20974 191.6986
[18,] 248.6316 67.25918 191.5440
[19,] 248.8128 67.33356 191.4253
[20,] 248.9593 67.37653 191.3364
[21,] 249.0793 67.38696 191.2713
[22,] 249.1787 67.37576 191.2246
[23,] 249.2617 67.35699 191.1919
[24,] 249.3312 67.34167 191.1696
[25,] 249.3895 67.34521 191.1548
[26,] 249.4382 67.35204 191.1455
[27,] 249.4789 67.34700 191.1400
[28,] 249.5128 67.35295 191.1372
[29,] 249.5408 67.35926 191.1361
[30,] 249.5640 67.36194 191.1361
[31,] 249.5831 67.36118 191.1368
[32,] 249.5988 67.35823 191.1378
[33,] 249.6117 67.35466 191.1389
[34,] 249.6222 67.35174 191.1400
[35,] 249.6309 67.35010 191.1410
[36,] 249.6380 67.34981 191.1419
[37,] 249.6438 67.35049 191.1426
[38,] 249.6486 67.35160 191.1432
[39,] 249.6525 67.35268 191.1437
[40,] 249.6557 67.35341 191.1440
[41,] 249.6583 67.35371 191.1443
[42,] 249.6605 67.35365 191.1445
[43,] 249.6622 67.35338 191.1446
[44,] 249.6637 67.35307 191.1447
[45,] 249.6649 67.35281 191.1448
[46,] 249.6659 67.35268 191.1448
[47,] 249.6667 67.35265 191.1448
[48,] 249.6673 67.35271 191.1448
[49,] 249.6679 67.35279 191.1448
[50,] 249.6683 67.35287 191.1448
[51,] 249.6687 67.35291 191.1448

> modelo.irf2

Impulse response coefficients
$Colombia
         México Colombia    Brasil
 [1,]   0.00000 114.2110  96.20835
 [2,] -61.98497 103.7221 113.92007
 [3,] -37.34337 129.2116 137.24250
 [4,] -15.49875 142.9159 147.96011
 [5,]  11.20509 156.8920 155.61348
 [6,]  34.54427 167.7770 160.36194
 [7,]  54.77233 176.7935 163.70681
 [8,]  71.49257 184.0082 166.08574
 [9,]  85.14652 189.8015 167.85564
[10,]  96.16389 194.4248 169.19743
[11,] 105.00476 198.1109 170.23297
[12,] 112.07124 201.0452 171.04025
[13,] 117.70690 203.3796 171.67411
[14,] 122.19499 205.2358 172.17396
[15,] 125.76607 206.7112 172.56925
[16,] 128.60596 207.8839 172.88241
[17,] 130.86361 208.8158 173.13076
[18,] 132.65801 209.5563 173.32787
[19,] 134.08402 210.1447 173.48436
[20,] 135.21720 210.6122 173.60865
[21,] 136.11762 210.9837 173.70737
[22,] 136.83307 211.2789 173.78579
[23,] 137.40154 211.5134 173.84810
[24,] 137.85322 211.6997 173.89759
[25,] 138.21209 211.8478 173.93692
[26,] 138.49723 211.9654 173.96817
[27,] 138.72379 212.0589 173.99299
[28,] 138.90380 212.1331 174.01272
[29,] 139.04682 212.1921 174.02839
[30,] 139.16046 212.2390 174.04084
[31,] 139.25075 212.2762 174.05073
[32,] 139.32248 212.3058 174.05859
[33,] 139.37948 212.3293 174.06484
[34,] 139.42477 212.3480 174.06980
[35,] 139.46075 212.3629 174.07375
[36,] 139.48934 212.3747 174.07688
[37,] 139.51205 212.3840 174.07937
[38,] 139.53010 212.3915 174.08134
[39,] 139.54444 212.3974 174.08292
[40,] 139.55583 212.4021 174.08416
[41,] 139.56488 212.4058 174.08516
[42,] 139.57208 212.4088 174.08594
[43,] 139.57779 212.4112 174.08657
[44,] 139.58233 212.4130 174.08707
[45,] 139.58594 212.4145 174.08746
[46,] 139.58880 212.4157 174.08778
[47,] 139.59108 212.4166 174.08803
[48,] 139.59289 212.4174 174.08822
[49,] 139.59433 212.4180 174.08838
[50,] 139.59547 212.4184 174.08851
[51,] 139.59638 212.4188 174.08861


Lower Band, CI= 0.95 
$Colombia
          México Colombia    Brasil
 [1,]    0.00000 69.98108 37.671111
 [2,] -127.60318 42.45366  4.151439
 [3,] -104.53036 66.26294 18.097004
 [4,]  -93.67278 62.57213 13.526756
 [5,]  -81.25980 71.05260 15.264850
 [6,]  -73.32103 71.83083 12.649868
 [7,]  -65.98398 71.63262  9.631241
 [8,]  -60.29884 71.85679  8.651476
 [9,]  -55.46934 72.66129 10.608813
[10,]  -51.48717 73.01459 13.306029
[11,]  -48.13064 73.52290 14.212920
[12,]  -45.31555 73.92376 14.113095
[13,]  -43.04618 74.35596 13.641418
[14,]  -41.65433 74.74413 12.864026
[15,]  -40.41837 75.15212 12.351668
[16,]  -39.31650 75.69987 12.002262
[17,]  -38.32973 76.21147 11.755398
[18,]  -37.44233 76.68330 11.569678
[19,]  -36.86223 77.12109 11.418050
[20,]  -36.78061 77.43784 11.290372
[21,]  -36.71981 77.44821 11.184917
[22,]  -36.67548 77.45571 11.101084
[23,]  -36.64414 77.46111 11.036963
[24,]  -36.62295 77.46498 10.989111
[25,]  -36.60963 77.46775 10.953614
[26,]  -36.60234 77.46972 10.926978
[27,]  -36.59963 77.47111 10.906592
[28,]  -36.60032 77.47210 10.890718
[29,]  -36.60350 77.47279 10.878263
[30,]  -36.60844 77.47328 10.868512
[31,]  -36.61457 77.47362 10.860940
[32,]  -36.62144 77.47386 10.855113
[33,]  -36.62872 77.47403 10.850655
[34,]  -36.63616 77.47415 10.847250
[35,]  -36.64355 77.47424 10.844643
[36,]  -36.65077 77.47430 10.842638
[37,]  -36.65772 77.47434 10.841090
[38,]  -36.66432 77.47437 10.839893
[39,]  -36.67054 77.47439 10.838967
[40,]  -36.67635 77.47440 10.838253
[41,]  -36.68175 77.47441 10.837702
[42,]  -36.68673 77.47442 10.837279
[43,]  -36.69131 77.47443 10.836953
[44,]  -36.69549 77.47443 10.836703
[45,]  -36.69931 77.47443 10.836510
[46,]  -36.70279 77.47444 10.836362
[47,]  -36.70593 77.47444 10.836247
[48,]  -36.70878 77.47444 10.836159
[49,]  -36.71135 77.47444 10.836091
[50,]  -36.71366 77.47444 10.836038
[51,]  -36.71573 77.47444 10.835998


Upper Band, CI= 0.95 
$Colombia
          México Colombia   Brasil
 [1,]   0.000000 133.8875 137.4453
 [2,]   1.195571 134.0735 201.7122
 [3,]  22.746440 163.0332 253.5777
 [4,]  62.828463 191.0949 276.0274
 [5,] 112.453240 221.1395 305.3599
 [6,] 141.985347 242.2743 326.1134
 [7,] 168.045537 258.7963 340.2541
 [8,] 195.961188 273.1443 349.1977
 [9,] 225.615304 285.8854 354.6691
[10,] 251.436575 295.6839 358.9315
[11,] 267.323734 303.0776 366.8723
[12,] 280.185865 312.2029 373.5099
[13,] 290.691935 319.9822 379.0741
[14,] 299.210834 326.4279 383.7397
[15,] 306.134415 331.8148 387.6561
[16,] 311.767990 336.2878 390.9442
[17,] 318.711073 340.0148 393.7061
[18,] 324.810342 343.1130 396.0263
[19,] 329.943568 345.6925 397.9757
[20,] 334.263291 348.8754 399.6138
[21,] 337.898528 352.2155 400.9904
[22,] 340.957679 355.1982 402.1473
[23,] 343.532104 357.8666 403.1196
[24,] 345.698635 360.2580 403.9368
[25,] 347.521949 362.4049 404.6237
[26,] 349.056456 364.3357 405.2011
[27,] 350.347939 366.0748 405.6864
[28,] 351.434918 367.6439 406.0944
[29,] 352.349803 369.0615 406.4373
[30,] 353.119864 370.3443 406.7256
[31,] 353.768046 371.5066 406.9680
[32,] 354.313656 372.5611 407.1718
[33,] 354.772940 373.5191 407.3431
[34,] 355.159568 374.3904 407.4872
[35,] 355.485044 375.1836 407.6083
[36,] 355.759048 375.9067 407.7101
[37,] 355.989728 376.5663 407.7958
[38,] 356.183940 377.1688 407.8678
[39,] 356.347453 377.7194 407.9283
[40,] 356.485125 378.2230 407.9792
[41,] 356.601043 378.6841 408.0221
[42,] 356.698648 379.1065 408.0581
[43,] 356.780834 379.4936 408.0884
[44,] 356.850041 379.8488 408.1138
[45,] 356.908319 380.1747 408.1353
[46,] 356.957395 380.4740 408.1533
[47,] 356.998725 380.7490 408.1684
[48,] 357.033531 381.0017 408.1812
[49,] 357.062844 381.2341 408.1919
[50,] 357.087532 381.4478 408.2009
[51,] 357.108325 381.6445 408.2085

> modelo.irf3

Impulse response coefficients
$Brasil
         México  Colombia   Brasil
 [1,]   0.00000   0.00000 178.9303
 [2,]  22.16509  39.71417 265.3638
 [3,]  49.08118  67.42943 304.9661
 [4,]  91.39646  94.76009 327.7933
 [5,] 134.21472 117.28597 341.1980
 [6,] 173.79679 136.11621 349.8671
 [7,] 207.86509 151.40010 355.7251
 [8,] 236.29569 163.74278 359.8940
 [9,] 259.54666 173.63742 362.9617
[10,] 278.35237 181.54551 365.2797
[11,] 293.45739 187.85094 367.0621
[12,] 305.53993 192.87202 368.4490
[13,] 315.17994 196.86692 369.5364
[14,] 322.85909 200.04373 370.3933
[15,] 328.97024 202.56915 371.0705
[16,] 333.83060 204.57636 371.6068
[17,] 337.69472 206.17148 372.0321
[18,] 340.76608 207.43902 372.3696
[19,] 343.20697 208.44620 372.6375
[20,] 345.14664 209.24648 372.8503
[21,] 346.68791 209.88236 373.0193
[22,] 347.91257 210.38759 373.1535
[23,] 348.88564 210.78901 373.2601
[24,] 349.65879 211.10797 373.3449
[25,] 350.27310 211.36138 373.4122
[26,] 350.76119 211.56273 373.4657
[27,] 351.14900 211.72271 373.5082
[28,] 351.45713 211.84982 373.5419
[29,] 351.70195 211.95082 373.5688
[30,] 351.89647 212.03106 373.5901
[31,] 352.05102 212.09481 373.6070
[32,] 352.17381 212.14547 373.6205
[33,] 352.27138 212.18572 373.6312
[34,] 352.34890 212.21770 373.6397
[35,] 352.41049 212.24310 373.6464
[36,] 352.45942 212.26329 373.6518
[37,] 352.49831 212.27933 373.6560
[38,] 352.52920 212.29207 373.6594
[39,] 352.55374 212.30220 373.6621
[40,] 352.57325 212.31025 373.6642
[41,] 352.58874 212.31664 373.6659
[42,] 352.60105 212.32172 373.6673
[43,] 352.61083 212.32575 373.6684
[44,] 352.61861 212.32896 373.6692
[45,] 352.62478 212.33150 373.6699
[46,] 352.62969 212.33353 373.6704
[47,] 352.63359 212.33514 373.6709
[48,] 352.63668 212.33641 373.6712
[49,] 352.63914 212.33743 373.6715
[50,] 352.64110 212.33824 373.6717
[51,] 352.64265 212.33888 373.6718


Lower Band, CI= 0.95 
$Brasil
          México  Colombia   Brasil
 [1,]   0.000000  0.000000 127.6563
 [2,] -29.324265  9.628094 167.9225
 [3,] -32.773578 29.457645 160.5129
 [4,]  -1.980392 35.001295 145.8526
 [5,]  14.208797 40.886192 135.9180
 [6,]  30.954337 47.691376 130.3251
 [7,]  44.448656 51.497253 124.6832
 [8,]  52.553517 53.594126 120.6906
 [9,]  57.215160 55.250856 117.8655
[10,]  61.956578 56.729799 115.8591
[11,]  67.774119 56.315240 108.2831
[12,]  72.275662 56.015239 102.9267
[13,]  76.064919 55.971999 102.6582
[14,]  79.042629 56.055112 102.9885
[15,]  81.849680 56.173612 103.1187
[16,]  82.860708 56.277888 103.1147
[17,]  82.958454 56.349498 103.0423
[18,]  83.108742 56.388529 104.0423
[19,]  83.271366 56.403227 105.4921
[20,]  83.418391 56.403356 106.6762
[21,]  83.535799 56.396891 107.5557
[22,]  83.620559 56.389024 108.1407
[23,]  83.676375 56.382427 108.4716
[24,]  83.709914 56.377993 108.5962
[25,]  83.728181 56.375604 108.5863
[26,]  83.737088 56.374717 108.4919
[27,]  83.740943 56.374728 108.3464
[28,]  83.742496 56.375149 108.1886
[29,]  83.743271 56.375661 108.0408
[30,]  83.743961 56.376096 107.9163
[31,]  83.744771 56.376393 107.8207
[32,]  83.745676 56.376558 107.7544
[33,]  83.746577 56.376622 107.7143
[34,]  83.747383 56.376624 107.6954
[35,]  83.748039 56.376597 107.6922
[36,]  83.748532 56.376562 107.6993
[37,]  83.748875 56.376532 107.7122
[38,]  83.749099 56.376511 107.7273
[39,]  83.749236 56.376499 107.7420
[40,]  83.749314 56.376494 107.7549
[41,]  83.749357 56.376493 107.7651
[42,]  83.749380 56.376495 107.7725
[43,]  83.749393 56.376498 107.7772
[44,]  83.749402 56.376500 107.7796
[45,]  83.749410 56.376501 107.7804
[46,]  83.749416 56.376502 107.7800
[47,]  83.749422 56.376503 107.7789
[48,]  83.749426 56.376503 107.7775
[49,]  83.749430 56.376503 107.7761
[50,]  83.749433 56.376502 107.7748
[51,]  83.749435 56.376502 107.7737


Upper Band, CI= 0.95 
$Brasil
         México  Colombia   Brasil
 [1,]   0.00000   0.00000 195.2325
 [2,]  76.88134  61.74727 300.0224
 [3,] 104.26156  94.37985 346.8604
 [4,] 162.15953 129.28987 381.9732
 [5,] 207.43355 157.40183 407.5519
 [6,] 259.86281 181.87677 430.2814
 [7,] 301.92482 203.53252 442.2178
 [8,] 338.93991 222.02575 453.6146
 [9,] 366.77729 235.60460 467.4151
[10,] 381.76655 250.39203 479.9511
[11,] 401.39099 261.16477 494.5031
[12,] 423.00750 270.17232 509.0309
[13,] 442.63323 275.56244 522.0960
[14,] 453.99123 280.58677 533.8603
[15,] 462.01206 284.70722 538.6823
[16,] 468.41967 288.28055 542.3567
[17,] 473.53879 291.72979 545.4566
[18,] 477.63096 295.23883 548.0792
[19,] 480.90393 300.13175 550.3035
[20,] 483.52375 304.50223 552.1942
[21,] 485.62233 308.58784 553.8045
[22,] 487.30484 312.25390 555.1784
[23,] 488.65492 313.68296 556.3525
[24,] 489.74187 314.78326 557.3571
[25,] 490.64708 315.69127 558.2178
[26,] 491.59829 316.73270 558.9560
[27,] 493.13387 319.62548 559.5897
[28,] 494.47332 322.42620 560.1340
[29,] 495.61095 325.15884 560.6021
[30,] 496.72269 327.71502 561.0047
[31,] 497.97185 330.19910 561.3512
[32,] 499.04716 334.07397 561.6496
[33,] 499.97281 337.72510 561.9066
[34,] 500.76964 341.16540 562.1281
[35,] 501.45559 344.40708 562.3190
[36,] 502.04609 347.46161 562.4836
[37,] 502.55443 350.33980 562.6256
[38,] 502.99204 353.05185 562.7481
[39,] 503.36878 355.14110 562.8537
[40,] 503.69310 356.82038 562.9449
[41,] 503.97231 358.38564 563.0236
[42,] 504.21268 359.84482 563.0915
[43,] 504.41962 361.20528 563.1501
[44,] 504.59778 362.47386 563.2007
[45,] 504.75117 363.65693 563.2443
[46,] 504.88322 364.76038 563.2820
[47,] 504.99691 365.78971 563.3146
[48,] 505.09480 366.75001 563.3427
[49,] 505.17907 367.64603 563.3670
[50,] 505.25163 368.48216 563.3879
[51,] 505.31410 369.26252 563.4060
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/e022311e-39a8-4021-bd45-c5345fd8433e)


![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/fe815c09-baf5-4c8c-bde5-13640d734392)


![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/49606d96-7a36-4b63-800f-f4d31ca42f74)



```r
Desc_var<-fevd(vec_modelo, n.ahead=10)
Desc_var
```

| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |
|-------------------------------------------------------------------------------|

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
