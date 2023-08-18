<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

<div align="center">
  <br><b>Universidad Escuela Colombiana de Ingeniería Julio Garavito</b><br>Alvaro Andrés Perdomo Strauch<br>Profesor del Centro de Estudios Oikonomía<br>alvaro.perdomo@escuelaing.edu.co<br>
</div>

### Entendiendo un Mundo en Desarrollo: 
# Análisis Aplicado de Series de Tiempo en $R$
Keywords: `Econometría` `Series de Tiempo` `Indicadores de Desarrollo Mundial` `Banco Mundial`
#### Duración del curso: 17.5 horas

Bienvenido al curso _Entendiendo un Mundo en Desarrollo: Análisis Aplicado de Series de Tiempo en_ $R$. En este curso aprenderá a hacer análisis básicos de series de tiempo en $R$ utilizando como entorno de desarrollo interactivo al software $RStudio$. Se tomará como referencia, para el análisis empírico, la base de datos del Banco Mundial "[_Indicadores de Desarrollo Mundial_](https://databank.worldbank.org/source/world-development-indicators)", con el fin de aprender las principales técnicas del análisis de series de tiempo de una forma aplicada utilizando la información económica más relevante del contexto internacional.

El curso está dividido en tres secciones principales, a través de las cuales el estudiante desarrollará diferentes habilidades computacionales y analíticas, que podrá aplicar en el análisis de estudios de caso propios.

| Secciones                                                                                               |   
|---------------------------------------------------------------------------------------------------------|
| 01. Las Series de Tiempo,  $R$ y los _Indicadores de Desarrollo Mundial_                                | 
| 02. Análisis Univariado de Series de Tiempo                                                             | 
| 03. Análisis Multivariado de Series de Tiempo                                                           | 


## Objetivos
* Explicar y aplicar los principales fundamentos del análisis de series de tiempo.
* Estudiar los modelos más relevantes en el análisis de series de tiempo.
* Explicar la construcción y análisis de modelos de series de tiempo univariados y multivariados.
* Realizar análisis de series de tiempo utilizando la base de datos _Indicadores de Desarrollo Mundial_ en $R$.

## Requisitos
Se requiere que los alumnos tengan como mínimo conocimientos básicos de $R$ y de probabilidad. Sin embargo, en el curso se van a proveer códigos completos de $R$ para ser utilizados por parte de los alumnos.

