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

Después de estimar uno o más modelos Arima, por ejemplo:

**Parsimonia**
``` r
nombre1<- Arima(PIBpc, order=c(1,1,1))  
nombre2<- Arima(PIBpc, order=c(0,1,1))
```
Se pueden utilizar los comandos 
``` r
AIC(nombre1,nombre2)
BIC(nombre1,nombre2)
```
Para, con el _Criterio de Información de Akaike_ y/o el _Criterio de Información de Schwartz_, determinar cuál es el modelo más parsimonioso

**Pruebas sobre los errores de la estimación**

Asuma, que el modelo más parsimonioso es el modelo que se llama **nombre1**, y sobre ese modelo se van a hacer las pruebas sobre los errores.

Los comandos
``` r
autoplot(acf(nombre1$residuals, plot = FALSE))
autoplot(pacf(nombre1$residuals, plot = FALSE))
```
Permiten visualizar la $FAC$ y la $FACP$ de los residuos del modelo **nombre1**

El comando,
``` r
ggtsdiag(nombre1)
```
Gráfica los residuos estándarizados, la $FAC$ de los residuos y los p-values del estadistico Q de Ljung-Box para diferentes opciones de rezagos.
