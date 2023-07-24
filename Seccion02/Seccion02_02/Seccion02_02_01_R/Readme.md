# Aplicando las pruebas $DF$ y $ADF$ en $R$

Para llevar a cabo la prueba $ADF$ en $R$ existen las siguientes cuatro opciones:

## 1) Primera Opción:** Utilice el comando _unitrootTest_ [^1]

[^1]: **Este comando pertenece a la librería _fUnitRoots_**

La estructura para hacer la prueba $ADF$ es:
``` r
unitrootTest(x, lags = 1, type = c("nc", "c", "ct"), title = NULL, description = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                       | 
|-------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                             |
| **lags**                | el número máximo de rezagos utilizados en la prueba (cuando lags = 1, estamos en el caso especial de una prueba $DF$) |
| **type**                | en esta opción se especifica el tipo de prueba $ADF$ que se va a llevar a cabo. Las opciones válidas son:             |
|                         |  **"nc"** para una prueba sin intercepto (constante) ni tendencia temporal                                            |
|                         |  **"c"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**         |
|                         |  **"ct"** para una prueba con intercepto (constante) y con tendencia temporal                                         |
| **title**               | cadena de caracteres que permite darle un título a la prueba                                                          |
| **description**         | cadena de caracteres que permite una breve descripción                                                                | 

Algunos ejemplos de cómo aplicar la prueba $ADF$ con esta opción son:
``` r
unitrootTest(x, lags = 5, type = c("nc"), title = "Modelo de Paseo Aleatorio", description = NULL)
unitrootTest(x, lags = 5, type = c("c"), title = "Modelo de Paseo Aleatorio con Intercepto", description = NULL)
unitrootTest(x, lags = 5, type = c("ct"), title = "Modelo de Paseo Aleatorio con Intercepto y Tendencia Lineal", description = NULL)
```

## 2) Segunda Opción: Utilice el comando _urdfTest_ [^2]

[^2]: **Este comando pertenece a la librería _fUnitRoots_**

La estructura para hacer la prueba $ADF$ es:
``` r
urdfTest(x, lags = 1, type = c("nc", "c", "ct"), doplot = TRUE)
```

| **Argumentos**          | **Descripción**                                                                                                       | 
|-------------------------|-----------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                             |
| **lags**                | el número máximo de rezagos utilizados en la prueba (cuando lags = 1, estamos en el caso especial de una prueba $DF$) |
| **type**                | en esta opción se especifica el tipo de prueba $ADF$ que se va a llevar a cabo. Las opciones válidas son:             |
|                         | **"nc"** para una prueba sin intercepto (constante) ni tendencia temporal                                             |
|                         | **"c"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**          |
|                         | **"ct"** para una prueba con intercepto (constante) y con tendencia temporal                                          |
| **doplot**              | Indicador lógico, para especificar si se desea obtener un gráfico de diagnóstico sobre los residuos estimados         | 
|                         | **"TRUE"** para mostrar gráfico de diagnóstico **_Opción Predeterminada_**                                            |
|                         | **"FALSE"** para no mostrar gráfico de diagnóstico                                                                    |

## 3) Tercera Opción: Utilice el comando _ur.df_ [^3]

[^3]: **Este comando pertenece a la librería _urca_**

La estructura para hacer la prueba $ADF$ es:
``` r
ur.df(x, type = c("none", "drift", "trend"), lags = 1, selectlags = c("Fixed", "AIC", "BIC"))
```

| **Argumentos**          | **Descripción**                                                                                                                        | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                              |
| **lags**                | el número máximo de rezagos utilizados en la prueba  (cuando lags = 1, estamos en el caso especial de una prueba $DF$)                 |
| **type**                | en esta opción se especifica el tipo de prueba $ADF$ que se va a llevar a cabo. Las opciones válidas son:                              |
|                         | **"none"** para una prueba sin intercepto (constante) ni tendencia temporal                                                            |
|                         | **"drift"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                       |
|                         | **"trend"** para una prueba con intercepto (constante) y con tendencia temporal                                                        |
| **selectlags**          | Método escogido para la selección del número de rezagos:                                                                               | 
|                         | **"Fixed"** el número de rezagos establecidos en la opción "lags" - **_Opción Predeterminada_**                                        |
|                         | **"AIC"** criterio de información de Akaike (el número máximo de rezagos analizados se establece en la opción "**lags**")              |
|                         | **"BIC"** criterio Bayesiano de Schwartz (el número máximo de rezagos analizados se establece en la opción "**lags**")                 |

## 4) Cuarta Opción: Utilice el comando _ndiffs_ [^4]

[^4]: **Este comando pertenece a la librería _forecast_**

Este comando permite estimar el número de diferencias necesarias para hacer estacionaria una serie temporal determinada. **ndiffs** encuentra la menor cantidad de diferencias requeridas para fallar la hipótesis nula de la prueba $ADF$ según el nivel de significancia **alpha**. La estructura para hallar el número de diferencias con la prueba $ADF$ es:
``` r
ndiffs(x, alpha = 0.05, test = c("adf"), type = c("level", "trend"), ...)
```

| **Argumentos**          | **Descripción**                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                            |
| **alpha**               | Nivel de significancia de la prueba, los valores posibles oscilan entre 0.01 y 0.1.                                                  |
| **type**                | en esta opción se especifica el tipo de prueba $ADF$ que se va a llevar a cabo. Las opciones válidas son:                            |
|                         | **"level"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                     |
|                         | **"trend"** para una regresión con intercepto (constante) y con tendencia temporal                                                   |

#### De las cuatro opciones, mi preferida es la tercera por la opción "selectlags" porque permite utilizar el _Criterio de Información de Akaike_ y el _Criterio Bayesiano de Schwartz_ para escoger la prueba apropiada. 
#### Posteriormente, se puede utilizar la segunda opción para aprovechar el comando "doplot" y así hacer la prueba sobre los residuos.
#### Por último, se pueden contrastar los resultados con la cuarta opción para tener una opinión adicional.

---
---
# Preguntas de selección múltiple

1. **¿Cuál argumento y en cuál comando le permite hacer gráficos de diagnóstico sobre los residuos estimados?:**
 
   a) El argumento "_doplot_" en el comando "_ndiffs_".

   b) El argumento "_plot_" en el comando "_ndiffs_".

   c) El argumento "_plot_" en el comando "_ur.df_".

   d) El argumento "_doplot_" en todos los comandos.

2. **¿Cuál comando permite escoger el número óptimo de rezagos segun el Criterio de Información de Akaike?:**
 
   a) _unitrootTest_.

   b) _urdfTest_.

   c) _ur.df_.

   d) Todos los anteriores.

---
---
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>


| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) | [2.2.1.(T) Explicación general de las pruebas _DF_ y _ADF_](../Seccion02_02_01_T/Readme.md)  |
|---------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------------------------|
