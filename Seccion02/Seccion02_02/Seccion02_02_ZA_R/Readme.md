# Aplicando la pruebas ZA en R

Para llevar a cabo la prueba ZA ofrecemos dos opciones:

## 1) **Primera Opción:** Utilice el comando **urzaTes**

``` r
urzaTest(x, model = c("intercept", "trend", "both"), lag, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **model**               | cadena de caracteres que especifica si cambio estructural ocurre en                                                                          | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el mayor número de variables diferenciadas endógenas rezagadas que se incluirán en la regresión de prueba.                                   |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico - **_Opción Predeterminada_**                                                                 |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

## 2) **Segunda Opción:** Utilice el comando **ur.za*
``` r
ur.za(x, model = c("intercept", "trend", "both"), lag=NULL)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **model**               | cadena de caracteres que especifica si cambio estructural ocurre en :                                                                        |
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el mayor número de variables diferenciales endógenas rezagadas que se incluirán en la regresión de prueba                                    |

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
