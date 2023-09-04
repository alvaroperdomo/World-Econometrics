<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

<div align="center">
  <br><b>Universidad Escuela Colombiana de Ingeniería Julio Garavito</b><br>Alvaro Andrés Perdomo Strauch<br>Profesor del Centro de Estudios Oikonomía<br>alvaro.perdomo@escuelaing.edu.co<br>
</div>

### Entendiendo un Mundo en Desarrollo: 
# Análisis Aplicado de Series de Tiempo en $R$
Palabras Clave: `Econometría` `Series de Tiempo` `Indicadores de Desarrollo Mundial` `Banco Mundial`
#### Duración del curso: 17.5 horas

### ¡Bienvenido a un emocionante viaje de aprendizaje!

En este curso aprenderás a hacer análisis básicos de series de tiempo en $R$, utilizando $RStudio$ como tu entorno de desarrollo interactivo. Utilizaremos como fuente de información de referencia la base de datos del Banco Mundial "[_Indicadores de Desarrollo Mundial_](https://databank.worldbank.org/source/world-development-indicators)". Esta base proporciona datos económicos relevantes a nivel internacional, lo que te permitirá aplicar tus conocimientos de una manera práctica y enriquecedora.

## Objetivos
Al finalizar el curso estarás preparado para: 
* Explicar y aplicar los principales fundamentos del análisis de series de tiempo.
* Analizar los modelos más relevantes en el análisis de series de tiempo.
* Construir y analizar modelos de series de tiempo univariados y multivariados.
* Realizar análisis de series de tiempo utilizando la base de datos _Indicadores de Desarrollo Mundial_ en $R$.

## Requisitos
Para aprovechar al máximo este curso, se requiere que los alumnos tengan como mínimo conocimientos básicos de $R$ y de probabilidad. No obstante, hemos simplificado el proceso de aprendizaje al proporcionar códigos completos de $R$ para cada uno de los temas desarrollados. Además, es fundamental que los alumnos tengan instalados $R$ y [_**RStudio**_](https://posit.co/download/rstudio-desktop/), así como el paquete de herramientas [_**Rtools**_](https://cran.r-project.org/bin/windows/Rtools/).

## Dirigido a
Este curso está diseñado para estudiantes y profesionales de diversas disciplinas que deseen aprender y fortalecer sus habilidades en análisis de series de tiempo en $R$. Es especialmente relevante para aquellos interesados en hacer análisis econométricos de series de tiempo en contextos económicos y de desarrollo.

## Metodología
El curso está dividido en tres secciones principales, a través de las cuales desarrollarás diferentes habilidades computacionales y analíticas que podrás aplicar en el análisis de estudios de caso propios.

| Secciones                                                                                               |   
|---------------------------------------------------------------------------------------------------------|
| 01. Las Series de Tiempo, $R$ y los _Indicadores de Desarrollo Mundial_                                | 
| 02. Análisis Univariado de Series de Tiempo                                                             | 
| 03. Análisis Multivariado de Series de Tiempo                                                           | 

La primera sección comienza con una introducción a las series de tiempo y a la base de datos "_Indicadores de Desarrollo Mundial_". Aprenderás a descargar y manipular esta base directamente desde $RStudio$. Las demás secciones del curso comienzan con una explicación general de las herramientas econométricas a utilizar, tanto en lo teórico como en su aplicación en $R$. A continuación, se desarrollan ejercicios empíricos utilizando la base de datos "_Indicadores de Desarrollo Mundial_".[^1] 

[^1]: **Los códigos en _R_ necesarios para el desarrollo de los ejercicios serán explicados durante el desarrollo de las secciones respectivas**

En todas las secciones, al final de cada subsección, encontrarás preguntas de selección múltiple. Estas preguntas son útiles para evaluar tu comprensión de los conceptos, aunque no son calificables a menos que estés interesado en obtener un certificado del curso.

## Estructura del curso
A continuación, encontrarás información más detallada acerca de cada una de las secciones en que se divide el curso. Si quieres ir a alguna sección en particular, dale _click_ con el mouse al título de la sección respectiva. 

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
| **3.1. Estimación de Modelos $VAR$**                                                                   | **Metodología para estimar un modelo $VAR$**   |              4            | 
| **3.1.1.** ¿Qué es un modelo $VAR$?, <br> ¿Cómo estimar y hacer pronósticos de un modelo $VAR$ en $R$? | Teoría y aplicación en $R$                    |                           | 
| **3.1.2.** Pruebas de Hipótesis en Modelos $VAR$                                                       | Teoría y aplicación en $R$                    |                           |                        
| **3.1.3.** La función impulso-respuesta y la descomposición de varianza del error de pronóstico        | Teoría y aplicación en $R$                    |                           |            
| **3.1.4** Caso de estudio de un modelo $VAR$ utilizando la base de datos _Indicadores de Desarrollo Mundial_         | Aplicación en $R$               |                           | 
| **3.2. Cointegración y estimación de Modelos $VEC$**                                                   | **Metodología para estimar un modelo $VEC$.** |              4            | 
| **3.2.1.** ¿Qué es y para qué sirve la Cointegración?                                                  | Explicación teórica                           |                           | 
|  **3.2.2.** La Metodología de Engle y Granger (**2 variables**): ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$? |  Teoría y aplicación en $R$    |
| **3.2.3.** La Metodología de Johansen (**2 o más variables**): ¿Cómo evaluar la presencia de cointegración? ¿Cómo estimar un Modelo $VEC$?   |  Teoría y aplicación en $R$    |
| **3.2.4.** Pruebas de hipótesis adicionales para implementar en un modelo $VEC$                        | Teoría y aplicación en $R$                    |                           |
| **3.2.5.** Caso de estudio de un modelo $VEC$ utilizando la base de datos _Indicadores de Desarrollo Mundial_         | Aplicación en $R$                             |                           | 

## ¿Cómo obtener un certificado, avalado por la Universidad Escuela Colombiana de Ingeniería Julio Garavito?

Si deseas obtener un certificado oficial por haber participado en el curso, sigue estos pasos:
1) Haz _click_ en el botón "Obtener el certificado" que encontrarás al final de esta página electrónica. Sigue las instrucciones proporcionadas y realiza el pago correspondiente. Una vez completado el proceso, recibirás un enlace personal e intransferible.
2) Este enlace te dará acceso a una cuenta en Moodle, donde podrás responder de manera interactiva a todas las preguntas de selección planteadas. Estas preguntas están diseñadas para evaluar tu comprensión de los conceptos clave abordados en el curso.

Si logras obtener una nota mínima de 70 al responder todas las preguntas, procederemos a tramitar tu certificado oficial avalado por la Universidad Escuela Colombiana de Ingeniería Julio Garavito.

  
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
