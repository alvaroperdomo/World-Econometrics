<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.2.2. (R)
# Aplicando las pruebas $ADF-GLS$ en $R$

Para llevar a cabo la prueba $ADF-GLS$ ofrecemos dos opciones:

## 1) Primera Opción:** Utilice el comando **urersTest** [^1]
La estructura para hacer la prueba $ADF-GLS$ es:
``` r
urersTest(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"), lag.max = 4, doplot = TRUE)
```
[^1]: **Este comando pertenece a la librería _fUnitRoots_**

| **Argumentos**          | **Descripción**                                                                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                                                            |
| **type**                | Tipo de prueba que se va a hacer. Las opciones válidas son:                                                                                                                          |
|                         | **"DF-GLS"** - prueba ADF-GLS estándar **_Opción Predeterminada_**                                                                                                                   |
|                         | **"P-test"** - prueba de punto óptimo factible                                                                                                                                       |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                                                        | 
|                         | **"constant"** Se refiere al modelo con constante pero sin tendencia (**_Opción Predeterminada_**)                                                                                   |
|                         | **"trend"** Se refiere al modelo con constante y tendencia                                                                                                                           |     
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                                                              |
|                         | el número máximo de rezagos utilizados con la opción type = c("P-test"), para escoger el número óptimo de rezagos de forma descendente con banse en el Criterio Bayesiano de Schwartz|   
|                         | el número máximo de diferencias rezagadas que se incluirán en la prueba $ADF-GLS$ que opera con el argumento type = c("DF-GLS")                                                      |
| **doplot**              | Indicador lógico, para especificar si se desea obtener un gráfico de diagnóstico sobre los residuos estimados                                                                        | 
|                         | **"TRUE"** para mostrar gráfico de diagnóstico **_Opción Predeterminada_**                                                                                                           |
|                         | **"FALSE"** para no mostrar gráfico de diagnóstico                                                                                                                                   |

## 2) Segunda Opción: Utilice el comando ur.ers[^2]
La estructura para hacer la prueba $ADF-GLS$ es:
``` r
urersTest(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"),lag.max = 4, doplot = TRUE)
```
[^2]: **Este comando pertenece a la librería _urca_**

| **Argumentos**          | **Descripción**                                                                                                                                                                      | 
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                                                            |
| **type**                | Tipo de prueba que se va a hacer. Las opciones válidas son:                                                                                                                          |
|                         | **"DF-GLS"** - prueba ADF-GLS estándar **_Opción Predeterminada_**                                                                                                                   |
|                         | **"P-test"** - prueba de punto óptimo factible                                                                                                                                       |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                                                        | 
|                         | **"constant"** Se refiere al modelo con constante pero sin tendencia (**_Opción Predeterminada_**)                                                                                   |
|                         | **"trend"** Se refiere al modelo con constante y tendencia                                                                                                                           |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                                                              |
|                         | el número máximo de rezagos utilizados con la opción type = c("P-test"), para escoger el número óptimo de rezagos de forma descendente con banse en el Criterio Bayesiano de Schwartz|   
|                         | el número máximo de diferencias rezagadas que se incluirán en la prueba $ADF-GLS$ que opera con el argumento type = c("DF-GLS")                                                      |

La prueba $ADF-GLS$  que se explicó en la subsección 2.2.2.(T) es el argumento que en **_urersTest_** y en _**ur.ers**_ aparece denotado como type = c("DF-GLS"). Sin embargo, Elliot, Rothenberg y Stock (1996) también propusieron una prueba de punto óptimo factible, que en los argumentos de **_urersTest_** y  _**ur.ers**_ aparece denotada como type = c("P-test"). La lectura de esta prueba es la misma que la de la prueba $ADF-GLS$: si el estadístico  de la prueba es superior al valor crítico, entonces no se rechaza la hipótesis nula de raíz unitaria, en caso contrario se rechaza la hipótesis de raíz unitaria.

De los dos comandos, comenzaría utilizando el comando **_urersTest_** con los siguientes argumentos fijos
* type = c("DF-GLS"),
* doplot("TRUE"),
* lag.max = 1, 

el resto de argumentos se escogen de acuerdo al contexto. La idea es poder visualizar las pruebas gráficas sobre los residuos estimados. Si no hay problemas de autocorrelación en los residuos estimados, entonces este es el número de rezagos con el que hago la prueba. Si hay problema de autocorrelación en los residuos estimados, entonces aumento el valor de lagmax, talque lagmax=2. Reviso de nuevo las gráficas de los residuos estimados.  Si no hay problemas de autocorrelación en los residuos estimados, entonces este es el número de rezagos con el que hago la prueba. Si hay problema de autocorrelación en los residuos estimados, entonces continuo subiendo el número de rezagos hasta que los residuos estimados no estén autocorelacionados.

Luego, con el comando _**ur.ers**_ hago la estimación utilizando la opción type = c("DF-GLS") e incorporando el número de rezagos encontrados previamente porque el comando _**ur.ers**_ muestra información numérica más ilustrativa que el comando **_urersTest_**. 

Finalmente, con el comando utilizaría el comando _**ur.ers**_ con type = c("P-test") para apreciar si los resultados se mantienen.

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


| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) | [2.2.2.(T) Explicación general de la pruebas ADF-GLS](../Seccion02_02_02_T/Readme.md)  |
|---------------------------------------------------------|--------------------------------------|----------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
