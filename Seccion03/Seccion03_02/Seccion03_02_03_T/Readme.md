## SECCIÓN 3.2.3:
# La Metodología de Johansen

Los estimadores de máxima verosimilitud de Johansen (1988): 
* evitan el uso de estimadores de dos pasos[^1],
* pueden estimar y probar la presencia de múltiples vectores de cointegración, y
* permiten probar versiones restringidas de los vectores de cointegración y la velocidad de los parámetros de ajuste.[^2] 

[^1]: **Tal como ocurre en la metodología de Engle y Granger (1987) visto previamente.
[^2]: **A menudo, es interesante determinar si es posible verificar una teoría probando restricciones en las magnitudes de los coeficientes estimados**

El procedimiento de Johansen (1988) se basa en gran medida en la relación entre el rango de una matriz y sus raíces características. Este no es más que una generalización multivariada de la prueba $DF$ utilizada para analizar la presencia de raíz unitaria en los modelos univariados. 

En el caso univariado, es posible ver que la estacionariedad de { $y_t$ } depende de $a_1$; es decir, dados $y_t=a_1y_{t-1}+\varepsilon_t$ o $\Delta y_t=(a_1-1)y_{t-1}+\varepsilon_t$:
* Si $(a_1-1)=0$, el proceso { $y_t$ } tiene una raíz unitaria.
* Descartando el caso en el que { $y_t$ } es explosivo, si $(a_1-1)≠0$ podemos concluir que la secuencia { $y_t$ } es estacionaria.

Las tablas de Dickey-Fuller proporcionan los estadísticos apropiados para probar formalmente la hipótesis nula $(a_1-1)=0$.

Consideremos la generalización al caso multivariado con $n$ variables; asuma que el vector $\mathbf{x_t}$ de $n$ variables, se comporta como $\mathbf{x_t=A_1x_{t-1}+\varepsilon_t}$  así que $\mathbf{\Delta x_t=A_1x_{t-1}-x_{t-1}+\varepsilon_t=(A_1-I)x_{t-1}+\varepsilon_t=\pi x_{t-1}+\varepsilon_t}$ donde 
* $\mathbf{\varepsilon_t}$ es un vector ( $n\times 1$ ),
* $\mathbf{A_1}$ es una matriz ( $n\times n$ ) de parámetros, 
* $\mathbf{I}$ es una matriz identidad ( $n\times n$ ),
* $\mathbf{\pi}$ se define como $\mathbf{(A_1-I)}$.

