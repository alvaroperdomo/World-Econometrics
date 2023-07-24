<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.2.2. (R)
# Aplicando las pruebas $ADF-GLS$ en $R$

Para llevar a cabo la prueba $ADF-GLS$ ofrecemos dos opciones:

## 1) Primera Opción:** Utilice el comando **unitrootTest** [^1]
La estructura para hacer la prueba $ADF-GLS$ es:
``` r
urersTest(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"), lag.max = 4,)
```
[^1]: **Este comando pertenece a la librería _fUnitRoots_**

| **Argumentos**          | **Descripción**                                                                                                                                          | 
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                       |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                     |
|                         | el número máximo de rezagos utilizados para, con el Criterio Bayesiano de Schwartz, determinar el número óptimo de rezagos de la $prueba P$ |   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para $DF-GLS$                                          |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                               | 
|                         | **"constant"** Se refiere al modelo con constante pero sin tendencia (**_Opción Predeterminada_**)                                          |
|                         | **"trend"** Se refiere al modelo con constante y tendencia                                                                                  |     

## 2) Segunda Opción: Utilice el comando ur.ers[^2]
La estructura para hacer la prueba $ADF-GLS$ es:
``` r
ur.ers(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"),lag.max = 4)
```
[^2]: **Este comando pertenece a la librería _urca_**

| **Argumentos**          | **Descripción**                                                                                                                                          | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                                    |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                                  |
|                         | el número máximo de rezagos utilizados para probar el truncamiento del rezago decendente para la $prueba P$, utilizando el Criterio Bayesiano de Schwartz|   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para $DF-GLS$                                                       |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                                                       |
|                         | **"DF-GLS"** - **_Opción Predeterminada_**                                                                                                               |
|                         | **"P-test"**[^3]                                                                                                                                         |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                            | 
|                         | **"constant"** Se refiere al modelo con constante pero sin tendencia (**_Opción Predeterminada_**)                                                       |
|                         | **"trend"** Se refiere al modelo con constante y tendencia                                                                                               |

[^3]: **La prueba de punto óptimo factible (_P-test_) tiene en cuenta la correlación serial del término de error y haya el número óptimo de rezagos utilizando el Criterio Bayesiano de Schwartz.** 

#### De las dos opciones, mi preferida es ésta última por la opción "type($P-test$)" que permite obtener el número óptimo de rezagos utilizando el Criterio Bayesiano de Schwartz. 

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
