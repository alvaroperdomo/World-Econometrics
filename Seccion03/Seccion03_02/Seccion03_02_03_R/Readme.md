<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.3. (R):
# La Prueba de Cointegración de Johansen 

La para hacer la prueba de cointegración de Johansen copie el comando:

``` r
ca.jo(x, type = c("eigen", "trace"), ecdet = c("none", "const", "trend"), K = 2,spec=c("longrun", "transitory"), dumvar=NULL)
```

| **Argumentos**     | **Descripción**                                                                                                                                          | 
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el $VEC$                                                       |
| **type**           | en esta opción se especificael tipo de estadistico de Johanswen que se quiere calcular:                                                                  |
|                    |  **"eigen"** se calcula el estadistico $\lambda_{max}$                                                                                                   |
|                    |  **"trace"** se calcula el estadistico $\lambda_{traza}$                                                                                                 |
| **ecdet**          | en esta opción se especifica si al $VEC$ se le van a incluir algunas variables exógenas predeterminadas:                                                 |
|                    |  **"none"** VEC sin intercepto en la cointegración                                                                                                       |
|                    |  **"const"** VEC con intercepto en la cointegración                                                                                                      |
|                    |  **"trend"** VEC con tendencia en la cointegración                                                                                                       |
| **K**              | número de rezagos que incluye el $VEC$[^1]                                                                                                               |
| **spec**           | aquí se pueden escoger dos opciones para hacer la prueba. Sin embargo, en la sección teórica la opción que se desarrollo es  **spec="transitory"** [^2]  |
|                    |  **"longrun"**                                                                                                                                           |
|                    |  **"transitory"**                                                                                                                                        |
| **dumvar**         |  Si este argumento no es **NULL** (**_Opción Predeterminada_**), se incluye una matriz de variables dummy en el $VEC$.[^3]                               |

[^1]: **Tenga en cuenta que _ca.jo_ sólo funciona con _K>1_. Entonces, si previamente utiliza el comando _VARselect_ para determinar el número de rezagos para la prueba, pero _VARselect_ le reporta que lo óptimo es utilizar un rezago, entonces utilice la segunda mejor opción reportada por el comando.**
[^2]: **ANOTACIÓN DE _R_: Independientemente de la especificación elegida, el poder explicativo también es el mismo.**
[^3]: **Tenga en cuenta que el número de filas de la matriz que contiene las variables dummy debe ser igual al número de filas de _x_**

En cuanto a la opción a escoger en el argumento $ecdet$, tenga en cuenta que:

* **ecdet = "none"**: Esta opción asume que no hay ningún término de tendencia o constante en el modelo. Es apropiado cuando se cree que las variables están cointegradas sin ninguna tendencia o desviación sistemática en el largo plazo.
* **ecdet = "const"**: Esta opción incluye un término de constante en el modelo. Es adecuado cuando se sospecha que hay una desviación sistemática en el largo plazo, pero no se espera una tendencia lineal.
* **ecdet = "trend"**: Esta opción incluye un término de tendencia lineal en el modelo. Es apropiado cuando se espera que las variables cointegradas tengan una tendencia lineal en el largo plazo.

# Estimación del $VEC$ 
Para estimar el modelo $VEC$ copie el comando:
``` r
cajorls(x, r = 1, reg.number = NULL)
```

| **Argumentos**     | **Descripción**                                                                                                                                                      | 
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **z**              | nombre de la prueba de cointegración de Johansen utilizada en ca.jo                                                                                                  |
| **r**              | valor que determina el número de vectores de cointegración obtenidos en $\lambda_{max}$ y/o en  $\lambda_{traza}$                                                   |
| **reg.number**     | El número de la ecuación del $VEC$ que se debe estimar o, si se escoge en $NULL$ ( esta es la **_Opción Predeterminada_** ), se estiman todas las ecuaciones dentro del $VEC$. |

Por ejemplo,
``` r
vec_rank <- ca.jo(x, type = "trace", ecdet = "const", K = 2)
cajorls(vec_rank, r = 1, reg.number = NULL)
```
---
---
# Preguntas de selección múltiple

1. **¿Cuál de los estadisticos de la prueba de cointegración de Johansen se pueden calcular con el comando _ca.jo(x, type = c("eigen"))_?:**
 
   a) El estadístico $\lambda_{max}$.

   b) el estadístico $\lambda_{traza}$ .

   c) Todos los anteriores.

   d) Ninguno de los anteriores.

2. **¿Con cuál comando se estima el $VEC$?:**
 
   a) _ca.jo_.

   b) _cajorls_.

   c) Todos los anteriores.

   d) Ninguno de los anteriores.

---
---
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |[Subsección: 3.2.3.(T)  La Metodología de Johansen (2 o más variables)](../Seccion03_02_03_T/Readme.md) |
|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