El rango de $\mathbf{(A_1-I}$ es igual al número de vectores de cointegración. Por analogía con el caso univariado, si $\mathbf{(A_1-I}$  tiene solo ceros, de modo que el $rango(\pi)=0$, todas las secuencias { $x_{it}$ } tienen raíz unitaria. En esta situación, dado que no hay una combinación lineal de los procesos { $x_{it}$ } que sea estacionaria, las variables no se cointegran. Descartando la presencia de raíces características mayores que $1$ y si el $rango(\pi)=n$, $\mathbf{\Delta x_t=\pi x_{t-1} + \varepsilon_t}$ es un sistema convergente de ecuaciones en diferencias, de modo que todas las variables son estacionarias.

Hay varias formas de generalizar $\mathbf{\Delta x_t=\pi x_{t-1}+\varepsilon_t}$. Por ejemplo, la ecuación se modifica fácilmente si se considera la presencia de un intercepto; simplemente asuma $\mathbf{\Delta x_t=A_0 \pi x_{t-1}+\varepsilon_t}$ donde $$ \mathbf{A_0}={\left\lbrack \matrix{ a_{10} \cr a_{20} \cr \dots \cr a_{n0} } \right\rbrack}. El efecto de incluir los diversos $a_{i0}$ es permitir la posibilidad de una tendencia lineal en el proceso de generación de datos. Lo ideal es incluir el intercepto si las variables exhiben una tendencia decidida a aumentar o a disminuir. Aquí, el rango de $\pi$ se puede ver como el número de relaciones de cointegración existentes en los datos sin tendencia. En el largo plazo, $\mathbf{\pi x_{t-1}=0}$ tal que el valor esperado de cada secuencia { $\Delta x_{it}$ } es $a_{i0}$. Al agregar todos estos cambios en el tiempo se obtiene la expresión determinista $a_{i0}t$.

Las figuras de abajo ilustran los efectos de incluir un intercepto en el proceso de generación de datos. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/61152b61-d2a2-4dab-8f67-133f11dde731)

En las figuras, se generaron dos secuencias aleatorias, { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ }, con $100$ observaciones cada una. Por otro lado, se asumió $y_0=z_0=0$, y que los siguientes $100$ valores de las secuencias { $y_t$ } y { $z_t$ } eran

de modo que la relación de cointegración es $y_t=z_t$

* En la figura de la izquierda, puede ver que cada secuencia se asemeja a un proceso aleatorio y que ninguno se aleja demasiado del otro.
* En la figura del centro se agregan interceptor de manera que $a_{10}=a_{20}=0.1$; ahora cada serie tiende a aumentar $0.1$ unidades en cada período. Además del hecho de que cada secuencia comparte la misma tendencia estocástica, tenga en cuenta que cada una también tiene la misma tendencia de tiempo determinista. El hecho de que cada uno tenga la misma tendencia determinista no es el resultado de la equivalencia entre $a_{10}$ y $a_{20}$; ya que $y_t$ y $z_t$ están cointegrados, la solución general a $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ requiere que cada uno tenga la misma tendencia lineal.
* Para verificar esto, la figura de la derecha establece $a_{10}=0.1$ y $a_{20}=0.4$. De nuevo, las secuencias tienen las mismas tendencias estocásticas y deterministas.

A continuación veremos que manipulando apropiadamente los elementos de $A_0$ es posible incluir una constante en los vectores de cointegración sin necesidad de in-cluir una tendencia de tiempo determinista al sistema.

Una forma de incluir una constante en las relaciones de cointegración es restringir los valores de los diversos $a_i0$. Por ejemplo, si $rango(\pi)=1$, las filas de $\pi$ pueden diferir sólo en un escalar, por lo que es posible escribir cada secuencia { $\Delta x_{it}$ } en $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)}) + a_{10} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)}) + a_{20} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)}) + a_{n0} + \varepsilon_{nt}$

donde $s_i$ es un escalar tal que $s_i \pi_{1j}= \pi_{ij}$  

Si $a_{i0}$ se puede restringir tal que $a_i0=s_ia_{10}$, todas las secuencias { $\Delta x_{it}$ } se pueden escribir con la constante incluida en el vector de cointegración:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + a_{10}) + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + a_{20}) + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + a_{20}) + \varepsilon_{nt}$

o en la forma compacta $\mathbf{\Delta x_t= A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ donde 

$\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$, 
$\eqalign{\mathbf{x_{t-1}^*} = {\left\lbrack \matrix{x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack}}$,     y 

$\eqalign{\mathbf{ \pi* } = {\left\lbrack \matrix{\pi_{11} & \pi_{12} & \dots & \pi_{1n} \cr \pi_{21} & \pi_{22} & \dots & \pi_{2n} \cr \dots & \dots & \dots & \dots \cr \pi_{n1} & \pi_{n2} & \dots & \pi_{nn} } \right\rbrack}}$ 

La característica interesante de $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ es que la tendencia lineal se elimina del sistema. En esencia, los diversos $a_io$ se han modificado de tal manera que la solución general para cada { $x_it$ } no contiene una tendencia temporal. La solución al conjunto de ecuaciones en diferencias representadas $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ por es tal que se espera que todos los $Delta x_{it}$ sean iguales a cero cuando $\pi_{11} x_{1(t-1)} + \pi_{12} x_{2(t-1)} + \dots + \pi_{1n} x_{n(t-1)} + a_{i0} =0$.

Para resaltar la diferencia entre $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ y $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + varepsilon_t}$, la figura de abajo ilustra las consecuencias de utilizar $a_10=0.1$ y $a_20=-0.1$.  

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/217d2e36-944c-493c-8cab-b45d1ff27ff6)

Se puede ver que ninguna secuencia contiene una tendencia determinista. De hecho, para los datos que se muestran en la figura, la tendencia desaparecerá siempre que seleccionemos valores de los interceptos manteniendo la relación $a_10=-a_20$.

