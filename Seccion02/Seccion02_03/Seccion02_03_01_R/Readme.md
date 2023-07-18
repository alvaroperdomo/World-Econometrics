## SECCIÓN 2.3.1.R. 
# Las tres etapas de la metodología de Box-Jenkins (Aplicación en $R$)

## Identificación:

### $FAC$ y $FACP$ muestrales
Para gráficar la función de autocorrelación $FAC$ y la función de autocorrelación parcial $FACP$ muestrales copie los siguientes comandos

``` r
autoplot(acf(x, plot = FALSE))
autoplot(pacf(x, plot = FALSE))
```

| **Argumentos**[^1]   | **Descripción**                                                                                                                                          | 
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                | vector o variable de series de tiempo                                                                                                                    |
| **plot**             | Argumento lógico que sólo tiene dos opciones:                                                                                                            |
|                      | **TRUE** (**Opción Predeterminada**)                                                                                                            |
|                      | **FALSE**                                                                                                             |

[^1]: El comando _autoplot_ tiene muchisimos más argumentos (buena parte de ellos estéticos), pero por lo pronto sólo nos interesan los dos que presentamos
