<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.3.1. (R): 
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
La parsimonia se evalua utilizando el _Criterio de Información de Akaike_ (**AIC**) y el _Criterio de Información de Schwartz_ (**BIC**). Para ello, se utilizan los siguientes comandos:

``` r
AIC(nombre1,nombre2)
BIC(nombre1,nombre2)
```
Por otro lado, el comando **auto.arima** (a partir de la estimación implicita de múltiples modelos $ARIMA$) permite calcular automáticamente cuál es el modelo $ARIMA$ más parsimonioso y muestra la estimación del mismo:
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
|                      | **TRUE** La buscueda del mejor $ARIMA$ se hace con un algoritmo que la acelera (**Opción Predeterminada**)                                                |
|                      | **FALSE** La busqueda se hace en todos los modelos                                                                                                       |
| **approximation**    | Argumento lógico que sólo tiene dos opciones:                                                                                                            |
|                      | **TRUE** La buscueda del mejor $ARIMA$ se hace con un algoritmo que la acelera (**Opción Predeterminada**)                                                |
|                      | **FALSE** La busqueda se hace de forma normal                                                                                                            |

[^2]: **Si no hay inconvenientes computacionales en el calculo de los diferentes modelos _ARIMA_, se recomienda utilizar las opciones **stepwise = FALSE, approximation = FALSE** para un calculo más preciso del modelo más parsimonioso. Por otro lado, el comando _auto.arima_ tiene aún más argumentos que los presentados. Sin embargo los principales son los que se estan presentando.**

### Pruebas sobre los residuos estimados

Asuma, que el modelo más parsimonioso es el modelo que se llama **nombre1**. Sobre ese modelo se pueden a hacer múltiples pruebas de ruido blanco a los residuos estimados.

#### a) Visualización de la $FAC$ y la $FACP$

Los comandos
``` r
autoplot(acf(nombre1$residuals, plot = FALSE))
autoplot(pacf(nombre1$residuals, plot = FALSE))
```
Permiten visualizar la $FAC$ y la $FACP$ de los residuos del modelo **nombre1**

#### b) Visualización de los residuos estimados y de sus _p-value_ del estadistico $Q$ de Ljung-Box

El comando,
``` r
ggtsdiag(nombre1, gof.lag = 15)
```
gráfica los residuos estimados estándarizados, la $FAC$ de los residuos y los _p-values_ del estadistico $Q$ de Ljung-Box de los residuos estimados para diferentes opciones de rezagos (el número de rezagos en este último gráfico se determina en la opción **gof.lag**, la **opción preseterminada** es de $10$ rezagos).

Si se desea determinar con precisión el valor de algún _p-value_ del estadistico $Q$ de Ljung-Box de los residuos estimados para algún rezago en particular, copie los comandos: 

``` r
lb <- Box.test(nombre1$residuals, lag=10, type="Ljung-Box") 
lb$p.value
```

# Pronósticos
Para hacer pronósticos y gráficarlos, utilice el comando _forecast_ así [^3]:
``` r
forecast1<-forecast(nombre1, level = c(95), h = 3)
summary(forecast1)
autoplot(forecast1)
```

| **Argumentos**       | **Descripción**                                                                                                                                          | 
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nombre1**          | nombre del modelo $ARIMA$ para el cual se desean hacer pronósticos                                                                                       |
| **level**            | valor para determinar el tamaño de la banda de confianza de los pronósticos                                                                              |
| **h**                | número de periodos que se desean pronósticar                                                                                                             |

Observe que los pronósticos se trabajaron con el nombre **forecast1**. Usted puede escoger un nombre diferente.

[^3]: **El comando _forecast_ pertenece a la librería _forecast_**

---
---
# Preguntas de selección múltiple

1. **¿Cuál de los comandos vistos en esta sección permite utilizar el _Criterio de Información de Akaike_ con corrección para muestras pequeña?:**
 
   a) _autoplot_.

   b) _auto.arima_.

   c) _Arima_.

   d) _forecast_.

2. **¿Con cuál comando se puede graficar la Función de Autocorrelación de los residuos ?:**
 
   a) _autoplot_.

   b) _ggtsdiag_.

   c) Todos los anteriores.

   d) Ninguno de los anteriores.

---
---
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Subsección 2.3. Análisis ARMA (La Metodología de Box-Jenkins)](../Readme.md) | [Subsección 2.3.1.(T) Explicación general de las tres etapas de la metodología de Box-Jenkins](../Seccion02_03_01_T/Readme.md) |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
