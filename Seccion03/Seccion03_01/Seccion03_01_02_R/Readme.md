## SECCIÓN 3.1.2. (R):

# Pruebas de Hipótesis de un $VAR$ en $R$

## 1) Número de rezagos
Para determinar el número óptimo de rezagos dentro del $VAR$ se utiliza el comando:
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
El comando "roots" calcula las raíces caracteristicas del modelo $VAR$ que ha sido previamente estimado. Recuerden que la condición de estabilidad requiere que estas raíces en valor absoluto sean menores que $1$. 

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
serial.test(nombre,lags.pt=10)
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **lags.pt**        | Número de Rezagos que se va a utilizar en la prueba de Portmanteau                                                  |


#### c) Prueba de normalidad de los residuos
Este comando calcula pruebas de Jarque-Bera univariadas y multivariadas y pruebas multivariadas de asimetría y curtosis para los residuos de un VAR o de un VEC en niveles.

``` r
normality.test(nombre, multivariate.only={FALSE, TRUE})
```

| **Argumentos**        | **Descripción**                                                                                                     | 
|-----------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**            | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **multivariate.only** | Este comando tiene dos opciones:                                                                                    |
|                       | **TRUE**:  Sólo se calculan las pruebas de normalidad multivariadas **(Opción: Predeterminada)**                    |
|                       | **FALSE**: Se calculan las pruebas de normalidad multivariadas y univariadas                                        |

## 4)  Prueba de Granger
Este comando cálcula la prueba de causalidad de Granger entre dos variables

``` r
grangertest(x, y, order = 1, na.action = na.omit, …)
grangertest(y, x, order = 1, na.action = na.omit, …)
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
grangertest(diff(y) ~ diff(x),order=1,data=Datos)
grangertest(diff(x) ~ diff(y),order=1,data=Datos)
```
| **Argumentos**        | **Descripción**                                                                                                     | 
|-----------------------|---------------------------------------------------------------------------------------------------------------------|
| **x** y **y**         | Nombre de las variables a las cuales se les va a llevar a cabo la prueba                                            |
| **order**             | Número que específica el orden de los rezagos que se incluirán en la regresión auxiliar.                            |
|                       | **TRUE**:  Sólo se calculan las pruebas de normalidad multivariadas **(Opción: Predeterminada)**                    |
|                       | **FALSE**: Se calculan las pruebas de normalidad multivariadas y univariadas                                        |



<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
