# Pruebas de Hipótesis

## ¿Cuáles variables incluir en un $VAR$?
En principio, no hay nada que impida incorporar una gran cantidad de variables en el $VAR$. Es posible construir un $VAR$ de $n$ ecuaciones con cada ecuación conteniendo $p$ rezagos de todas las $n$ variables en el sistema. Por lo general, uno quiere incluir aquellas variables que tienen importantes efectos económicos entre sí. No obstante, tenga en cuenta que los grados de libertad disminuyen rápidamente a medida que se incluyen más variables. [^1] 

[^1]: **Por ejemplo, al usar datos con 5 rezagos, la inclusión de una variable adicional usa 5 grados adicionales de libertad en cada ecuación.** 

Un examen cuidadoso del modelo teórico relevante lo ayudará a seleccionar el conjunto de variables para incluir en su modelo $VAR$. Un $VAR$ de $n$ ecuaciones puede ser representado por:

$$ {\left\lbrack \matrix{ x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack} = \left\lbrack \matrix{ A_{10} \cr A_{20} \cr \dots \cr A_{n0} } \right\rbrack + \left\lbrack \matrix{A_{11}(L) & A_{12}(L) & \dots & A_{1n}(L) \cr A_{21}(L) & A_{22}(L) & \dots & A_{2n}(L) \cr \dots & \dots & \dots & \dots \cr A_{n1}(L) & A_{n2}(L) & \dots & A_{nn}(L) } \right\rbrack  \left\lbrack \matrix{ x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack + \left\lbrack \matrix{ e_{1t} \cr e_{2t} \cr \dots \cr e_{nt} } \right\rbrack
$$

* los $A_{i0}$ son los interceptos,
* los $A_{ij}(L)$ son los polinomios en el operador de rezagos $L$. Los coeficientes individuales de los $A_{ij}(L)$ se denotan por $a_{ij}(1), a_{ij}(2), \dots$. Como todas las ecuaciones tienen la misma longitud de rezagos, los polinomios $A_{ij}(L)$ son todos del mismo grado.
* Los términos $e_{1t}$ son perturbaciones ruido blanco que pueden estar correlacionadas entre sí.
* La matriz de varianzas y covarianza $\mathbf{\Sigma}$ tiene dimensión ($n \times n$)

## ¿Cuántos rezagos incluir en un $VAR$?
Además de la determinación del conjunto de variables a incluir en el $VAR$, es importante determinar la longitud apropiada de rezagos. Un posible procedimiento es permitir diferentes longitudes de rezago para cada variable en cada ecuación. Sin embargo, para preservar la simetría del sistema (y poder usar $MCO$ de manera eficiente), es común usar la misma longitud de rezagos para todas las ecuaciones. [^2] 

[^2]: **Siempre que haya regresores idénticos en cada ecuación, las estimaciones _MCO_ son consistentes y asintóticamente eficientes.** 

Si algunas de las ecuaciones del $VAR$ tienen regresores no incluidos en las otras, las regresiones aparentemente no relacionadas ($SUR$ por sus siglas en inglés) proporcionan estimaciones eficientes de los coeficientes $VAR$. Por lo tanto, cuando hay una buena razón para dejar que las longitudes de los rezagos difieran entre las ecuaciones, estime un $near-VAR$ utilizando un $SUR$.

En un $VAR$, los rezagos consumen rápidamente los grados de libertad. Si la longitud del rezago es $p$, cada una de las $p$ ecuaciones contiene $np$ coeficientes más el intercepto. La selección apropiada de la longitud de rezagos puede ser crítica. 
* Si $p$ es demasiado pequeño, el modelo está mal especificado;
* si $p$ es demasiado grande, se pierden grados de libertad. 

Para determinar la longitud del rezago,
1) comience con la longitud plausible más larga o la longitud más larga posible dadas las consideraciones de grados de libertad.
2) Estime el $VAR$ y forme la matriz de varianzas y covarianzas de los residuos.

Por ejemplo, puede comenzar con un rezago de $3$ años basado en la noción a priori de que este tiempo es lo suficientemente largo para capturar la dinámica del sistema. La matriz de varianzas y covarianzas de los residuos del modelo a $3$ rezagos es $\mathbf{\Sigma_3}$. Ahora suponga que quiere determinar si $2$ rezagos son apropiados.  Después de todo, restringir el modelo de $3$ a $2$ rezagos reduciría el número de parámetros estimados en $n$ en cada ecuación. La prueba adecuada para esta restricción de ecuación cruzada es una prueba de razón de verosimilitud. Vuelva a estimar el $VAR$ durante el mismo período de muestra utilizando $2$ rezagos y obtenga la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_2}$. Tenga en cuenta que $\mathbf{\Sigma_2}$ pertenece a un sistema de $n$ ecuaciones con $n$ restricciones en cada ecuación, para un total de $n^2$ restricciones. El estadístico de la razón de verosimilitud es $T(\ln{|\mathbf{\Sigma_2}|}-\ln{|\mathbf{\Sigma_3}|})$. Sin embargo, dados los tamaños de muestra que generalmente se encuentran en el análisis económico, Sims (1980) recomendó usar $(T-c)(\ln{|\mathbf{\Sigma_2}|}-\ln{|\mathbf{\Sigma_3}|})$ donde 
* $T$ es el número de observaciones utilizables,
* $c$ el número de parámetros estimados en cada ecuación del sistema no restringido, y
* $\ln{|\mathbf{\Sigma_n}|}$ es el logaritmo natural del determinante de $\mathbf{\Sigma_n}$