**Algunos econometristas prefieren incluir un intercepto en el vector de cointegración junto con un intercepto fuera de éste.** Esto tiene sentido si las variables contienen un intercepto y si la teoría económica sugiere que el vector de cointegración contiene un intercepto. Sin embargo, debe quedar claro que el intercepto en el vector de cointegración no se identifica en presencia de un intercepto fuera de este. Después de todo, alguna parte del intercepto no restringido siempre puede incluirse en el vector de cointegración. 

En términos del ejemplo anterior, el sistema siempre puede escribirse como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + b_{10}) + b_{11} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + b_{20}) + b_{21} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + b_{n0}) + b_{n1} + \varepsilon_{nt}$

donde  $s_i b_{10} + b_{11}= a_{10}$  

Todo lo que se hace es dividir $a_{10}$ en dos partes y colocar una parte dentro de la relación de cointegración. Es necesaria alguna estrategia de identificación ya que la proporción del intercepto a incluir en el vector de cointegración es arbitraria. 

De los gráficos previos, note que es necesario una constante fuera de la relación de cointegración para capturar los efectos de una tendencia sostenida de las variables a aumentar (o a disminuir). La mayoría de los investigadores incluyen interceptos si los datos se asemejan a los gráficos donde ambos interceptos eran 0.1 o donde los interceptos eram 0.1 y 0.4. De lo contrario, incluyen interceptos en el vector de cointegración o excluyen por completo a los regresores deterministas. Si no está seguro, puede usar los métodos que se describen más adelante para probar si las desviaciones se pueden restringir adecuadamente. R permite incluir una tendencia de tiempo determinista en el modelo. Claro que es mejor evitar el uso de la tendencia como variable explicativa a menos que tenga una buena razón para incluirla en el modelo. Johansen (1994) discute el papel de los regresores deterministas en una relación de cointegración.

Al igual que con la prueba $ADF$, el modelo multivariado también puede generalizarse para permitir un proceso autorregresivo de orden superior. 

Considere $x_t = A_1x_{t-1} + A_2x_{t-2}  + \dots +  A_px_{t-p} + \varepsilon_t$ donde $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$ y $\mathbf{\varepsilon_t}$ es un vector n-dimensional distribuido de forma independiente e idéntica con media cero y matriz de varianza $\Sigma_\varepsilon$.

Como se hizo con la prueba $ADF$ en el modelo univariado, con las sumas apropiadas, la ecuación $\mathbf{x_t= \displaystyle\sum_{i=1}^{p} A_i x_{t-i} + \varepsilon_t}$ se puede transformar en: $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ donde $\mathbf{\pi = -(I-\displaystyle\sum_{i=1}^p A_i)}$ y $\mathbf{\pi_i = -(I-\displaystyle\sum_{j=i+1}^p A_j)}$ 

La característica clave a tener en esta nueva ecuación es el rango de la matriz $\mathbf{\pi}$ ; **el rango de $\mathbf{\pi}$ es igual al número de vectores cointegrantes independientes**:

* si $\mathbf{rango(\pi)=0}$, la matriz es nula y $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ es el modelo $VAR$ en las primeras diferencias.
* si $\mathbf{rango(\pi)=n}$, el proceso vectorial es estacionario.
* si $\mathbf{rango(\pi)=1}$, hay un solo vector de cointegración y la expresión $\mathbf{\pi x_{t-1}}$ es el término de corrección de errores.
* si $\mathbf{1 \lt rango(\pi) \lt n}$, hay múltiples vectores de cointegración.

El número de distintos vectores de cointegración se puede obtener al verificar la significancia de las raíces características de $\mathbf{\pi}$. 
Sabemos que el rango de una matriz es igual al número de sus raíces características que difieren de cero. Suponga que se obtiene la matriz $\mathbf{\pi}$ y se ordenan las $n$ raíces características de forma que $\lambda_1>\lambda_2>\dots>\lambda_n$. 

Si las variables en $\mathbf{x_t}$ no están cointegradas, $\mathbf{rango(\pi)=0}$ y todas las raíces características serán iguales a cero. Como $\ln{(1)}=0$, entonces cada una de las expresiones $\ln{(1 - \lambda_i)}$ será igual a cero si las variables no están cointegradas. 

