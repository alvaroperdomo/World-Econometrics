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

# Estimación del $VEC$ 
Para estimar el modelo $VEC$ copie el comando:
``` r
cajorls(johansen,r=1)
```
