### Análisis Aplicado de Series de Tiempo: 
# Entendiendo un Mundo en Desarrollo
Keywords: `Econometría` `Series de Tiempo` `Indicadores de Desarrollo Económico` `Banco Mundial`
#### Duración del curso: XXXX horas
Bienvenido al curso _Análisis Aplicado de Series de Tiempo: Entendiendo un Mundo en Desarrollo_. En este curso aprenderá a hacer análisis básicos de series de tiempo en R utilizando como entorno de desarrollo la plataforma [RStudio](https://posit.co/download/rstudio-desktop/). Se tomara como referencia, para el análisis empírico, la base de datos del Banco Mundial "[Indicadores de Desarrollo Económico](https://databank.bancomundial.org/reports.aspx?source=world-development-indicators)", con el fin de aprender los principales entresijos del análisis de series de tiempo de una forma interesante.

El curso esta dividido en tres secciones principales, a través de las cuales el estudiante desarrollará diferentes habilidades computacionales y analíticas, que podrá aplicar en el análisis de estudios de caso propios.

| Secciones                                                                                               |   
|---------------------------------------------------------------------------------------------------------|
| 01. Las Series de Tiempo, R y la Base de Datos del Banco Mundial                                        | 
| 02. Análisis Univariado (ARIMA)                                                                         | 
| 03. Análisis Multivariado (VAR y VEC)                                                                   | 


## Objetivos generales
* Explicar y aplicar los principales fundamentos del análisis de series de tiempo.
* Estudiar los modelos de series de tiempo más relevantes
* Explicar la construcción y análisis de modelos de series de tiempo univariados y multivariados.
* Utilizar modelos de series de tiempo para desarrollar análisis de desarrollo económico

## Requisitos
Es deseable que las alumnos tengan como mínimo conocimientos básicos de R. Sin embargo, en el curso se van a proveer códigos completos de R para ser utilizados por parte de los alumnos.

Es recomendable que los alumnos además de tener instalado R y [RStudio](https://posit.co/download/rstudio-desktop/) tengan instalado el paquete de herramientas [Rtools](https://cran.r-project.org/bin/windows/Rtools/).

A las personas que no utilizan regularmente R se les recomienda desarrollar dee antemano los tutoriales de https://learnr.numbat.space/

## Dirigido a
Los contenidos presentados en este curso están dirigidos a estudiantes y profesionales de diferentes disciplinas que requieran aprender y/o fortalecer sus conocimientos en análisis de series de tiempo en R.

## Metodología
La primera sección del curso comienza con una explicación de lo que es el análisis de series de tiempo y de las caracteristicas principales de la base de datos "Indicadores de Desarrollo Económico" del Banco Mundial. Posteriormente, se explica cómo se puede descargar y manipular esta base directamente desde RStudio. Al final de la sección se hace una evaluación donde se formulan varias preguntas de selección múltiple con respecto a los conceptos vistos en la sección y con respecto a un ejercicio que el alumno debe desarrollar en RStudio utilizando la base de datos del Banco Mundial[^1]. 

[^1]: **Los códigos en R necesarios para el desarrollo del ejercicio serán explicados durante el desarrollo de la sección**

Las otras dos secciones del curso comienzan con una explicación general de las herramientas econométricas a utilizar. Luego, se desarrollan algunos ejercicios y posteriormente se deja un ejercicio para ser desarrollado por el alumno. Con respecto a este último se haran algunas preguntas de selección múltiple para evaluar si los conceptos principales han sido entendidos de forma apropiada.

## Sección 1 - Las Series de Tiempo, R y la Base de Datos del Banco Mundial
| Subsecciones                                                                                            | Contenido                                                                                                                | Dedicación,<br> 2 horas   | 
|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|:-------------------------:|
| [01. Series de tiempo](Seccion01/Seccion01_01/Readme.md)                                                |¿Qué es una serie de tiempo?, ¿En qué consiste el análisis de series de tiempo?, ¿Qué es una variable discreta aleatoria? |             0.5           | 
| [02. R y Los Indicadores de Desarrollo Económico del Banco Mundial](Seccion01/Seccion01_02/README.md)   |¿Cómo manipular los Indicadores de Desarrollo Económico del Banco Mundial en R                                            |             1.5           | 

## Sección 2 - Análisis Univariado (ARIMA)
| Subsecciones                                                                                        | Contenido                                                                                                      | Dedicación,<br> 10 horas  | 
|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|:-------------------------:|
| [01. Introducción](Seccion01/Seccion01_01/README.md)                                                | Introducción al análisis univariado de series de tiempo                                                        |             2.5           | 
| [02. Pruebas de Raíz Unitaria](Seccion02/Seccion02_02/Readme.md)                                    | Explicación general de la filosofía detrás de las pruebas de raíz unitaria                                     |             5.5           | 
| [03. Estimación de Modelos ARIMA](Seccion02/Seccion02_03/Readme.md)                                            | Explicación acerca de la metododología de Box-Jenkins para estimar un modelo ARIMA.                 |              1            | 
| [04. Principales pruebas de validación de los Modelos ARIMA](Section01/CaseStudy)                   | Explicación de las pruebas de validación de un modelo ARIMA                                                    |              1            | 


## Sección 3 - Análisis Multivariado (VAR y VEC)
| Subsecciones                                                                                        | Contenido                                                                                                      | Dedicación,<br> 5.5 horas | 
|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|:-------------------------:|
| [01. Introducción](Seccion01/Seccion01_01)                                                          | Introducción al análisis multivariado de series de tiempo                                                      |             2.5           | 
| [02. Estimación de Modelos VAR](Section01/Requirement)                                              | Explicación acerca de la metododología para estimar un modelo VAR.                                             |              1            | 
| [03. Principales pruebas de validación de los Modelos VAR](Section01/CaseStudy)                     | Explicación de las pruebas de validación de un modelo VAR                                                      |              1            | 
| [04. Prueba de Cointegración de Johansen](Section01/Requirement)                                    | Explicación acerca de la prueba de Cointegración de Johansen.                                                  |             0.5           | 
| [05. Estimación de Modelos VEC](Section01/Requirement)                                              | Explicación acerca de la metododología para estimar un modelo VEC.                                             |             1.5           | 
| [06. Principales pruebas de validación de los Modelos VEC](Section01/CaseStudy)                     | Explicación de las pruebas de validación de un modelo VEC                                                      |             1.5           | 

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
