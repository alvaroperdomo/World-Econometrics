<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.4_R:
# Pruebas de Hipótesis de un $VEC$ en R
## 1) Número de rezagos
Para determinar el número óptimo de rezagos dentro del VEC se utiliza el comando:

``` r
VARorder(x,maxp=10)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                    |
| **maxp**           | el número máximo de rezagos utilizados en la prueba (la **_Opción Predeterminada es 13_**)                          |

## 2) Gráfico de la $FAC$ y la $FACP$ de los residuos de la prueba de cointegración de Johansen
Recuerde que los residuos del la prueba de cointegración de Johansen tienen que ser ruido blanco ruido blanco. Entonces, el siguiente comando permite ver el gráfico de la $FAC$ y de la $FACP$ para verificar que los mismos no esten correlacionados:

``` r
plotres(nombre)
```
| **Argumentos**     | **Descripción**                                                        | 
|--------------------|------------------------------------------------------------------------|
| **nombre**         | nombre del $VEC$ que se estimó  con el comando "ca.jo"                 |

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
