# Aplicando las pruebas DF y ADF en R

Para llevar a cabo la prueba ADF ofrecemos tres opciones:

## 1) **Primera Opción:** Utilice el comando **unitrootTest**
La estructura para hacer la prueba ADF es:
``` r
unitrootTest(x, lags = 1, type = c("nc", "c", "ct"), title = NULL, description = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                     | 
|-------------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                               |
| **lags**                | el número máximo de rezagos utilizados en la prueba (cuando lags = 1, estamos en el caso especial de una prueba DF) |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                  |
|                         |  **"nc"** para una regresión sin intercepto (constante) ni tendencia temporal                                       |
|                         |  **"c"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**    |
|                         |  **"ct"** para una regresión con intercepto (constante) y con tendencia temporal                                    |
| **title**               | cadena de caracteres que permite darle un título a la prueba                                                        |
| **description**         | cadena de caracteres que permite una breve descripción                                                              | 

``` r
unitrootTest(x, lags = 5, type = c("nc"), title = "Modelo de Paseo Aleatorio", description = NULL)
unitrootTest(x, lags = 5, type = c("c"), title = "Modelo de Paseo Aleatorio con Intercepto", description = NULL)
unitrootTest(x, lags = 5, type = c("ct"), title = "Modelo de Paseo Aleatorio con Intercepto y Tendencia Lineal", description = NULL)
```

## 2) **Segunda Opción:** Utilice el comando **urdfTest**
La estructura para hacer la prueba ADF es:
``` r
urdfTest(x, lags = 1, type = c("nc", "c", "ct"), doplot = TRUE)
```

| **Argumentos**          | **Descripción**                                                                                                     | 
|-------------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                               |
| **lags**                | el número máximo de rezagos utilizados en la prueba (cuando lags = 1, estamos en el caso especial de una prueba DF) |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                  |
|                         | **"nc"** para una regresión sin intercepto (constante) ni tendencia temporal                                        |
|                         | **"c"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**     |
|                         | **"ct"** para una regresión con intercepto (constante) y con tendencia temporal                                     |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                 | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico **_Opción Predeterminada_**                                          |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                  |

## 3) **Tercera Opción:** Utilice el comando **ur.df**
``` r
ur.df(x, type = c("none", "drift", "trend"), lags = 1, selectlags = c("Fixed", "AIC", "BIC"))
```

| **Argumentos**          | **Descripción**                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                |
| **lags**                | el número máximo de rezagos utilizados en la prueba  (cuando lags = 1, estamos en el caso especial de una prueba DF)                 |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                                   |
|                         | **"none"** para una regresión sin intercepto (constante) ni tendencia temporal                                                       |
|                         | **"drift"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                  |
|                         | **"trend"** para una regresión con intercepto (constante) y con tendencia temporal                                                   |
| **selectlags**          | Método escogido para la selección del número de rezagos:                                                                             | 
|                         | **"Fixed"** el número de rezagos establecidos en la opción "lags" - **_Opción Predeterminada_**                                      |
|                         | **"AIC"** criterio de selección de Akaike (el número máximo de rezagos analizados se establece en la opción "**lags**")              |
|                         | **"BIC"** criterio de selección Bayesiano de Schwartz (el número máximo de rezagos analizados se establece en la opción "**lags**")  |

#### De las tres opciones, mi preferida es esta última por la opción "selectlags" ya que permite utilizar los criterios de selección de Akaike y el Bayesiano de Schwartz para escoger la prueba apropiada. 
#### Posteriormente, se puede utilizar cualqueiera de las otras dos opciones para aprovechar el comando "doplot"

| [Retornar: 02. Pruebas de Raíz Unitaria](../Readme.md) | [02. Pruebas de DF y ADF](../Seccion02_02_ADF_T/Readme.md)  |
|--------------------------------------------------------|-------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
