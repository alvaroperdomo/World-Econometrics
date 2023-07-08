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

## Pruebas de Cointegración

Para el análisis de cointegración las metodologías más utilizadas son la de Engle y Granger (1987) y la de Johansen (1988)
 
### La Metodología de Engle-Granger

Para explicar el procedimiento de prueba de Engle-Granger, comencemos con el tipo de problema que es usual en los estudios aplicados. Suponga que dos variables, digamos $y_t$ y $z_t$, se creen integradas de orden $1$ y se desea determinar si existe una relación de equilibrio entre las dos. Engle y Granger (1987) proponen un procedimiento de cuatro pasos para determinar si dos variables $I(1)$ están cointegradas de orden $CI(1,1):

1) **Haga una prueba preliminar de las variables para conocer su orden de integración.** 

Por definición, la cointegración requiere que dos variables estén integradas en el mismo orden. Por lo tanto, el primer paso en el análisis es probar cada variable para determinar su orden de integración. Si ambas variables son estacionarias, no es necesario continuar ya que los métodos estándar de series de tiempo se aplican a las variables estacionarias. Si las variables están integradas de diferentes órdenes, es posible concluir que no están cointegradas. Sin embargo, si tiene más de dos variables, de manera que algunas son $I(1)$ y otras son $I(2)$, es posible que desee determinar si las variables están integradas en forma múltiple.

2) **Estime la relación del equilibrio a largo plazo.** 

Si los resultados indican que tanto { $y_t$ } como { $z_t$ } son $I(1)$, estime la relación de equilibrio: $y_t=\beta_0+\beta_1z_t+e_t$. Si las variables están cointegradas, una regresión de Mínimos Cuadrados Ordinarios produce un estimador "superconsistente" de los parámetros de cointegración $\beta_0$ y $\beta_1$. 

Para determinar si las variables están realmente cointegradas, sea { $\hat{e_t}$ } la secuencia de residuos de esta ecuación. Por lo tanto, la serie { $\hat{e_t}$ } contiene los valores estimados de las desviaciones de la relación a largo plazo. Si se encuentra que estas desviaciones son estacionarias, las secuencias { $y_t$ } y { $z_t$ } son cointegradas de orden $(1,1)$. 

Sería conveniente si pudiéramos realizar una prueba ADF de estos residuos para determinar su orden de integración. Considere la regresión de los residuos: $\Delta\hat{e}=a_1\hat{e_{t-1}}+\varepsilon_t$. Dado que la secuencia { $\hat{e_t}$ } es el residuo de una regresión (con una media necesariamente igual a cero), no es necesario incluir un intercepto; el parámetro de interés es $a_1$. 
* Si no podemos rechazar la hipótesis nula $a_1=0$, podemos concluir que los residuos tienen una raíz unitaria. Por lo tanto, concluimos que las secuencias { $y_t$ } y { $z_t$ } no están cointegradas. 
* En cambio, el rechazo de la hipótesis nula implica que la secuencia de residuos es estacionaria. Dado que se encontró que { $y_t$ } y { $z_t$ } son $I(1)$ y que los residuos son estacionarios, podemos concluir que las series están cointegradas de orden $(1,1)$.

En la mayoría de los estudios aplicados, no es posible utilizar las tablas de Dickey-Fuller. El problema es que la secuencia { $\hat{e_t}$ } se genera a partir de una regresión; el investigador no conoce el error real $e_t$, solo el error estimado { $\hat{e_t}$ }.[^1]

[^1]: **Engle y Granger (1987) propusieron nuevas tablas para hacer los cálculos. Y estas posteriormente fueron actualizadas por MacKinnon (1990)**




