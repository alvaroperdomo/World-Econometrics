## SECCIÓN 1.1:
# Series de Tiempo

## ¿Qué es una serie de tiempo?
Una serie de tiempo es una secuencia de observaciones sobre una variable tomada a intervalos discretos en el tiempo. Donde estos intervalos discretos tienen la misma unidad de medida; es decir, están igualmente distanciados [^1] .

Muchos tipos de datos aparecen como series de tiempo[^2]: 
* la serie anual del PIB de un país en particular,
* la secuencia trimestral de la tasa de desempleo de una ciudad,
* una secuencia mensual de la tasa de inflación de una nación,
* una serie semanal de las ventas de una empresa, 
* observaciones por hora o  por minuto de la tasa de cambio de una divisa, 

y así sucesivamente. 

[^1]: **El curso sólo se va a enfocar en series de tiempo anuales. Sin embargo, todo lo aprendido también es aplicable a series de tiempo con diferente periodicidad.**
[^2]:**La periodicidad de estas secuencias no es única. Algunos datos son públicados con diferentes periodicidades**

## ¿En qué consiste el análisis de series de tiempo?
Una característica intrínseca de una serie tiempo es que, por lo general, las observaciones adyacentes son dependientes.

El análisis de series tiempo (o en otras palabras, la econometría de series de tiempo) se refiere al manejo de técnicas para el análisis de esta dependencia. Esto requiere el desarrollo de modelos estocásticos y dinámicos para datos de series de tiempo y el uso aplicado de dichos modelos.

Un uso importante del análisis de series de tiempo ha sido para el desarrollo de modelos dinámicos multivariados que permitan representar las relaciones conjuntas entre diversas variables a lo largo del tiempo. También ha sido tradicionalmente importante el análisis de series de tiempo para pronosticar los valores futuros de una serie de tiempo a partir de sus valores presentes y pasados.[^3]

[^3]:**Cuando se trata de pronósticos, un supuesto crucial es que los datos en la muestra que se usan para la especificación y estimación del modelo son similares a los datos fuera de la muestra. Si no, no tiene sentido dedicar mucho tiempo a la construcción de modelos de series de tiempo, al menos si las posibles diferencias no se tienen en cuenta de manera adecuada.**

## ¿Qué es una variable discreta aleatoria?
Una variable discreta $y$ es una variable aleatoria (es decir, estocástica) si, para cualquier número real $r$, existe una probabilidad $p(y\leq r)$ de que $y$ tome un valor menor o igual a $r$. 

## Los procesos estocásticos
Un proceso estocástico se define como un conjunto de magnitudes aleatorias que varían con el tiempo. En consecuencia, a los elementos observados de una serie de tiempo { $y_0, y_1, y_2, ..., y_t$ } se les denota como realizaciones de un proceso estocástico, en donde que $y_t$ se refiere a un elemento de la secuencia completa { $y_t$ }. 

| [Siguiente Sección: 1.2. R y los _Indicadores de Desarrollo Mundial_](../Seccion01_02/README.md) | [:house: Inicio](../../README.md) |
|--------------------------------------------------------------------------------------------------|-----------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
