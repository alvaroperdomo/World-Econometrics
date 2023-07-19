## SECCIÓN 3.1.1. (R):

LEER ESTO: https://otexts.com/fpp2/VAR.html

# Estimando un $VAR$ en $R$

Para estimar un $VAR$ se utiliza el comando _**VAR**_:[^1]

[^1]: **Este comando pertenece a la librería _vars_**

``` r
VAR(x, p = 1, type = c("const", "trend", "both", "none"), season = NULL, exogen = NULL, lag.max = NULL, ic = c("AIC", "HQ", "SC", "FPE"))
```

| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el $VA$                   |
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

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
