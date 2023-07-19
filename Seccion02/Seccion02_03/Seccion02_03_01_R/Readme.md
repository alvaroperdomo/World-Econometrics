## SECCIÓN 2.3.1.R. 
# Las tres etapas de la metodología de Box-Jenkins (Aplicación en $R$)

## 1) Identificación:

La forma para gráficar una serie { $y_t$ } en $R$ ya la vimos en la sección 1.2. con el comando **ggplot**. Entonces, vamos a pasar directamente a cómo hacer el gráfico de la $FAC$ y de la $FACP$. 

### Gráficos de las $FAC$ y $FACP$ muestrales
Para gráficar la función de autocorrelación $FAC$ y la función de autocorrelación parcial $FACP$ muestrales copie los siguientes comandos

``` r
autoplot(acf(x, plot = FALSE))
autoplot(pacf(x, plot = FALSE))
```

| **Argumentos**[^1]   | **Descripción**                                                                                                                                          | 
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                | vector o variable de series de tiempo                                                                                                                    |
| **plot**             | Argumento lógico que sólo tiene dos opciones:                                                                                                            |
|                      | **TRUE** Saca dos opciones de gráfico (**Opción Predeterminada**)                                                                                        |
|                      | **FALSE** Saca una sola opción de gráfico                                                                                                                |

[^1]: **El comando _autoplot_ tiene muchisimos más argumentos (buena parte de ellos estéticos), pero por lo pronto sólo nos interesan los dos que presentamos**


## 2) Estimación:
La estimación del modelo univariado se hace con el comando

``` r
Arima(x, order=c("p","I","q"))
```

| **Argumentos**            | **Descripción**                                                                                                                      | 
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **x**                     | variable a la que se le va a hacer la estimación $ARIMA$                                                                             |
| **order=c("p","I","q"))** | con este comado es escogen:                                                                                                          |
|                           | **p**: El número de componentes autorregresivos que tiene la estimación $ARIMA$                                                      |
|                           | **I**: El grado de integración de la estimación $ARIMA$                                                                              |
|                           | **q**: El número de componentes autorregresivos que tiene la estimación $ARIMA$                                                      |

## 3) Evaluación y diagnóstico:

### Parsimonia

Después de estimar uno o más modelos Arima, por ejemplo, utilizando los comandos:

``` r
nombre1<- Arima(PIBpc, order=c(1,1,1))  
nombre2<- Arima(PIBpc, order=c(0,1,1))
```
La parsimonia se puede evaluar con los comandos 
``` r
AIC(nombre1,nombre2)
BIC(nombre1,nombre2)
```
Para, con el _Criterio de Información de Akaike_ (**AIC**) y/o el _Criterio de Información de Schwartz_ (**BIC**), se puede determinar cuál es el modelo más parsimonioso.

Por otro lado, el comando **auto.arima** permite calcular automáticamente cuál es el modelo $ARIMA$ más parsimonioso:

``` r
auto.arima(x, ic = c("aicc", "aic", "bic"), stepwise = FALSE, approximation = FALSE)
```

| **Argumentos** [^2]  | **Descripción**                                                                                                                                          | 
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                | variable de series de tiempo que se desea evaluar                                                                                                        |
| **ic**               | criterio de información que se desea utilizar para determinar el mejor $ARIMA$. Las opciones son:                                                        |
|                      | **aicc** _Criterio de Información de Akaike_ con corrección para muestras pequeñas                                                                       |
|                      | **aic** _Criterio de Información de Akaike_                                                                                                              |
|                      | **bic** _Criterio Bayesiano de Schwartz_                                                                                                                 |
| **stepwise**         | Argumento lógico que sólo tiene dos opciones:                                                                                                            |
|                      | **TRUE** La buscueda del mejor $ARIMA$ se hace con un algoritmo que la acelera (**Opció Predeterminada**)                                                |
|                      | **FALSE** La busqueda se hace en todos los modelos                                                                                                       |
| **approximation**    | Argumento lógico que sólo tiene dos opciones:                                                                                                            |
|                      | **TRUE** La buscueda del mejor $ARIMA$ se hace con un algoritmo que la acelera (**Opció Predeterminada**)                                                |
|                      | **FALSE** La busqueda se hace de forma normal                                                                                                            |

[^2]: **Si no inconvenientes computacionales en el calculo de los diferentes modelos ARIMA, se recomienda utilizar las opciones **stepwise = FALSE, approximation = FALSE**. Por otro lado, el comando _auto.arima_ tiene aún más argumentos que los presentados. Sin embargo los principales son los que se estan presentando.**

### Pruebas sobre los residuos estimados

Asuma, que el modelo más parsimonioso es el modelo que se llama **nombre1**, y sobre ese modelo se van a hacer las pruebas sobre los residuos estimados.

Los comandos
``` r
autoplot(acf(nombre1$residuals, plot = FALSE))
autoplot(pacf(nombre1$residuals, plot = FALSE))
```
Permiten visualizar la $FAC$ y la $FACP$ de los residuos del modelo **nombre1**

El comando,

``` r
ggtsdiag(nombre1, gof.lag = 10)
```
^gráfica los residuos estándarizados, la $FAC$ de los residuos y los _p-values_ del estadistico $Q$ de Ljung-Box de los residuos estimados para diferentes opciones de rezagos (el número de rezagos en este gráfico se determina en la opción **gof.lag**, la **opción preseterminada** es de $10$ rezagos).

Si se desea determinar con precisión el valor de algún _p-value_ del estadistico $Q$ de Ljung-Box de los residuos estimados para algún rezago en particular, copie los comandos: 

``` r
lb <- Box.test(nombre1$residuals, lag=10, type="Ljung-Box") 
lb$p.value
```

