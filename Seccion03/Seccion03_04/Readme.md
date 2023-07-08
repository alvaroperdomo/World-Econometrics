# Cointegración

En los modelos univariados, hemos visto que una tendencia estocástica puede eliminarse por diferenciación. Las series estacionarias resultantes podían estimarse utilizando técnicas univariadas de Box-Jenkins. En otros tiempos, la práctica convencional era generalizar esta idea y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión. Sin embargo, la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria; se dice que tales variables están cointegradas. 

**En presencia de variables cointegradas, es posible modelar el modelo de largo plazo y la dinámica de corto plazo simultáneamente.** 

Engle y Granger (1987) fueron los primeros en hablar acerca de la cointegración. Para entender su significado, considere el siguiente ejemplo:

Asuma que las variables $x_{1t}$, $x_{2t}$, $x_{3t}$ y $x_{4t}$ son variables $I(1)$ no estacionarias y que conforman un verctor $x=$