De manera similar, si $\mathbf{rango(\pi)=1}$, $0 < 𝜆_1 < 1$, entonces $\ln{(1 - \lambda_1)} < 0$ y $\ln{(1 - \lambda_2)} = \ln{(1 - \lambda_3)} = \dots = \ln{(1 - \lambda_n)} = 0$.

En la práctica, solo podemos obtener estimaciones de $\mathbf{\pi}$ y de sus raíces características. La prueba para el número de raíces características que son significativamente diferentes de $1$ se puede realizar utilizando los siguientes dos estadísticos de prueba:

* $\lambda_{traza}(r)=-T\displaystyle\sum_{r+1}^n \ln{(1-\hat{\lambda_i})}$ 
* $\lambda_{max}(r,r+1)=-T\ln{(1-\hat{\lambda_{r+1}})}$

donde
* $\hat{\lambda_i}$ son los valores estimados de las raíces características (también llamados valores propios) obtenido de la matriz $\mathbf{\pi}$ estimada 
* $T$ es el número de observaciones utilizables

El estadístico $\lambda_{traza}$ prueba la hipótesis nula de que el número de diferentes vectores de cointegración es menor o igual que $r$ frente a una alterna-tiva general. Note que $\lambda_{traza}=0$ cuando todos los $\lambda_i=0$. Cuanto más lejos están las raíces características estimadas de cero, más negativo es $(1-\hat{\lambda_i})$  y más grande es el estadístico $\lambda_{traza}$. 

El estadístico $\lambda_{max}$ prueba la hipótesis nula de que el número de vectores de cointegración es $r$ frente a la alternativa de $r+1$ vectores de cointe-gración. Si el valor estimado de la raíz característica está cerca de cero, $\lambda_{max}$ será pequeño.

Los valores críticos de los estadísticos $\lambda_{traza}$  y $\lambda_{max}$ se obtienen utilizan-do el enfoque de Monte Carlo. La distribución de estos estadísticos depende de dos cosas:
1) El número de componentes no estacionarios bajo la hipótesis nula (es decir, $n-r$).
2) La forma del vector $A_0$. Es decir: 
    * si no incluye interceptos ni en la ecuación ni en el vector de cointegración.
    * si incluye un intercepto en la ecuación.
    * si incluye un intercepto en el vector de cointegración.

Es importante anotar que estos estadísticos necesitan que sus residuos sean ruido blanco. Cualquier evidencia de que los errores no son ruido blanco generalmente significa que las longitudes de rezago son demasiado cortas. 

En la prueba de Johansen, es importante determinar correctamente la forma de los regresores deterministas. Por ejemplo, los valores críticos de los estadísticos $\lambda_{traza}$ y $\lambda_{max}$ son más pequeños sin ningún tipo de regresores deterministas y más grandes con un intercepto en el vector de cointegración. 
En lugar de plantear con cautela la forma de $A_0$, es posible probar formas restringidas del vector [^*]. La idea clave de todas las pruebas de hipótesis es que **si hay $r$ vectores de cointegración, solo estas $r$ combinaciones lineales de las variables son estacionarias**. Todas las demás combinaciones lineales son no estacionarias. Por lo tanto, suponga que se reestima el modelo restringiendo los parámetros de $\pi$. Si las restricciones no son vinculantes, debe encontrar que el número de vectores de cointegración no ha disminuido. 

[^*]: Uno de los aspectos más interesantes del procedimiento de Johansen es que permite probar formas restringidas de los vectores de cointegración.

Para probar la presencia de un intercepto en el vector de cointegración en oposición al intercepto no restringido $A_0$, estime las dos formas del modelo. Denote 

* las raíces características ordenadas de la matriz no restringida $\pi$ por $\hat{\lambda_i},\dots,\hat{\lambda_n}$, y
* las raíces características del modelo con los interceptos en los vectores de cointegración por $\hat{\lambda_i^* },\dots,\hat{\lambda_n^* }$. 

