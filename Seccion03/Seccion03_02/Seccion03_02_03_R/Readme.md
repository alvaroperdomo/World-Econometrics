## SECCIÓN 3.1.1. (R):
# La Prueba de Cointegración de Johansen 

La para hacer la prueba de cointegración de Johansen copie el comando:

``` r
ca.jo(x, type = c("eigen", "trace"), ecdet = c("none", "const", "trend"), K = 2,spec=c("longrun", "transitory"))
ca.jo(txhs, type="eigen",spec="transitory",ecdet="none",K=2)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                    |
| **type**           | en esta opción se especificael tipo de estadistico de Johanswen que se quiere calcular:                             |
|                    |  **"eigen"** se calcula el estadistico $\lambda_{max}$                                                              |
|                    |  **"trace"** se calcula el estadistico $\lambda_{traza}$                                                            |
| **ecdet**          | en esta opción se especifica si al VEC se le van a incluir algunas variables exógenas predeterminadas:              |
|                    |  **"none"** VEC sin intercepto en la cointegración                                                                  |
|                    |  **"const"** VEC con intercepto en la cointegración                                                                 |
|                    |  **"trend"** VEC con tendencia en la cointegración                                                                  |
| **K**              | número de rezagos que incluye el VEC                                                                                |
| **spec**           | aquí se pueden escoger dos opciones para hacer la prueba                                                            |
|                    |  **"longrun"** se calcula el estadistico $\lambda_{traza}$                                                          |
|                    |  **"transitory"** se calcula el estadistico $\lambda_{traza}$                                                       |

Dado un $VAR$ de la forma:

XXXXXXXXXXXXXXXXXXXXXXXXXX

existen las siguientes dos especificaciones de un VECM:

XXXXXXXXXXXXXXXXXXXXXXXXXX

donde

XXXXXXXXXXXXXXXXXXXXXXXXXX

y

XXXXXXXXXXXXXXXXXXXXXXXXXX

Las XXXXX las matrices contienen los impactos acumulativos de largo plazo, por lo tanto, si se escoge **spec="long run"**, se estima el $VEC$ anterior.

La otra especificación del $VEC$ es de la forma: 

XXXXXXXXXXXXXXXXXXXXXXXXXX

donde

XXXXXXXXXXXXXXXXXXXXXXXXXX

y

XXXXXXXXXXXXXXXXXXXXXXXXXX


**ANOTACIÓN de R:** La matriz XXXXX es la misma que en la primera especificación. Sin embargo, las matrices ahora difieren, en el sentido de que miden los efectos transitorios, por lo tanto, al establecer **spec="transitory"** se estima la segunda forma del $VEC$. Tenga en cuenta que las inferencias extraídas serán las mismas, independientemente de la especificación elegida y que el poder explicativo también es el mismo.


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
