<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.1.3. (R):

# Funciones Impulso Respuesta en R

Las funciones impulso-respuesta se obtienen utilizando el siguiente comando:[^1]

[^1]:A este comandos se le pueden incluir más argumentos, pero sólo nos vamos a enfocar en los que se colocan a continuación:

``` r
irf(nombre, impulse = NULL, response = NULL, n.ahead = 10, ortho = TRUE, ci=0.95)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **impulse**        | variable o vector de variables a las que se les genera el impulso (**_Opción Predeterminada es "all"_**)            |
| **response**       | variable o vector de variables a las que se les evalua la respuesta  (**_Opción Predeterminada es "all"_**)         |
| **ortho**          | Se calculan los coeficientes impulso-respuesta ortogonalizados (**_Opción Predeterminada es "TRUE"_**)[^2]          | 
| **ci**             | Intervalo de condianza (**_Opción Predeterminada es 0.95_**)                                                        | 

[^2]: **La ortogonalización implica que los efectos de los shocks en una variable se descomponen de manera que sean independientes de los efectos de los shocks en las demás variables del sistema. Esto permite una interpretación más clara de cómo un shock en una variable en particular afecta a todas las demás, ya que elimina la influencia directa de los shocks en las otras variables. En resumen, los coeficientes de respuesta al impulso ortogonalizados proporcionan una representación de cómo las variables en un sistema VAR responden a los shocks en otras variables, teniendo en cuenta las relaciones de causalidad entre ellas y asegurando que los efectos sean mutuamente independientes, lo que facilita la interpretación y el análisis de la dinámica del sistema.**

``` r
modelo.irf<-irf(modelo,impulse="x", response="y")
modelo.irf2<-irf(modelo,impulse="x", response="x")
plot(modelo.irf)
plot(modelo.irf2)
```

# Descomposición de Varianza en R

``` r
fevd(nombre, n.ahead=10, ...)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **n.ahead**        | Número de periodos para los cuales se va a calcular la descomposición de varianza                                   |

--
---
# Preguntas de selección múltiple

1. **En el comando _**irf**_ ¿A cual variable, esta predeterminado, que se le calcule la función impulso-respuesta?:**
 
   a) A la primera variable del _VAR_,

   b) A la última variable del _VAR_.

   c) A ninguna variable.

   d) A todas las variables del _VAR_.
 
2. **¿Para que sirve la opción _n.ahead_ en el comando _fevd_?:**

   a) Para colocar el nombre del VAR al cual se le va a calcular la descomposición de varianza

   b) Para determinar el número de periodos para los cuales se va a calcular la descomposición de varianza

   c) Para calcular la descomposición de varianza de los datos ortogonalizados.

   d) Para determinar el nombre de la variable a la cual se le va a calcular la descomposición de varianza.
---
---
| [Subsección: 3.1. Estimación de Modelos _VAR_](../Readme.md) |[Subsección: 3.1.3.(T) La Función Impulso-Respuesta y la Descomposición de Varianza del Error de Pronóstico](../Seccion03_01_03_T/Readme.md) |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
