# Cointegración

En los modelos univariados, hemos visto que una tendencia estocástica puede eliminarse por diferenciación. Las series estacionarias resultantes podían estimarse utilizando técnicas univariadas de Box-Jenkins. En otros tiempos, la práctica convencional era generalizar esta idea y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión. Sin embargo, la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria; se dice que tales variables están cointegradas. 

**En presencia de variables cointegradas, es posible modelar el modelo de largo plazo y la dinámica de corto plazo simultáneamente.** 

Engle y Granger (1987) fueron los primeros en hablar acerca de la cointegración.

Su análisis formal comienza considerando un conjunto de variables económicas en equilibrio a largo plazo cuando $\eqalign{\sum_{i=1}^n \beta_ix_{it}=0}$ 

Sea 𝛽=[■8(𝛽_1&■8(𝛽_2&⋯&𝛽_𝑛 ))] y 𝑥_𝑡=[■8(𝑥_1𝑡@■8(𝑥_2𝑡@⋮@𝑥_𝑛𝑡 ))], el sistema está en equilibrio a largo plazo cuando 𝛽𝑥_𝑡= 0. 
La desviación del equilibrio a largo plazo, llamado error de equilibrio es 𝑒_𝑡, por lo tanto 𝑒_𝑡=𝛽𝑥_𝑡.
Si el equilibrio es significativo, 𝑒_𝑡  debe ser estacionario. 