Suponga que la forma no restringida del modelo tiene $r$ raíces características distintas de cero.  Asintóticamente, el estadístico 
$-T\displaystyle\sum_{i=r+1}^n[\ln{(1-\hat{\lambda_i^* )}}-\ln{(1-\hat{\lambda_n})}]$ tiene una distribución $\chi^2$ con $(n-r)$ grados de libertad. La intuición detrás de la prueba es que todos los valores de $\ln{1-\hat{\lambda_i^*}}$ y \ln{1-\hat{\lambda_n}} deben ser equivalentes si la restricción no es vinculante. Por lo tanto, valores pequeños del estadístico de prueba implican que está permitido incluir el intercepto en el vector de cointegración. Sin embargo, la probabilidad de encontrar una combinación lineal estacionaria de las 𝑛 variables es mayor con el intercepto en el vector de cointegración que si el intercepto está ausente de este. 

Por lo tanto, un valor grande de $\hat{\lambda_{r+1}^* }$ [y en consecuencia de $-T \ln{(1-\hat{\lambda_i^* )}}$], implica que la restricción infla artificialmente el número de vectores de cointegración. En-tonces, como lo demuestra Johansen (1991), si el estadístico de prueba es suficientemente grande, es posible rechazar la hipótesis nula de un intercepto en el vector de cointegración y concluir que hay una tendencia lineal en las variables. 

La manera más sencilla de comprender las pruebas de longitud de rezagos es considerar el sistema en la forma $\mathbf{\Delta x_t=\pi^* x_{t-1} + \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$. Independientemente del rango de $\pi$, todos los $\Delta x_{t-i}$ son variables estacionarias. Entonces, a partir de Sims, Stock y Watson (1990), sabemos que los coeficientes de interés en las variables estacionarias de media cero se pueden probar utilizando una distribución normal. Dado que la longitud del rezago depende únicamente de los valores de los diversos $\pi_i$, una distribución $\chi^2$ es apropiada para probar cualquier restricción relacionada con la longitud del rezago. Como en el caso de cualquier $Var$, sea $Sigma_u$ y $Sigma_r$ las matrices de varianzas y covarianzas de los sistemas no restringidos y restringidos, respectivamente. Suponga que $c$ denota el número máximo de regresores contenidos en la ecuación más larga. El estadístico de prueba $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ se puede comparar con una distribución $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. 

Alternativamente, puede usar el Coeficiente de Información de Akaike o el Coeficiente Bayesiano de Schwartz multivariado para determinar la longitud del rezago. Si desea probar la longitud del rezago para una sola ecuación, una prueba $F$ es apropiada. Sims, Stock y Watson (1990) también establecen que no se pueden realizar pruebas de causalidad de Granger en un sistema cointegrado usando una prueba $F$ estándar. Cuando el $rango(\pi)=0$ entonces $\mathbf{\Delta x_t= \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$. En este caso (es decir, en un $VAR$ en diferencias), sólo hay variables estacionarias. Por lo tanto, las pruebas de causalidad de Granger se pueden realizar utilizando una distribución $F$ estándar. Sin embargo, si las variables están cointegradas, $\mathbf{\Delta x_t=\pi^* x_{t-1} + \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$, una prueba de causalidad de Granger involucra los coeficientes de $\pi$. Dado que estos coeficientes multiplican las variables no estacionarias, no es apropiado usar un estadístico $F$ para probar la causalidad de Granger. Después de todo, si el $rango(\pi)=0$, es imposible escribir las restricciones de la prueba como restricciones en un conjunto de variables $I(0)$. 

Sims, Stock y Watson (1990), también descartan las pruebas de exogenei-dad en bloque. 

## Resumen de la metodología propuesta por Johansen

Utilice los siguientes cuatro pasos cuando implemente el procedimiento Johansen:

1) **Realice una prueba de todas las variables para evaluar su orden de integración.**

   Grafique los datos para ver si es probable que una tendencia de tiempo lineal esté presente en el proceso de generación de datos. Los resultados de la prueba pueden ser bastante sensibles a la longitud del rezago, por lo que es importante tener cuidado. El procedimiento más común es estimar $VAR$ de los datos no diferenciados. Luego use las mismas pruebas de longitud de rezagos que en un $VAR$ tradicional. Comience con el rezago mas largo que considere razonable y verifique si se puede acortar. Por ejemplo, si queremos probar si los rezagos $1$ a $4$ son importantes, podemos estimar los siguientes dos 𝑉𝐴𝑅:
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+A_2 x_{t-2}+A_3 x_{t-3}+A_4 x_{t-4}+e_{1t}}$
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+e_{2t}}$

