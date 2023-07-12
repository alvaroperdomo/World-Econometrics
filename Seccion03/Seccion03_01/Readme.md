## SECCIÓN 3.1:
# Modelos de Vectores Autorregresivos $VAR$
Si se quiere tener en cuenta todas las relaciones posibles entre, digamos, $k$ variables, parece sensato construir un modelo conjunto que incluya todas estas series de tiempo en lugar de construir modelos para cada una de las series individuales, a este tipo de modelos se les llama modelos $VAR$. A continuación, dando click en la **X** de la segunda columna de la siguiente tabla se redirigira a la explicación de cada uno de los aspectos teóricos que se encuentran en la primera columna de la tabla. Por otra parte, dando click en la **X** de la tercera columna de la tabla podra ver las aplicaciones en R de cada uno de estos temas:

| Temas                                                                                                                                                                  | Explicación teórica                   |  Aplicación en R                     |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------:|:------------------------------------:|
| 1. ¿Qué es un modelo VAR?, ¿Cuáles son los tipos de modelos VAR?, ¿Cómo hacer pronósticos con un modelo VAR?, ¿Cómo estimar y hacer pronosticos de un modelo VAR en R? |  [X](Seccion03_01_01_T/Readme.md)    | [X](Seccion03_01_01_R/Readme.md)    | 
| 2. Pruebas de Hipótesis en Modelos VAR                                                                                                                                 |  [X](Seccion03_01_02_T/Readme.md)     | [X](Seccion03_01_02_R/Readme.md)     |
| 3. La Función Impulso Respuesta y la Descomposición de Varianza                                                                                                        |  [X](Seccion03_01_03_T/Readme.md) | [X](Seccion03_01_03_R/Readme.md) | 

A continuación, se presenta un ejemplo utilizando la base de datos "Indicadores de Desarrollo Económico del Banco Mundial". Para el ejercicio, se ha escogido analizar la variable más utilizada en términos de desarrollo económico, el PIB per cápita, de un país en vías de desarrollo. Más específicamente, se va a analizar la evolución del PIB per cápita de Colombia a precios constantes.


<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

