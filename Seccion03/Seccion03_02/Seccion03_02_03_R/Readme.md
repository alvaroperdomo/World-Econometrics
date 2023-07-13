## SECCIÓN 3.1.1. (R):
# La Prueba de Cointegración de Johansen 

La para hacer la prueba de cointegración de Johansen copie el comando:

``` r
ca.jo(x, type = c("eigen", "trace"), ecdet = c("none", "const", "trend"), K = 2,spec=c("longrun", "transitory"))
```

| **Argumentos**     | **Descripción**                                                                                                                                          | 
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                                                         |
| **type**           | en esta opción se especificael tipo de estadistico de Johanswen que se quiere calcular:                                                                  |
|                    |  **"eigen"** se calcula el estadistico $\lambda_{max}$                                                                                                   |
|                    |  **"trace"** se calcula el estadistico $\lambda_{traza}$                                                                                                 |
| **ecdet**          | en esta opción se especifica si al VEC se le van a incluir algunas variables exógenas predeterminadas:                                                   |
|                    |  **"none"** VEC sin intercepto en la cointegración                                                                                                       |
|                    |  **"const"** VEC con intercepto en la cointegración                                                                                                      |
|                    |  **"trend"** VEC con tendencia en la cointegración                                                                                                       |
| **K**              | número de rezagos que incluye el VEC                                                                                                                     |
| **spec**           | aquí se pueden escoger dos opciones para hacer la prueba. Sin embargo, en la sección teórica la opción que se desarrollo es  **spec="transitory"** [^1]  |
|                    |  **"longrun"**                                                                                                                                           |
|                    |  **"transitory"**                                                                                                                                        |


[^1]: **ANOTACIÓN DE R: Independientemente de la especificación elegida, el poder explicativo también es el mismo.**


# Estimación del $VEC$ 
Para estimar el modelo $VEC$ copie el comando:
``` r
cajorls(x, r = 1, reg.number = NULL)
```

| **Argumentos**     | **Descripción**                                                                                                                                                      | 
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                                                                     |
| **r**              | número que determina el número de vectores de cointegración obtenidos en $\lambda_{max}$ y/o en  $\lambda_{traza}$                                                   |
| **reg.number**     | El número de la ecuación del VEC que se debe estimar o, si se escoge en NULL ( esta es la **_Opción Predeterminada), se estiman todas las ecuaciones dentro del VEC. |