donde 
   * $\mathbf{x_t}$ es un vector de variables ( $n\times 1$ ),
   * $\mathbf{A_0}$ es la matriz ( $n\times 1$ ) de interceptos,
   * $\mathbf{A_1}$ son matrices ( $n\times n$  de coeficientes, y
   * $\mathbf{e_{1t}}$ y $\mathbf{e_{2t}}$ son vectores ( $n\times 1$ ) de términos de error.
   
   Calcule el primer sistema con cuatro rezagos de cada variable en cada ecuación y llame a la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_4}$. Ahora estime la segunda ecuación usando solo un rezago de cada variable en cada ecuación y llame a la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_1}$. 

Aunque trabajamos con variables no estacionarias, podemos realizar pruebas de longitud de rezagos utilizando el estadístico de prueba de razón de verosi-militud recomendado por Sims (1980): $(T-c)(\ln{|\mathbf{\Sigma_1}|}-\ln{|\mathbf{\Sigma_4}|})$ donde 
* $T$ es el número de observaciones,
* $c" es el número de parámetros en el sistema no restringido y
* $ln{|\mathbf{\Sigma_i}|}$ es el logaritmo natural del determinante de $\mathbf{\Sigma_i}$.

Siguiendo a Sims, use la distribución $\chi^2$ con grados de libertad igual al número de restricciones de los coeficientes. Como cada $\mathbf{A_i}$ tiene $n^2$ coeficientes, la restricción $\mathbf{A_2=A_3=A_4=0}$  implica restricciones $3n^2$. 

Alternativamente, puede seleccionar la longitud del rezago $p$ utilizando las generalizaciones multivariadas del Criterio de Información de Akaike o del Criterio Bayesiano de Schwartz. En el modelo en cuestión, puede darse por ejemplo que el método general a específico y el Criterio de Información de Akaike seleccionan una longitud de rezago de $2$, mientras que el Criterio Bayesiano de Schwartz selecciona una longitud de rezago de $1$. 

2) **Estime el modelo y determine el rango de $\pi$.**

   Muchos paquetes de software econométrico contienen una rutina para estimar el modelo. Aquí, basta con decir que Mínimos Cuadrados Ordinarios no es apropiado porque es necesario imponer restricciones de ecuaciones cruzadas en la matriz. En la mayoría de los casos, puede elegir estimar el modelo en tres formas:
   
   i) con todos los elementos de $\mathbf{A_0}$ establecidos en cero,
   
   ii) con una constante en la ecuación, o
   
   iii) con un término constante en el vector de cointegración. 

3) **Analice el(los) vector(es) de cointegración normalizados y la velocidad de los coeficientes de ajuste.**

   Por ejemplo, asuma que si seleccionamos $r=1$, el vector estimado de cointegración $(\beta_0,\beta_1,\beta_2,\beta_3)$ es $\mathbf{\beta_t}=(0.00553, 0.41532, 0.42988, −0.42207)$. Normalizando con respecto a $\beta_1$, el vector de cointegración normalizado es $\mathbf{\beta_t}=(−0.01331, −1.0000, −1.0350, 1.0162)$.

   Asuma que los valores teóricos del vector de cointegración son $(0,−1,−1, 1)$. En consecuencia, considere hacer las siguientes pruebas:
   a) La prueba de $\beta_0=0$ implica una restricción en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con un grado de libertad.
   b) Para restringir el vector de cointegración normalizado tal que $\beta_2=-1$ y $\beta_3=-1$ implica dos restricciones en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con dos grados de libertad.
   c) Para probar la restricción conjunta $\mathbf{\beta_t}=(0,−1,−1, 1)$ implica las tres restricciones $\beta_0=0$, $\beta_2=-1$ y $\beta_3=-1$.

4) Finalmente, **las pruebas de impulso-respuesta, descomposición de varianza y causalidad en el modelo de corrección de errores** podrían ayudar a identificar un modelo estructural y determinar si el modelo estimado parece ser razonable.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
