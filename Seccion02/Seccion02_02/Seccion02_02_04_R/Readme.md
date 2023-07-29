<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 2.2.4. (R)
# Aplicando la prueba $ZA$ en $R$

Para llevar a cabo la prueba $ZA$ ofrecemos dos opciones:

## 1) Primera Opción: Utilice el comando _urzaTest_[^1]

[^1]: **Este comando pertenece a la librería _fUnitRoots_**

``` r
urzaTest(x, model = c("intercept", "trend", "both"), lag, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                                | 
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                      |
| **model**               | en esta opción se especifica el tipo de prueba $ZA$ que se va a llevar a cabo. En particular, se especifica si cambio estructural ocurre en:   | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                      | 
|                         | **"trend**: tendencia                                                                                                                          |
|                         | **"both"**: ambos                                                                                                                              | 
| **lag**                 | el número máximo de rezagos utilizados en la prueba                                                                                            |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                            | 
|                         | **"TRUE"** para mostrar el gráfico de diagnostico - **_Opción Predeterminada_**                                                                |
|                         | **"FALSE"** para no mostrar el gráfico de diagnostico                                                                                          |

## 2) Segunda Opción: Utilice el comando _ur.za_ [^2]

[^2]: **Este comando pertenece a la librería _urca_**

``` r
ur.za(x, model = c("intercept", "trend", "both"), lag=NULL)
```
| **Argumentos**          | **Descripción**                                                                                                                                | 
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo a la que se le va a hacer la prueba                                                                      |
| **model**               | en esta opción se especifica el tipo de prueba $ZA$ que se va a llevar a cabo. En particular, se especifica si cambio estructural ocurre en:   | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                      | 
|                         | **"trend**: tendencia                                                                                                                          |
|                         | **"both"**: ambos                                                                                                                              | 
| **lag**                 | el número máximo de rezagos utilizados en la prueba                                                                                            |

#### De las dos opciones, mi preferida es la segunda por la información que revela al hacer la prueba. Después se puede utilizar la primera opciónvisualizar la prueba gráficamente. 


| [Subsección 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [Subsección 2.2.4.(T) Explicación general de las pruebas de cambio estructural (la prueba de Perron y la prueba _ZA_)](../Seccion02_02_03_T/Readme.md)  |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
