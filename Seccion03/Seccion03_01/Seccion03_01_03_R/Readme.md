## SECCIÓN 3.1.3. (R):

# Funciones impulso Respuesta en R

``` r
modelo.irf<-irf(modelo,impulse="x", response="y")
modelo.irf2<-irf(modelo,impulse="x", response="x")
plot(modelo.irf)
plot(modelo.irf2)
```

# Descomposición de Varianza en R
