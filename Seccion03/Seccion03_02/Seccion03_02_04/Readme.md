## SECCIÓN 3.2.4:
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

## 2) Gráfico de la $FAC$ y la $FACP$ de los residuos del $VEC$
Recuerde que los residuos del $VEC$ son ruido blanco. Entonces, el siguiente comando permite ver el gráfico de la $FAC$ y de la $FACP$ para verificar que los mismos no esten correlacionados:

``` r
plotres(nombre)
```
| **Argumentos**     | **Descripción**                                                        | 
|--------------------|------------------------------------------------------------------------|
| **nombre**         | nombre del $VEC$ que se estimó  con el comando "ca.jo"                 |
