# Aplicando la pruebas ZA en R

Para llevar a cabo la prueba ZA ofrecemos dos opciones:

## 1) **Primera Opción:** Utilice el comando **urzaTes**
``` r
urzaTest(x, model = c("intercept", "trend", "both"), lag, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                    |
| **model**               | en esta opción se especifica el tipo de prueba ZA que se va a llevar a cabo. En particular, se especifica si cambio estructural ocurre en:   | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el número máximo de rezagos utilizados en la prueba                                                                                          |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar el gráfico de diagnostico - **_Opción Predeterminada_**                                                              |
|                         | **"FALSE"** para no mostrar el gráfico de diagnostico                                                                                        |

## 2) **Segunda Opción:** Utilice el comando **ur.za*
``` r
ur.za(x, model = c("intercept", "trend", "both"), lag=NULL)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                    |
| **model**               | en esta opción se especifica el tipo de prueba ZA que se va a llevar a cabo. En particular, se especifica si cambio estructural ocurre en:   | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el número máximo de rezagos utilizados en la prueba                                                                                          |

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
