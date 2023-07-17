## SECCIÓN 2.2.3. (R)
# Aplicando la prueba $KPSS$ en $R$

Para llevar a cabo la prueba $KPSS$ ofrecemos tres opciones:

## 1) Primera Opción: Utilice el comando _unitrootTest_ [^1]

[^1]: **Este comando pertenece a la librería _fUnitRoots_**

``` r
urkpssTest(x, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** - **_Opción Predeterminada_**                                                                                                       |
|                         | **"tau"**                                                                                                                                    |
| **lags**                | el máximo número de rezagos                                                                                                                  |
| **use.lag**             | cadena de caracteres que especifica el número de rezagos                                                                                     |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico - **_Opción Predeterminada_**                                                                 |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

## 2) Segunda Opción: Utilice el comando _ur.kpss_ [^2]

[^2]: **Este comando pertenece a la librería _urca_**

``` r
ur.kpss(y, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** - **_Opción Predeterminada_**                                                                                                       |
|                         | **"tau"**                                                                                                                                    |
| **lags**                | el máximo número de rezagos utilizado en la corrección del error                                                                             |
|                         | **"short"** establece el número de rezagos de acuerdpoa  √[4]{4 \times (n/100)}                                                              |
|                         | **"longe"** establece el número de rezagos de acuerdpoa  √[4]{12 \times (n/100)}                                                             |
|                         | **"nil"** establece que no se hace ninguna corrección del error                                                                              |
| **use.lag**             | número de rezagos especificados por el usuario                                                                                               |

## 3) **Tercera Opción:** Utilice el comando **ndiffs**
Este comando permite estimar el número de diferencias necesarias para hacer estacionaria una serie temporal determinada. **ndiffs** encuentra la menor cantidad de diferencias requeridas para no rechazar la hipótesis nula de la prueba KPSS según el nivel de significancia alpha. La estructura para hallar el número de diferencias con la prueba KPSS es:
``` r
ndiffs(x, alpha = 0.05, test = c("kpss"), type = c("level", "trend"), ...)
```

| **Argumentos**          | **Descripción**                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                            |
| **alpha**               | Nivel de significancia dela prueba, los valores posibles oscilan entre 0,01 y 0,1.                                                   |
| **type**                | en esta opción se especifica el tipo de prueba ADF que se va a llevar a cabo. Las opciones válidas son:                              |
|                         | **"level"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                     |
|                         | **"trend"** para una regresión con intercepto (constante) y con tendencia temporal                                                   |

#### De las cuatro opciones, mi preferida es la tercera por la opción "selectlags" ya que permite utilizar los criterios de selección de Akaike y el Bayesiano de Schwartz para escoger la prueba apropiada. 
#### Posteriormente, se puede utilizar la segunda opción para aprovechar el comando "doplot" y así hacer la prueba sobre los residuos.

#### De las dos opciones, mi preferida es esta última porque la opción "lags" da varias recomendaciones para escoger los rezagos.
#### Posteriormente, se puede utilizar cla primera opción para aprovechar el comando "doplot"
#### Por último, se puede contrastar con la cuarta opción para tener una opinión adicional.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) | [2.2.1.(T) Explicación general de la prueba _KPSS_](../Seccion02_02_03_T/Readme.md)  |
|---------------------------------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
