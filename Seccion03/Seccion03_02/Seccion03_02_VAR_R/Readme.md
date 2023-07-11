# Estimando un $VAR$ en R

Llame el paquete "vars"

``` r
library("vars")
```

Para estimar un VAR se utiliza el comando:

``` r
VAR(x, p = 1, type = c("const", "trend", "both", "none"), season = NULL, exogen = NULL, lag.max = NULL, ic = c("AIC", "HQ", "SC", "FPE"))
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VAR                    |
| **p**              | número de rezagos que se incluyen en el VAR (la **_Opción Predeterminada es 1_**)                                   |
| **type**           | en esta opción se especifica si al VAR se le van a incluir algunas variables exógenas predeterminadas:              |
|                    |  **"const"** para un VAR con intercepto                                                                             |
|                    |  **"trend"** para un VAR con tendencia lineal                                                                       |
|                    |  **"both"** para un VAR con intercepto y con tendencia lineal                                                       |
|                    |  **"none"** para un VAR sin intercepto y sin tendencia lineal                                                       |
| **season**         | se da la opción de incluir variables dummy centradas dentro del VAR                                                 |
| **exogen**         | Se da la opción de incluir variables exógenas adicionales dentro del VAR                                            | 
| **lag.max**        | el número máximo de rezagos utilizado en la prueba determinada por la opción **ic**                                 | 
| **ic**             | Criterio de Información utilizado                                                                                   | 


Para determinar el número óptimo de rezagos dentro del VAR se utiliza el comando:
``` r
VARselect(x, lag.max = 10, type = c("const", "trend", "both", "none"), season = NULL, exogen = NULL)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VAR                    |
| **lag.max**        | el número máximo de rezagos utilizados en la prueba (la **_Opción Predeterminada es 10_**)                          |
| **type**           | en esta opción se especifica si al VAR se le van a incluir algunas variables exógenas predeterminadas:              |
|                    |  **"const"** para un VAR con intercepto                                                                             |
|                    |  **"trend"** para un VAR con tendencia lineal                                                                       |
|                    |  **"both"** para un VAR con intercepto y con tendencia lineal                                                       |
|                    |  **"none"** para un VAR sin intercepto y sin tendencia lineal                                                       |
| **season**         | se da la opción de incluir variables dummy centradas dentro del VAR                                                 |
| **exogen**         | Se da la opción de incluir variables exógenas adicionales dentro del VAR                                            | 
