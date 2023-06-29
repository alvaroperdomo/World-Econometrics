# Aplicando la prueba ZA en R

Para llevar a cabo la prueba ZA ofrecemos dos opciones:

## 1) **Primera Opción:** Utilice el comando **unitrootTest**

``` r
urkpssTest(x, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** - **_Opción Predeterminada_**                                                                                                       |
|                         | **"tau"**                                                                                                                                    |
| **lags**                | el máximo número de rezagos                                                                                                                  |
| **use.lag**             | cadena de caracteres que especifica el número de rezagos                                                                                     |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico - **_Opción Predeterminada_**                                                                 |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

## 2) **Segunda Opción:** Utilice el comando **ur.za*
``` r
ur.za(x, model = c("intercept", "trend", "both"), lag=NULL
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **model**               | Especificación si el quiebre estructural ocurrió en el intercepto, en la tendencia o en ambos.                                               |
|                         | **"intercept"** establece que el quiebre estructural se da en el intercepto                                                             |
|                         | **"trend"** establece que el quiebre estructural se da en la tendencia                                                            |
|                         | **"both"** establece que el quiebre estructural se da en el intercepto y en la tendencia                                                                             |
| **use.lag**             | número de rezagos especificados por el usuario                                                                                               |

#### De las dos opciones, mi preferida es esta última porque la opción "lags" da varias recomendaciones para escoger los rezagos.
#### Posteriormente, se puede utilizar cla primera opción para aprovechar el comando "doplot"