En el ejemplo que nos ocupa, $c=1+3n$  ya que cada ecuación del modelo no restringido tiene $3$ rezagos para cada variable más un intercepto

Este estadístico tiene una distribución asintótica $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. En el ejemplo considerado, hay $n$ restricciones en cada ecuación, para un total de restricciones $n^2$ en el sistema. Si la restricción de un número reducido de rezagos no es vinculante, esperaríamos que $\ln{|\mathbf{\Sigma_2}|}=\ln{|\mathbf{\Sigma_3}|}$. Los valores grandes de este estadístico muestral indican que tener sólo $2$ rezagos es una restricción vinculante; por lo tanto, podemos rechazar la hipótesis nula de que la longitud de rezagos es igual a $2$. Si el valor calculado del estadístico es menor que el $\chi^2$ en un nivel de significación preespecificado, no podremos rechazar la hipótesis nula de sólo $2$ rezagos. En ese momento, podríamos tratar de determinar si $1$ rezagos eran apropiados construyendo $(T-c)(\ln{|\mathbf{\Sigma_1}|}-\ln{|\mathbf{\Sigma_2}|})$. Se debe tener mucho cuidado al reducir la longitud del rezago de esta manera. A menudo, este procedimiento no rechazará las hipótesis nulas de $2$ versus $3$ rezagos y $1$ versus $2$ rezagos, aunque rechazará una hipótesis nula de $1$ versus 43$ rezagos. El problema con reducir el modelo es que puede perder una pequeña cantidad de poder explicativo en cada etapa. En general, la pérdida total en el poder explicativo puede ser significativa. En tales circunstancias, es mejor utilizar las longitudes de rezago más largas.

Este tipo de prueba de razón de verosimilitud es aplicable a cualquier tipo de restricción de ecuación cruzada. Sean $\mathbf{\Sigma_u}$ y $\mathbf{\Sigma_r}$ las matrices de varianzas y covarianzas de los sistemas no restringidos y restringidos, respectivamente. Si las ecuaciones del modelo no restringido contienen regresores diferentes, vamos a denotar el número máximo de regresores contenidos en la ecuación más larga. La recomendación de Sims es comparar el estadís-tico de prueba $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ con respecto a una distribución $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. La prueba de razón de verosimilitud se basa en la teoría asintótica, que puede no ser muy útil en las muestras pequeñas disponibles para los econometristas de series de tiempo. 

Además, la prueba de razón de verosimilitud sólo es aplicable cuando un modelo es una versión restringida del otro. Los criterios de prueba alternativos son las generalizaciones multivariadas del Criterio de Información de Akaike (AIC por sus siglas en inglés) y el Criterio Bayesiano de Schwartz (BSC por sus siglas en inglés):
* $AIC=T \ln{|\mathbf{\Sigma}|}+2N$
* $BSC=T \ln{|\mathbf{\Sigma}|}+N\ln{T}$ 
donde
* $|\mathbf{\Sigma}|$ es el determinante de la varianza matriz de varianzas y covarianzas de los residuos
* $N$ es el número total de parámetros estimados en todas las ecuaciones. 

Por lo tanto, si cada ecuación en un $VAR$ de $n$ variables tiene $p$ rezagos y un intercepto, $N=n^2p+n$; cada una de las $n$ ecuaciones tiene $np$ regresores rezagados y un intercepto.La adición de regresores adicionales reducirá $|\mathbf{\Sigma}|$ a expensas de aumentar $N$. Como en el caso univariado, seleccione el modelo con el valor más bajo de $AIC$ o $BSC$. 

Tenga en cuenta que el Criterio de Información de Akaike y el Criterio Bayesiano de Schwartz multivariados no se pueden usar para probar la importancia estadística de modelos alternativos. En cambio, son medidas del ajuste general de las alternativas. 

Una prueba de causalidad establece si los rezagos de una variable entran en la ecuación de otra variable. En un modelo de dos ecuaciones con $p$ rezagos, { $y_t$ } no causa en el sentido de Granger a { $z_t$ } si y solo si todos los coeficientes de $A_{21}(L)$ son nulos. Por lo tanto, si { $y_t$ } no mejora el rendimiento de pronóstico de { $z_t$ }, entonces { $y_t$ } no causa en el sentido de Granger a { $z_t$ }. Si todas las variables en el $VAR$ son estacionarias, la forma directa de probar la causalidad de Granger es usar una prueba $F$ estándar de la restricción $a_21(1)=a_21(2)=a_21(3)= \dots = a_21(p)=0$. 

