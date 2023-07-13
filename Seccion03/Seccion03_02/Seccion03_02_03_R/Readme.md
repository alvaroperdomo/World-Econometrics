## SECCIÓN 3.1.1. (R):
# La Prueba de Cointegración de Johansen 

``` r
ca.jo(x, type = c("eigen", "trace"), ecdet = c("none", "const", "trend"), K = 2,spec=c("longrun", "transitory"), season = NULL, dumvar = NULL)
ca.jo(txhs, type="eigen",spec="transitory",ecdet="none",K=2)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                    |
| **type**           | en esta opción se especificael tipo de estadistico de Johanswen que se quiere calcular:                             |
|                    |  **"eigen"** se calcula el estadistico $\lambda_{max}$                                                                             |
|                    |  **"trace"** se calcula el estadistico $\lambda_{traza}$                                                                            |
| **ecdet**          | en esta opción se especifica si al VEC se le van a incluir algunas variables exógenas predeterminadas:              |
|                    |  **"const"** para un VAR con intercepto                                                                             |
|                    |  **"trend"** para un VAR con tendencia lineal                                                                       |
|                    |  **"both"** para un VAR con intercepto y con tendencia lineal                                                       |
|                    |  **"none"** para un VAR sin intercepto y sin tendencia lineal                                                       |
| **season**         | se da la opción de incluir variables dummy estacionales centradas dentro del VAR                                    |
| **exogen**         | Se da la opción de incluir variables exógenas adicionales dentro del VAR                                            | 
