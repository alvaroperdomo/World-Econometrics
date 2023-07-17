## SECCIÓN 2.2.4. (T)
# El cambio estructural

Cuando hay cambios estructurales, los diversos estadísticos de las diferentes pruebas de raíz unitaria vistas previamente están sesgados hacia el no rechazo de la existencia de una raíz unitaria. Por lo tanto, al realizar pruebas de raíz unitaria, se debe tener especial cuidado si se sospecha que ha ocurrido un cambio estructural

## El cambio estructural en series estacionarias 

Como ejemplo, considere la situación en la que hay un cambio único en la media de una secuencia estacionaria. En el gráfico  de abajo la secuencia { $y_t$ } se construyó de manera que es estacionaria alrededor de una media de 0 para $t=0, …, 50$ y luego es estacionaria alrededor de una media de 6 para $t=51, …, 100$. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5e70a54a-0564-4c7a-bde3-9812c475657d)

La secuencia se formó:
* Simulando 100 valores distribuidos normalmente e independientemente para la secuencia { $\varepsilon_t$ }.  
* Asumiendo $y_0=0$. 
* Los siguientes 100 valores en la secuencia fueron generados usando la fórmula: $y_t=0.5y_{t-1}+ \varepsilon_t + D_L$ donde 

$$
D_L=\left\[\
\begin{array}{ll}
0 & \text{para  } t=1,...,50 \\
3 & \text{para  } t=51,...,100
\end{array}
\right.
$$

El subíndice $L$ indica que el nivel (_Level_ en inglés) de la variable dummy $D_L$ cambia[^1]. 
[^1]: **Una variable dummy es una variable que toma únicamente dos valores. Generalmente estos valores son 0 y 1, pero no son la única opción**

En la práctica, el cambio estructural puede no ser tan evidente como se muestra en la figura de arriba. Sin embargo, la variable dummy $D_L$ es útil para ilustrar el problema de usar una prueba de Dickey-Fuller: 
* La línea recta del gráfico es la mejor estimación lineal de Mínimos Cuadrados Ordinarios ($y_t=a_0+a_2t+e_t$, donde $a_0<0$ y $a_2>0$) plantea erróneamente que la serie tiene una tendencia determinista. 
* La forma correcta de hacer la estimación es un modelo $AR(1)$ en el que se permita  que la intersección cambie al incluir la variable dummy $D_L$ tal que permita obtener $y_t=0.5y_{t-1}+\varepsilon_t+D_L$. 
* Sin embargo, suponga que se estima la ecuación: $y_t=a_0+a_1y_{t-1}+e_t$. Como se puede inferir de la figura, el valor estimado de $a_1$ está necesariamente sesgado hacia 1. 

La razón de este sesgo ascendente es que:
* el valor estimado de $a_1$ captura la propiedad de que los valores "bajos" de $y_t$ (es decir, aquellos que fluctúan alrededor de 0) están seguidos por otros valores "bajos" y 
* los valores "altos" (es decir, aquellos que fluctúan alrededor de una media de 6) son seguidos por otros valores "altos". 

Para una demostración formal, tenga en cuenta que a medida que $a_1$ se acerca a 1, $y_t=a_0+a_1y_{t-1}+e_t$ se acerca a un paseo aleatorio con intercepto. Recuerde que, como vimos en el Anexo 1 de la sección 2.1, la solución del paseo aleatorio con intercepto incluye una tendencia determinista; es decir,  $y_t=y_0+a_0t+\displaystyle\sum_{i=1}^t \varepsilon_i$. Por lo tanto, la ecuación mal especificada $y_t=a_0+a_1y_{t-1}+e_t$ tenderá a imitar la línea de tendencia mostrada en la figura de arriba sesgando $a_1$ hacia 1. Este sesgo en $a_1$ significa que la prueba $ADF$ está sesgada hacia el no rechazo de la hipótesis nula de una raíz unitaria, aunque la serie es estacionaria dentro de cada uno de los subperíodos.

## El cambio estructural en series no estacionarias 

Por supuesto, un proceso de raíz unitaria también puede exhibir un cambio estructural. La figura de abajo simula un paseo aleatorio con un cambio estructural que se produce en $t=51$. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/452c868c-1da9-447c-be1d-bb36eb69e366)