Es sencillo generalizar esta noción al caso de $n$ variables

$$ {\left\lbrack \matrix{ x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack} = \left\lbrack \matrix{ A_{10} \cr A_{20} \cr \dots \cr A_{n0} } \right\rbrack + \left\lbrack \matrix{A_{11}(L) & A_{12}(L) & \dots & A_{1n}(L) \cr A_{21}(L) & A_{22}(L) & \dots & A_{2n}(L) \cr \dots & \dots & \dots & \dots \cr A_{n1}(L) & A_{n2}(L) & \dots & A_{nn}(L) } \right\rbrack  \left\lbrack \matrix{ x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack + \left\lbrack \matrix{ e_{1t} \cr e_{2t} \cr \dots \cr e_{nt} } \right\rbrack
$$

Dado que $A_{ij}(L)$ representa los coeficientes de los valores rezagados de la variable $j$ en la variable $i$, la variable $j$ no causa la variable $i$ en el sentido de Granger si todos los coeficientes del polinomio $A_{ij}(L)$ pueden establecerse iguales a cero.

La causalidad de Granger es algo muy diferente de una prueba de exogeneidad. Para que $z_t$ sea exógena, requerimos que no se vea afectada por el valor contemporáneo de $y_t$. Sin embargo, la causalidad de Granger se refiere sólo a los efectos de valores pasados de { $y_t$ } en el valor actual de $z_t$. Por lo tanto, la causalidad de Granger en realidad mide si los valores actuales y pasados de { $y_t$ } ayudan a pronosticar valores futuros de { $z_t$ }. 

Para ilustrar la distinción en términos de un modelo $VMA$, considere la siguiente ecuación tal que $y_t$ no causa en el sentido de Granger $z_t$ y a pesar de todo $z_t$ no es exógeno $\eqalign{z_t=\bar{z} +\phi_{21}(0) \varepsilon_{yt} + \sum_{i=0}^{\infty} \phi_{22}(i) \varepsilon_{z(t-i)} }$

Si pronosticamos $z_{t+1}=$ condicionado a los valores de $\varepsilon_{z(t-i)}$ ($i=0, 1, \dots$) solo, obtendremos el error de pronóstico $\phi_{21}(0) \varepsilon_{y(t+1)} + \phi_{22}(1) \varepsilon_{z(t+1)}$. Sin embargo, obtenemos el mismo error de pronóstico si pronosticamos $z_{t+1}$ condicionado en $\varepsilon_{z(t-1)} y $\varepsilon_{y(t-1)}$ ($i=0, 1, \dots$). Dado el valor de $z_t$, la información sobre $y_t$ no ayuda a reducir el error de pronóstico para $z_{t+1}. 

En otras palabras, para el modelo en consideración, $E_t(z_{t+1}|z_t)=E_t(z_{t+1}|z_t, y_t). Por lo tanto, { $y_t$ } no causa { $z_t$ } en el sentido de Granger. 

Por otro lado, dado que estamos asumiendo que $\phi_{21}(0) \not= 0$, { $z_t$ } no es exógeno. Si $\phi_{21}(0) \not= 0$, los choques puros de $y_{t+1}$ (es decir, $\varepsilon_{y(t+1)}$ ) afectan el valor de $z_{t+1}$  aunque la secuencia { $y_t$ } Granger no cause la secuencia { $z_t$ }. 

Una prueba de bloques de exogeneidad es útil para detectar si se debe incorporar una variable adicional en un $VAR$. El problema consiste en determinar si los rezagos de una variable, por ejemplo, $w_t$, causan en el sentido de Granger alguna otra variable en el sistema. En el caso de tres variables con $w_t$, $y_t$ y $z_t$, la prueba es si los rezagos de $w_t$ causan en el sentido de Granger a $y_t$ o $z_t$. En esencia, el bloque de exogeneidad restringe todos los rezagos de $w_t$ en $y_t$ y $z_t$ para que sean iguales a cero. 

Esta restricción se prueba adecuadamente usando la prueba de razón de verosimilitud. Calcule las ecuaciones $y_t$ y $z_t$  usando valores rezagados de { $y_t$ } , { $z_t$ } y { $w_t$ } y calcule $\mathbf{\Sigma_u}$. Vuelva a estimar excluyendo los valores rezagados de { $w_t$ } y calcule $\mathbf{\Sigma_r}$. Encuentre el estadístico de razón de verosimilitud: $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ este estadístico tiene una distribución $\chi^2$ con grados de libertad iguales a $2p$ (ya que los $p$ valores de rezagados de { $w_t$ } se excluyen de cada ecuación). Aquí $c=3p+1$ porque las ecuaciones no restringidas de $y_t$ y $z_t$ contienen $p$ rezagos de { $y_t$ } , { $z_t$ } y { $w_t$ } más una constante.