Es importante que los alumnos además de tener instalados $R$ y [_**RStudio**_](https://posit.co/download/rstudio-desktop/), también tengan instalado el paquete de herramientas [_**Rtools**_](https://cran.r-project.org/bin/windows/Rtools/).

## Dirigido a
Los contenidos presentados en este curso están dirigidos a estudiantes y profesionales de diferentes disciplinas que requieran aprender y/o fortalecer sus conocimientos en análisis de series de tiempo en $R$.

## Metodología
La primera sección del curso comienza con la explicación de algunas generalidades el análisis de series de tiempo y de las características principales de la base de datos "_Indicadores de Desarrollo Mundial_". Posteriormente, se explica cómo se puede descargar y manipular esta base directamente desde $RStudio$. Al final de la sección se hace una evaluación donde se formulan preguntas de selección múltiple con respecto a los conceptos vistos en esta y con respecto a un ejercicio que el alumno debe desarrollar en $RStudio$ utilizando los _Indicadores de Desarrollo Mundial_ [^1]. 

[^1]: **Los códigos en _R_ necesarios para el desarrollo del ejercicio serán explicados durante el desarrollo de la sección**

Las otras dos secciones del curso comienzan con una explicación general de las herramientas econométricas a utilizar, tanto en lo teórico como en su aplicación en $R$. Luego, se desarrollan algunos ejercicios y posteriormente se propone un ejercicio empírico para ser realizado por el alumno. Durante el desarrollo de las secciones se harán algunas preguntas de selección múltiple para evaluar si los conceptos principales han sido entendidos de forma apropiada.

A continuación encontrará información más detallada acerca de cada una de las secciones en que se divide el curso. Si quiere ir a alguna sección en particular, dele _click_ con el mouse al título de la sección respectiva. 

## [Sección 1 - Las Series de Tiempo, _R_ y los _Indicadores de Desarrollo Mundial_](Seccion01/Readme.md)
| Subsecciones                                             | Contenido                                                                           | Dedicación,<br> 2.5 horas | 
|----------------------------------------------------------|-------------------------------------------------------------------------------------|:-------------------------:|
| **1.1. Series de tiempo**                                | **Generalidades acerca del análisis de series de tiempo**                           |             0.5           | 
| **1.2. $R$ y los _Indicadores de Desarrollo Mundial_**   | **¿Cómo manipular los _Indicadores de Desarrollo Mundial_ en $R$?**                 |             2.0           | 

## [Sección 2 - Análisis Univariado de Series de Tiempo](Seccion02/Readme.md)
| Subsecciones                                           | Contenido                                                                                               | Dedicación,<br> 7 horas     
|--------------------------------------------------------|------------------------------------------------------------------------------------------------------   |:-------------------------:|
| **2.1. Series Estacionarias y No Estacionarias**       | **Importancia de las series estacionarias en el análisis univariado de series de tiempo**               |              1            | 
| **2.2 Pruebas de Raíz Unitaria**                       | **Metodología para aplicar e interpretar diferentes tipos de pruebas de raíz unitaria**                 |              3            | 
| **2.2.1.** Las Pruebas $DF$ y $ADF$: _Pruebas de Dickey Fuller y Aumentada de Dickey Fuller_ | Teoría y aplicación en $R$                                        |                           | 
| **2.2.2.** La Prueba $ADF-GLS$: _Prueba de Elliott, Rothenberg y Stock_                    | Teoría y aplicación en $R$                                          |                           | 
| **2.2.3.** La Prueba $KPSS$: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_             | Teoría y aplicación en $R$                                          |                           |   
| **2.2.4.** Pruebas de Cambio Estructural: _Prueba de Perron y Prueba de Zivot y Andrews_ | Teoría y aplicación en $R$                                            |                           | 
| **2.2.5.** Caso de estudio utilizando la base de datos _Indicadores de Desarrollo Mundial_ (primera parte) | Aplicación en $R$                                   |                           | 
| **2.3. Análisis $ARMA$ (Metodología de Box-Jenkins)** | **Metodología para estimar un modelo $ARMA$**                                                            |              3            | 
| **2.3.1.** Las tres etapas de la metodología de Box-Jenkins                      | Teoría y aplicación en $R$                                                    |                           | 
| **2.3.2.** Estimación de un $ARMA$ - Ejemplo simulado                            | Aplicación en $R$                                                             |                           | 
| **2.3.3.** Caso de estudio utilizando la base de datos _Indicadores de Desarrollo Mundial_ (segunda parte) | Aplicación en $R$                                   |                           | 

## [Sección 3 - Análisis Multivariado de Series de Tiempo](Seccion03/Readme.md)
| Subsecciones                                                                                           | Contenido                                     | Dedicación,<br> 8 horas | 
|--------------------------------------------------------------------------------------------------------|------------------------------------------------|:-------------------------:|
| **3.1. Estimación de Modelos $VAR$**                                                                   | **Metododología para estimar un modelo $VAR$** |              4            | 
| **3.1.1.** ¿Qué es un modelo $VAR$?, <br> ¿Cómo estimar y hacer pronosticos de un modelo $VAR$ en $R$? | Teoría y aplicación en $R$                    |                           | 
| **3.1.2.** Pruebas de Hipótesis en Modelos $VAR$                                                       | Teoría y aplicación en $R$                    |                           |                        
| **3.1.3.** La función impulso-respuesta y la descomposición de varianza del error de pronóstico        | Teoría y aplicación en $R$                    |                           |            
| **3.1.4** Caso de estudio de un modelo $VAR$ utilizando la base de datos _Indicadores de Desarrollo Mundial_         | Aplicación en $R$               |                           | 
| **3.2. Cointegración y estimación de Modelos $VEC$**                                                   | **Metodología para estimar un modelo $VEC$.** |              4            | 
| **3.2.1.** ¿Qué es y para qué sirve la Cointegración?                                                  | Explicación teórica                           |                           | 
|  **3.2.2.** La Metodología de Engle y Granger (**2 variables**): ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$? |  Teoría y aplicación en $R$    |
| **3.2.3.** La Metodología de Johansen (**2 o más variables**): ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$?   |  Teoría y aplicación en $R$    |
| **3.2.4.** Pruebas de hipótesis adicionales para implementar en un modelo $VEC$                        | Teoría y aplicación en $R$                    |                           |
| **3.2.5.** Caso de estudio de un modelo $VEC$ utilizando la base de datos _Indicadores de Desarrollo Mundial_         | Aplicación en $R$                             |                           | 

  
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
