<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.1.1. (R):

# Estimando y proyectando un modelo $VAR$ en $R$

## (1) Estimando un modelo $VAR$
Para estimar un $VAR$ se utiliza el comando _**VAR**_:[^1]

[^1]: **Este comando pertenece a la librería _vars_**

``` r
VAR(x, p = 1, type = c("const", "trend", "both", "none"), season = NULL, exogen = NULL, lag.max = NULL, ic = c("AIC", "HQ", "SC", "FPE"))
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el $VAR$                  |
| **p**              | número de rezagos que se incluyen en el $VAR$ (la **_Opción Predeterminada es 1_**)                                 |
| **type**           | en esta opción se especifica si al $VAR$ se le van a incluir algunas variables exógenas predeterminadas:            |
|                    |  **"const"** para un VAR con intercepto                                                                             |
|                    |  **"trend"** para un VAR con tendencia lineal                                                                       |
|                    |  **"both"** para un VAR con intercepto y con tendencia lineal                                                       |
|                    |  **"none"** para un VAR sin intercepto y sin tendencia lineal                                                       |
| **season**         | si los datos son estacionales, se da la opción de incluir variables dummy centradas dentro del VAR                  |
| **exogen**         | se da la opción de incluir variables exógenas adicionales dentro del $VAR$                                          | 
| **lag.max**        | el número máximo de rezagos utilizado en la prueba determinada por la opción **ic**                                 | 
| **ic**             | Criterio de Información utilizado                                                                                   | 
|                    | **AIC**: Criterio de Información de Akaike                                                                          | 
|                    | **HQ**: Criterio de Información de Hannan-Quinn                                                                     | 
|                    | **SC**: Criterio Bayesiano de Schwartz                                                                              | 
|                    | **FPE**: Error Final de Predicción                                                                                  | 

## (2) Proyectando un modelo $VAR$
Para hacer pronósticos y gráficos de los mismos en un modelo $VAR$, utilice el comando _**forecast**_:[^2]

``` r
forecast1<-forecast(nombre1, level = c(95), h = 3)
forecast1
autoplot(forecast1)
```

| **Argumentos**       | **Descripción**                                                                                                                                          | 
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nombre1**          | nombre del modelo $VAR$ para el cual se desean hacer pronósticos                                                                                         |
| **level**            | valor para determinar el tamaño de la banda de confianza de los pronósticos                                                                              |
| **h**                | número de periodos que se desean pronósticar                                                                                                             |

Observe que los pronósticos se trabajaron con el nombre **forecast1**. Usted puede escoger un nombre diferente.

[^2]: **Este comando pertenece a la librería _forecast_**


---
---
# Preguntas de selección múltiple

1. **En el comando $VAR$ ¿cuál es el número de rezagos que se estiman de forma predeterminada?:**
 
   a) $0$.

   b) $1$.

   c) $2$.

   d) Ninguno de los anteriores.
 
2. **¿Cuáles criterios de información se puden utilizar para estimar un $VAR$ más parsimonioso?:**

   a) El Criterio de Información de Akaike.

   b) El Criterio Bayesiano de Schwartz.

   c) El Criterio de Información de Hannan-Quinn

   d) Todos los anteriores.
---
---  

| [Subsección: 3.1. Estimación de Modelos _VAR_](../Readme.md) |[Subsección: 3.1.1.(T) ¿Qué es un _VAR_, cómo estimarlo y cómo hacer pronósticos?](../Seccion03_01_01_T/Readme.md) |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
