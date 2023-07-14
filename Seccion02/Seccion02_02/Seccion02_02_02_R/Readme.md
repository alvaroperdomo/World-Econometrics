## SECCIÓN 2.2.2. (R)
# Aplicando las pruebas ADF-GLS en R

Para llevar a cabo la prueba ADF-GLS ofrecemos dos opciones:

## 1) **Primera Opción:** Utilice el comando **unitrootTest**
La estructura para hacer la prueba ADF-GLS es:
``` r
urersTest(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"), lag.max = 4, doplot = TRUE)
```

Para mejorar el poder de la prueba de raíz unitaria, Elliot, Rothenberg y Stock (1996) propusieron la eliminación de tendencia local de la serie de tiempo.  Para ello, desarrollaron una prueba de punto óptimo factible, **"P-test"**, que tiene en cuenta la correlación serial del término de error. El segundo tipo de prueba es la prueba "DF-GLS", que es una prueba de tipo ADF.

| **Argumentos**          | **Descripción**                                                                                                                                          | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                                    |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                                  |
|                         | el número máximo de rezagos utilizados para probar el truncamiento del rezago decendente para la "prueba P", utilizando el método Bayesiano de Schwartz  |   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para "DF-GLS"                                                       |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                            | 
|                         | **"constant"** - **_Opción Predeterminada_**                                                                                                             |
|                         | **"trend"**                                                                                                                                              |
| **lag.max**             | el número máximo de rezagos utilizados en la prueba                                                                                                      |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                                      | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico **_Opción Predeterminada_**                                                                               |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                                       |

## 2) **Segunda Opción:** Utilice el comando **ur.ers**
La estructura para hacer la prueba ADF-GLS es:
``` r
ur.ers(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"),lag.max = 4)
```

| **Argumentos**          | **Descripción**                                                                                                                                          | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                                    |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                                  |
|                         | el número máximo de rezagos utilizados para probar el truncamiento del rezago decendente para la "prueba P", utilizando el método Bayesiano de Schwartz  |   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para "DF-GLS"                                                       |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                                                       |
|                         | **"DF-GLS"** - **_Opción Predeterminada_**                                                                                                               |
|                         | **"P-test"**                                                                                                                                             |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                            | 
|                         | **"constant"** - **_Opción Predeterminada_**                                                                                                             |
|                         | **"trend"**                                                                                                                                              |

#### De las dos opciones, mi preferida es esta última por la opción "type("P-test") que permite obtener esta información.
#### Posteriormente, se puede utilizar cla primera opción para aprovechar el comando "doplot"

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
