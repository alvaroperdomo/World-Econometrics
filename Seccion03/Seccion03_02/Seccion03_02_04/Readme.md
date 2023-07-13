## SECCIÓN 3.2.4:
# Pruebas de Hipótesis de un $VEC$ en R
## 1) Número de rezagos
Para determinar el número óptimo de rezagos dentro del VEC se utiliza el comando:

``` r
VARorder(x,maxp=10)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VAR                    |
| **maxp**           | el número máximo de rezagos utilizados en la prueba (la **_Opción Predeterminada es 13_**)                          |