Esta segunda simulación utilizó:
* Los mismos 100 valores para la secuencia { $\varepsilon_t$ } y 
* la condición inicial $y_0=2$. 
* Las 100 realizaciones siguientes de la secuencia $y_t$ se construyeron como $y_t=y_{t-1}+\varepsilon_t+D_P$ donde

$$
D_P=\left\[\
\begin{array}{ll}
1 & \text{para  } t=51 \\
0 & \text{en los otros casos } 
\end{array}
\right.
$$

El subíndice $P$ indica que $D_P$ es una variable dummy de pulso. 

En un proceso de raíz unitaria, un único pulso en la dummy tendrá un efecto permanente en el nivel de la secuencia { $y_t$ }. En $t=51$, el pulso en la dummy es equivalente a un choque $\varepsilon_{t+51}$ de cuatro unidades adicionales. Por lo tanto, el choque de una sola vez a $D_P(51)$ tiene un efecto permanente en el valor medio de la secuencia para $t \ge 51$. 

El sesgo en las pruebas tradicionales de raíz unitaria, y en particular en la prueba $ADF$ se confirmó en un experimento de Monte Carlo realizado por Perron (1989). Perron generó 10.000 repeticiones de un proceso de la misma naturaleza que $y_t=0.5y_{t-1}+\varepsilon_t+D_L$. Cada replica la formó:
* generando 100 valores distribuidos normalmente e independientemente para la secuencia { $\varepsilon_t$ }. 
* Para cada una de las 10.000 series replicadas, utilizó Mínimos Cuadrados Ordinarios para estimar una regresión en la forma de $y_t=a_0+a_1y_{t-1}+e_t$. 

Perron descubrió que los valores estimados de $a_1$ estaban sesgados hacia $1$. Además, el sesgo se hizo más pronunciado a medida que aumentaba la magnitud del cambio.

## Prueba de Perron
Volviendo a las dos gráficas de arriba, puede haber casos en los que a plena vista sin ayuda no se puede detectar fácilmente la diferencia entre los tipos alternativos de secuencias. 

Un procedimiento econométrico para probar las raíces unitarias en presencia de un cambio estructural implica dividir la muestra en dos partes y usar las pruebas $ADF$ en cada una de las partes. El problema con este procedimiento es que los grados de libertad para cada una de las regresiones resultantes disminuyen. 

Perron (1989) desarrolla un procedimiento formal para probar la presencia de raíz unitaria cuando hay un cambio estructural en el período de tiempo $t=\tau+1$. 

Considere la hipótesis nula de un salto de una sola vez en el nivel de un proceso de raíz unitaria frente a la hipótesis alternativa de un cambio de una sola vez en el intercepto de un proceso estacionario en tendencia. Formalmente, dejemos que las hipótesis nula y alternativa sean:
* $H_0: y_t= a_0 + y_{t-1}+ \mu_1 D_P + \varepsilon_t$ donde 

$$
D_P=\left\[\
\begin{array}{ccc}
1 & \text{si  } t= \tau+1 \\
0 & \text{si  } t \ne \tau+1 \\
\end{array}
\right.
$$

* $H_1: y_t= a_0 + a_2t+ \mu_2 D_L + \varepsilon_t$ donde 

$$
D_L=\left\[\
\begin{array}{ccc}
1 & \text{si  } t \gt \tau \\
0 & \text{si  } t \le \tau \\
\end{array}
\right.
$$

Bajo la hipótesis nula, { $y_t$ } es un proceso de raíz unitaria con un salto de una sola vez en el nivel de la secuencia en el período $t= \tau + 1$. Bajo la hipótesis alternativa, { $y_t$ } es una tendencia estacionaria con un salto de una sola vez en el intercepto. 

La figura de abajo puede ayudar a visualizar las dos hipótesis:
* Simulando $y_t=0.2+y_{t-1}+\mu_1 D_P(51) + \varepsilon_t$ y usando 100 realizaciones para la secuencia { $\varepsilon_t$ }, la línea discontinua errática representa la trayectoria en el tiempo de { $y_t$ } bajo **$H_0$**. Antes y después del salto en el periodo 51, la secuencia { $y_t$ } sigue el mismo paseo aleatorio con intercepto. 
* **$H_1$**  postula que la secuencia { $y_t$ }  es estacionaria alrededor de la línea de tendencia discontinua. Hasta $t=\tau$, { $y_t$ } es estacionaria alrededor de $a_0+a_2t$, y comenzando en $\tau+1$, $y_t$ es estacionaria alrededor de $a_0+a_2t+\mu_2$. Como lo ilustra la línea continua, hay un aumento de una sola vez en el intercepto de la tendencia si $\mu_2 \gt 0$.

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/3b118e20-4f18-4cfb-907b-6ee107a30d44)

El problema econométrico es determinar si una serie observada se modela mejor con $H_0$ o con $H_1$. La implementación de la técnica de Perron (1989) se divide en cuatro sencillos pasos:

**PASO 1:** No hay una forma directa de restringir los coeficientes de $H_1$  para obtener $H_0$. Como tal, necesitamos combinar ambas hipótesis de la siguiente manera: $y_t=a_0+a_1y_{t-1}+a_2t+\mu_1 D_P+\mu_2 D_L+ \varepsilon_t$

**PASO 2:** Estime la ecuación formada en el Paso 1. Bajo la hipótesis nula de una raíz unitaria, $a_1=1$. Perron (1989) muestra que, cuando los residuos están idéntica e independientemente distribuidos, la distribución de $a_1$ depende de la proporción $\lambda$ de observaciones que se producen antes del cambio.
#### Sea esta proporción $\lambda = \displaystyle\frac{\tau}{T} $

**PASO 3:** Realice las verificaciones de diagnóstico para determinar si los residuos del Paso 2 están serialmente correlacionados. Si hay correlación serial, use la forma aumentada de la regresión $y_t=a_0+a_1y_{t-1}+a_2t+\mu_1 D_P+\mu_2 D_L+ \displaystyle\sum_{i = 1}^{p} \beta_i \Delta y_{t-i} + \varepsilon_t$

**PASO 4:** Calcule el estadístico $t$ para la hipótesis nula $a_1=1$. Este estadístico puede compararse con los valores críticos calculados por Perron. 

Perron generó 5000 series de acuerdo con $H_1$ usando valores de $\lambda$ que van de $0$ a $1$ en incrementos de $0.1$. Para cada valor de $\lambda$, estimó cada una de las regresiones y calculó la distribución muestral de $a_1$. Los valores críticos son idénticos a los estadísticos de Dickey-Fuller cuando $\lambda=0$ y $\lambda=1$; en efecto, no hay cambio estructural a menos que $0 \lt \lambda \lt 1$. 

La diferencia máxima entre los dos estadísticos se produce cuando $\lambda=0.5$. Para $\lambda=0.5$, el valor crítico del estadístico $t$ en el nivel de significancia del 5% es $−3.76$ (que es mayor en valor absoluto que el estadístico Dickey-Fuller correspondiente de $−3.41$). 

Si encuentra un estadístico $t$ mayor que el valor crítico calculado por Perron, es posible rechazar la hipótesis nula de una raíz unitaria. La metodología es bastante general, ya que también puede permitir un cambio único en el intercepto o un cambio único tanto en la tendencia como en el intercepto. 

Por ejemplo, es posible probar la hipótesis nula de un cambio permanente en el intercepto frente a la alternativa de un cambio en la pendiente de la tendencia.  

$H_0: y_t= a_0 + y_{t-1} + \mu_2 D_L + \varepsilon_t$ donde 

$$
D_L=\left\[\
\begin{array}{ccc}
1 & \text{para  } t \gt tau \\
0 & \text{para  } t \le tau \\
\end{array}
\right.
$$

$H_1: y_t= a_0 + a_2t + \mu_3 D_T + \varepsilon_t$ donde

$$
D_T=\left\[\
\begin{array}{ccc}
t-\tau & \text{para  } t \gt \tau \\
0 & \text{para  } t \le \tau \\
\end{array}
\right.
$$

En **$H_0$**, la secuencia { $y_t$ } se genera por $\Delta y_t = a_0 + \varepsilon_t$ hasta el período $\tau$ y por  $\Delta y_t = a_0 + \mu_2 + \varepsilon_t$ a partir de entonces:
* Si $\mu_2 \gt 0$, la magnitud del intercepto aumenta para $t \gt \tau$.
* De manera similar, se produce una reducción en el intercepto si $\mu_2 \lt 0$.

En **$H_1$** se postula una serie estacionaria en tendencia con un cambio uniforme en la pendiente de la tendencia a partir de $t \gt tau$:
* positivo si $\mu_3 \gt 0$ y 
* negativo si $\mu_3 \lt 0$ 

Para ser aún más general, es posible combinar las dos hipótesis previamente analizadas:

$H_0: y_t= a_0 + y_{t-1} + \mu_1 D_P + \mu_2 D_L + \varepsilon_t$

$H_1: y_t= a_0 + a_2t + \mu_2 D_L + \mu_3 D_T + \varepsilon_t$

Nuevamente, el procedimiento implica combinar las hipótesis nula y alternativa en una sola ecuación. Considere $H_0: y_t= a_0 + a_1 y_{t-1} + \mu_1 D_P + \mu_2 D_L + \mu_3 D_T + \varepsilon_t$. Compare el estadístico $t$ de la estimación de $a_1$ con el valor crítico calculado por Perron (1989). Si los errores de esta regresión no parecen ser ruido blanco, estime la ecuación en la forma de una prueba aumentada de Dickey-Fuller. El estadístico $t$ para la hipótesis nula $a_1=1$ puede compararse con los valores críticos calculados por Perron (1989)

## Prueba de Zivot y Andrews
Se debe tener cuidado al usar el procedimiento de Perron, ya que supone que la fecha del cambio estructural es conocida. Zivot y Andrews (1992) muestran que si el momento de cambio estructural es erróneo y se aplica la prueba de Perron, entonces aumenta la probabilidad de que se rechace la presencia de raíz unitaria en momentos en que esta realmente existe. Por lo tanto, Zivot y Andrews (1992) proponen una prueba de raíz unitaria que calcula endógenamente el momento del cambio estructural. Ante ello, plantean el siguiente procedimiento:

**Paso 1:** Calcule el estadístico $t$ para verificar la hipótesis nula $a_1=0$  (es decir, la hipótesis nula de raíz unitaria) para todos los momentos ($\tau$) y para todos los posibles tipos de cambio estructural. Para el caso de cambio estructural en tendencia y pendiente. $\Delta y_t= a_0 + a_1 y_{t-1} + a_2t + \mu_2 D_L + \mu_3 D_T + \displaystyle\sum_{i = 1}^{p} \beta_i \Delta y_{t-i} +  \varepsilon_t$

**Paso 2:** Escoja el estadístico $|t|$ más alto (es decir, el menos favorable a aceptar $H_0$) y compárelo con los valores críticos propuestos por Zivot y Andrews.

**Paso 3:** Si se rechaza $H_0$, se concluye que no existe raíz unitaria. En caso contrario, se debe identificar el mejor modelo de cambio estructural utilizando la prueba iterativa de Chow.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
