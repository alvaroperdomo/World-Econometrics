### Entendiendo un Mundo en Desarrollo: 
# Análisis Aplicado de Series de Tiempo
Keywords: `Econometría` `Series de Tiempo` `Indicadores de Desarrollo Mundial` `Banco Mundial`
#### Duración del curso: XXXX horas
Bienvenido al curso _Entendiendo un Mundo en Desarrollo: Análisis Aplicado de Series de Tiempo_. En este curso aprenderá a hacer análisis básicos de series de tiempo en $R$ utilizando como entorno de desarrollo interactivo al software $RStudio$. Se tomará como referencia, para el análisis empírico, la base de datos del Banco Mundial "[_Indicadores de Desarrollo Mundial_](https://databank.worldbank.org/source/world-development-indicators)", con el fin de aprender las principales técnicas del análisis de series de tiempo de una forma interesante.

El curso esta dividido en tres secciones principales, a través de las cuales el estudiante desarrollará diferentes habilidades computacionales y analíticas, que podrá aplicar en el análisis de estudios de caso propios.

| Secciones                                                                                               |   
|---------------------------------------------------------------------------------------------------------|
| 01. Las Series de Tiempo,  $R$ y los _Indicadores de Desarrollo Mundial_                                | 
| 02. Análisis Univariado (_ARIMA_)                                                                       | 
| 03. Análisis Multivariado (_VAR_ y _VEC_)                                                               | 


## Objetivos generales
* Explicar y aplicar los principales fundamentos del análisis de series de tiempo.
* Estudiar los modelos de series de tiempo más relevantes.
* Explicar la construcción y análisis de modelos de series de tiempo univariados y multivariados.
* Explicar cómo hacer análisis de series de tiempo en $R$ utilizando la base _Indicadores de Desarrollo Mundial_.

## Requisitos
Es deseable que las alumnos tengan como mínimo conocimientos básicos de $R$ y de probabilidad. Sin embargo, en el curso se van a proveer códigos completos de $R$ para ser utilizados por parte de los alumnos.

