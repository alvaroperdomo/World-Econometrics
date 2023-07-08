# Cointegración

En los modelos univariados, hemos visto que una tendencia estocástica puede eliminarse por diferenciación. Las series estacionarias resultantes podían estimarse utilizando técnicas univariadas de Box-Jenkins. En otros tiempos, la práctica convencional era generalizar esta idea y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión. Sin embargo, la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria; se dice que tales variables están cointegradas. 

**En presencia de variables cointegradas, es posible modelar el modelo de largo plazo y la dinámica de corto plazo simultáneamente.** 

Engle y Granger (1987) fueron los primeros en hablar acerca de la cointegración. Para entender su significado, considere el siguiente ejemplo:

Asuma que las variables $x_{1t}$, $x_{2t}$, $x_{3t}$, ..., $x_{4t}$ son variables $I(1)$ no estacionarias y que conforman un vector $x_t=$

++++++++++++++++++++++++++++


Hay cuatro puntos importantes a tener en cuenta sobre la definición:
1) **La cointegración generalmente se refiere a una combinación lineal de variables no estacionarias**.

El vector de cointegración no es único. Si ${\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ es un vector de cointegración, entonces para cualquier $\lambda≠0$, ${\left\lbrack \matrix{\lambda\beta_1 & \lambda\beta_2 & ... & \lambda\beta_n} \right\rbrack}$ también es un vector de cointegración. Normalmente, una de las variables se usa para normalizar el vector de cointegración fijando su coeficiente en $1$. Para normalizar el vector de cointegración con respecto a $x_{1t}$, simplemente seleccione $\lambda=\frac{1}{\beta_1}$.

2) En la definición original de Engle y Granger, la cointegración se refiere a variables que están integradas en el mismo orden. Esto no implica que todas las variables integradas estén cointegradas; por lo general, un conjunto de variables $I(d)$. Tal falta de cointegración implica que no hay un equilibrio a largo plazo entre las variables, de modo que puedan desviarse arbitrariamente una de la otra. 

Si dos variables son integradas de órdenes diferentes, no pueden estar cointegradas. Suponga que $x_1t$ es $I(d_1)$ y $x_{2t}$ es $I(d_2)$ donde $d_2>d_1$ . Entonces, cualquier combinación lineal de $x_{1t}$ y $x_{2t}$ es $I(d_2)$. Sin embargo, es posible encontrar relaciones de equilibrio entre grupos de variables que están integradas de diferentes órdenes. Supongamos que $x_{1t}$ y $x_{2t}$ son $I(2)$ y que las otras variables en consideración son $I(1)$. Como tal, no puede haber una relación de cointegración entre $x_{1t}$ (o $x_{2t}$) y $x_{3t}$. Sin embargo, si $x_{1t}$ y $x_{2t}$ son $CL(2,1)$, existe una combinación lineal de la forma $\beta_1x_{1t}+\beta_2x_{2t}$ que es $I(1)$. Es posible que esta combinación de $x_{1t}$ y $x_{2t}$ esté cointegrada con las variables $I(1)$. 

3) **Puede haber más de un vector de cointegración independiente para un conjunto de variables $I(1)$. El número de vectores de cointegración se denomina rango de cointegración de $x_t$**. 

Si $x_t$ tiene $n$ componentes no estacionarios, puede haber hasta $n-1$ vectores de cointegración linealmente independientes. Por lo tanto, si $x_t$ contiene solo dos variables, puede haber a lo sumo un vector de cointegración independiente.

4) La mayor parte de la literatura sobre cointegración se centra en el ca-so en el que cada variable tiene una sola raíz unitaria. La razón es que la regresión tradicional o el análisis de series de tiempo se aplica cuando las variables son $I(0)$ y pocas variables económicas están integradas en un orden superior a $1$. 

# Pruebas de Cointegración




