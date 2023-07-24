## SECCIÓN 2.2.3. (R)
# Aplicando la prueba $KPSS$ en $R$

Para llevar a cabo la prueba $KPSS$ ofrecemos tres opciones:

## 1) Primera Opción: Utilice el comando _urkpssTest_ [^1]

[^1]: **Este comando pertenece a la librería _fUnitRoots_**

``` r
urkpssTest(x, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                                |
|                         | **"tau"** para una prueba con intercepto (constante) y con tendencia temporal                                                                |
| **lags**                | el máximo número de rezagos                                                                                                                  |
| **use.lag**             | número de rezagos especificados por el usuario                                                                                               |

## 2) Segunda Opción: Utilice el comando _ur.kpss_ [^2]

[^2]: **Este comando pertenece a la librería _urca_**

``` r
ur.kpss(y, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **mu** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                                  |
|                         | **"tau"** para una prueba con intercepto (constante) y con tendencia temporal                                                                |
| **lags**                | el máximo número de rezagos utilizado en la corrección del error, se puede escoger cualquiera de las siguientes opciones:                    |
|                         | **"short"** establece el número de rezagos de acuerdo a $(4 \times \frac{n}{100})^{\frac{1}{4}}$                                             |
|                         | **"longe"** establece el número de rezagos de acuerdoa  $(12 \times \frac{n}{100})^{\frac{1}{4}}$                                            |
|                         | **"nil"** establece que no se hace ninguna corrección del error                                                                              |
| **use.lag**             | número de rezagos especificados por el usuario                                                                                               |

## 3) **Tercera Opción:** Utilice el comando **ndiffs**[^3]
Este comando permite estimar el número de diferencias necesarias para hacer estacionaria una serie temporal determinada. **ndiffs** encuentra la menor cantidad de diferencias requeridas para no rechazar la hipótesis nula de la prueba $KPSS$ según el nivel de significancia alpha. La estructura para hallar el número de diferencias con la prueba $KPSS$ es:

[^3]: **Este comando pertenece a la librería _forecast_**

``` r
ndiffs(x, alpha = 0.05, test = c("kpss"), type = c("level", "trend"), ...)
```

| **Argumentos**          | **Descripción**                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                            |
| **alpha**               | Nivel de significancia de la prueba, los valores posibles oscilan entre 0,01 y 0,1.                                                  |
| **type**                | en esta opción se especifica el tipo de prueba ADF que se va a llevar a cabo. Las opciones válidas son:                              |
|                         | **"level"** para una prueba con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**                     |
|                         | **"trend"** para una regresión con intercepto (constante) y con tendencia temporal                                                   |

#### De las tres opciones, mi preferida es la segunda porque reporta mejor información que la primera. Después, se puede contrastar con la tercera opción para tener una opinión adicional.


---
---
# Preguntas de selección múltiple

1. **¿Cuál comando admite escoger la opción type($ARIMA$)?:**
 
   a) El comando "unitrootTest".

   b) El comando "ur.ers".

   c) Todos los anteriores.

   d) Ninguno de los anteriores.

2. **¿Cuál comando admite escoger la opción lag.max?:**
 
   a) El comando "unitrootTest".

   b) El comando "ur.ers".

   c) Todos los anteriores.

   d) Ninguno de los anteriores.

---
---
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) | [2.2.1.(T) Explicación general de la prueba _KPSS_](../Seccion02_02_03_T/Readme.md)  |
|---------------------------------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
