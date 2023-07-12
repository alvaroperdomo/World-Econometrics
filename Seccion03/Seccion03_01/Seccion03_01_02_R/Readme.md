## SECCIÓN 3.1.2. (R):

# Pruebas de Hipótesis de un $VAR$ en R

## 1) Número de rezagos
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


## 2) Prueba de estabilidad
El comando "roots" calcula las raíces caracteristicas del modelo VAR que ha sido previamente estimado. Recuerden que la condición de estabilidad requiere que estas raíces en valor absoluto sean menores que $1$. 

``` r
roots(nombre)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |

## 3)  Pruebas sobre los residuos
#### a)  Gráficos de la $FAC$ y de la $FACP$ de los residuos para analizar la autocorrelación de los residios de la estimación

Recuerde que se requiere que los residuos sean ruido blanco (por lo que no debe haber autocorrelación). Para visualizar la $FAC$ y la $FACP$ copie los siguientes comandos: [^1]
[^1]: A estos dos comandos se le pueden incluir más argumentos, pero para llevar a cabo las pruebas sobre los residuos del VAR, con los que aquí se utilizan es suficiente

``` r
acf(residuals(nombre)[,1])
pacf(residuals(nombre)[,1])
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |


#### b)  Prueba Portmanteau Multivariada para analizar la autocorrelación de los residuos
Utilice el siguiente comando:
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
