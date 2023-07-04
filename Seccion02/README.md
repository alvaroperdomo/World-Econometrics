# Introducción
En esta sección se van a revisar las técnicas de análisis univariado de series de tiempo, en particular:
* Se van a explicar los aspectos teóricos más relevantes del análisis univariado de series de tiempo
* Se va explicar la forma cómo se utiliza R para hacer un análisis univariado de series de tiempo
* Se van a desarrollar ejemplos empíricos utilizando la base de datos "_Indicadores de Desarrollo Económico_" del Banco Mundial
  * Más específicamente, durante el desarrollo de la sección **utilizaremos el _PIB per cápita de Colombia a precios constantes durante el periodo 1960-2019_ para calcular las proyecciones de esta variable durante el periodo 2020-2025 si no se hubiera presentado la pandemia del Covid 19**.
  * Ello no quita que se puedan utilizar los datos de 1960-2022 para proyectar el PIB per cápita de Colombia a precios constantes durante el periodo 2023-2025. Sin embargo, las pruebas de raíz unitaria en términos didácticos resultan un poco más engorrosas de interpretar a raíz del choque temporal que tuvó el Covid-19 (Ver gráfico de abajo)

   ![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/ec783053-9f06-4834-9983-9158350145b8)
  
<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>