Es importante que los alumnos además de tener instalados $R$ y [_**RStudio**_](https://posit.co/download/rstudio-desktop/), también tengan instalado el paquete de herramientas [Rtools](https://cran.r-project.org/bin/windows/Rtools/).

A las personas que no utilizan regularmente $R$ se les recomienda desarrollar de antemano los tutoriales de https://learnr.numbat.space/

## Dirigido a
Los contenidos presentados en este curso están dirigidos a estudiantes y profesionales de diferentes disciplinas que requieran aprender y/o fortalecer sus conocimientos en análisis de series de tiempo en R.

## Metodología
La primera sección del curso comienza con una explicación de lo que es el análisis de series de tiempo y de las caracteristicas principales de la base de datos "_Indicadores de Desarrollo Mundial_". Posteriormente, se explica cómo se puede descargar y manipular esta base directamente desde $RStudio$. Al final de la sección se hace una evaluación donde se formulan varias preguntas de selección múltiple con respecto a los conceptos vistos en ésta y con respecto a un ejercicio que el alumno debe desarrollar en $RStudio$ utilizando los "Indicadores de Desarrollo Mundial" [^1]. 

[^1]: **Los códigos en _R_ necesarios para el desarrollo del ejercicio serán explicados durante el desarrollo de la sección**

Las otras dos secciones del curso comienzan con una explicación general de las herramientas econométricas a utilizar. Luego, se desarrollan algunos ejercicios y posteriormente se propone un ejercicio empírico para ser realizado por el alumno. Con respecto a este último se haran algunas preguntas de selección múltiple para evaluar si los conceptos principales han sido entendidos de forma apropiada.

A continuación encontrara información más detallada acerca de cada una de las secciones en que se divide el curso. Si quiere ir a alguna sección en particular, dele _click_ con el mouse al titulo de la sección respectiva. 

## [Sección 1 - Las Series de Tiempo, _R_ y los _Indicadores de Desarrollo Mundial_](Seccion01/Readme.md)
| Subsecciones                                             | Contenido                                                                                                                | Dedicación,<br> 2.5 horas   | 
|----------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|:-------------------------:|
| **1.1. Series de tiempo**                                |¿Qué es una serie de tiempo?, ¿En qué consiste el análisis de series de tiempo?, ¿Qué es una variable discreta aleatoria? |             0.5           | 
| **1.2. $R$ y los _Indicadores de Desarrollo Mundial_**   |¿Cómo manipular los _Indicadores de Desarrollo Mundial_ en R?                                                             |             2.0           | 

## [Sección 2 - Análisis Univariado (ARIMA)](Seccion02/Readme.md)
| Subsecciones                                           | Contenido                                                                                            | Dedicación,<br> 5.5 horas | 
|--------------------------------------------------------|------------------------------------------------------------------------------------------------------|:-------------------------:|
| **2.1. Series Estacionarias y No Estacionarias**       | Se explica la importancia de las series estacionarias en el análisis univariado de series de tiempo. |              1            | 
| **2.2* Pruebas de Raíz Unitaria**                      | Explicación acerca de la metodología y lectura de diferentes tipos de pruebas de raíz unitaria       |              1            | 
| **2.2.1.** Las Pruebas $DF$ y $ADF$: _Pruebas de Dickey Fuller y Aumentada de Dickey Fuller_ | Teoría y aplicación en $R$                                     |                           | 
| **2.2.2.** La Prueba $ADF-GLS$: _Prueba de Elliott, Rothenberg y Stock_                    | Teoría y aplicación en $R$                                       |                           | 
| **2.2.3.** La Prueba $KPSS$: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_             | Teoría y aplicación en $R$                                       |                           |   
| **2.2.4.** Pruebas de Cambio Estructural: _Prueba de Perron y Prueba de Zivot y Andrews_ | Teoría y aplicación en $R$                                         |                           | 
| **2.3. Análisis $ARIMA$ (Metodología de Box-Jenkins)** | Explicación acerca de cómo estimar un modelo ARIMA                                                   |              1            | 
| **2.3.1.** Las tres etapas de la metodología de Box-Jenkins                      | Teoría y aplicación en $R$                                                 |                           | 
| **2.3.2.** Estimación de un $ARMA$ - Ejemplos simulados                          | Desarrollo de ejemplos conceptuales                                        |                           | 
| **2.3.3.** Ejemplo utilizando la base de datos _Indicadores de Desarrollo Mundial_ | Aplicación en $R$                                                        |                           | 

## [Sección 3 - Análisis Multivariado de Series de Tiempo](Seccion03/Readme.md)
| Subsecciones                                                                                           | Contenido                                     | Dedicación,<br> 5.5 horas | 
|--------------------------------------------------------------------------------------------------------|-----------------------------------------------|:-------------------------:|
| **3.1. Estimación de Modelos $VAR$**                                                                   | Metododología para estimar un modelo $VAR$    |              1            | 
| **3.1.1.** ¿Qué es un modelo $VAR$?, <br> ¿Cómo estimar y hacer pronosticos de un modelo $VAR$ en $R$? | Teoría y aplicación en $R$                    |                           | 
| **3.1.2.** Pruebas de Hipótesis en Modelos $VAR$                                                       | Teoría y aplicación en $R$                    |                           |                        
| **3.1.3.** La Función Impulso Respuesta y la Descomposición de Varianza                                | Teoría y aplicación en $R$                    |                           |            
| **3.2. Cointegración y estimación de Modelos $VEC$**                                                   | Metodología para estimar un modelo $VEC$.     |              1            | 
| **3.2.1.** ¿Qué es y para qué sirve la Cointegración?                                                  | Explicación teórica                           |                           | 
|  **3.2.2.** La Metodología de Engle y Granger (**2 variables**): <br> ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo VEC? |  Teoría y aplicación en $R$      |
| **3.2.3.** La Metodología de Johansen (**2 o más variables**): <br> ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo VEC?   |  Teoría y aplicación en $R$      |
| **3.2.4.** Pruebas de hipótesis adicionales para implementar en un moelo VEC                           | Teoría y aplicación en $R$                    |                           |

### _Durante el desarrollo del curso se van a plantear algunos ejercicios con los cuales al ser resueltos de forma satisfactoria van a permitir optar, previo pago, a la obtención del certificado del curso._
  
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
