# Pruebas de Hipótesis de un $VAR$ en R

## Número de rezagos

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
| **season**         | se da la opción de incluir variables dummy estacionales centradas dentro del VAR                                    |
| **exogen**         | Se da la opción de incluir variables exógenas adicionales dentro del VAR                                            | 


## Prueba de estabilidad
La función "roots" calcula las raíces caracteristicas del modelo VAR que ha sido previamente estimado. Recuerden que la condición de estabilidad requiere que estas raíces en valor absoluto sean menores que $1$. 

``` r
roots(p)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **p**              | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |

##  Pruebas sobre los residuos
``` r
acf(residuals(modelo)[,1])
```

###  Prueba Portmanteau Multivariada
``` r
serial.test(modelo,lags.pt=10)
```

### Prueba de normalidad de los residuos
``` r
normality.test(modelo, multivariate.only=FALSE)
```

### Prueba de Granger
``` r
grangertest(diff(y) ~ diff(x),order=1,data=Datos)
grangertest(diff(x) ~ diff(y),order=1,data=Datos)
```

# Funciones Impulso-Respuesta
``` r
modelo.irf<-irf(modelo,impulse="x", response="y")
modelo.irf2<-irf(modelo,impulse="x", response="x")
plot(modelo.irf)
plot(modelo.irf2)
```

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
