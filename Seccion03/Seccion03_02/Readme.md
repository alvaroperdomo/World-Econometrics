# Estimación de Modelos VAR
Si se quiere tener en cuenta todas las relaciones posibles entre, digamos, $k$ variables, parece sensato construir un modelo conjunto para todas estas series de tiempo en lugar de construir modelos para todas las series individuales. 

Considere el sistema bivariado simple:

$I$) $y_t=b_{10}-b_{12}z_t+\gamma_{11}y_{t-1}+\gamma_{12}z_{t-1}+\varepsilon_{yt}$  

$II$) $z_t=b_{20}-b_{21}y_t+\gamma_{21}y_{t-1}+\gamma_{22}z_{t-1}+\varepsilon_{zt}$   

donde se supone que 
* tanto $y_t$ como $z_t$ son estacionarios; 
* $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son perturbaciones ruido blanco con desviaciones estándar $\sigma_y$  y $\sigma_z$, respectivamente; y 
* { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco no correlacionadas.

Las ecuaciones $I$ y $II$ constituyen un vector autorregresivo de primer orden ($VAR$) porque la longitud de rezago más larga es $1$ [^1]. La estructura del sistema incorpora retroalimentación porque permite que $y_t$ y $z_t$ se afecten entre sí. Por ejemplo, $-b_{12}$  es el efecto contemporáneo de un cambio unitario de $z_t$ en $y_t$ y $\gamma_{12}$  es el efecto de un cambio de unitario de $z_{t-1}$ en $y_t$. 

[^1]: Este _VAR_ sencillo es útil para ilustrar los sistemas multivariados de orden superior que se presentaran más adelante.

Tenga en cuenta que los términos { $\varepsilon_{yt}$ y $\varepsilon_{zt}$  son innovaciones puras (o choques) en $y_t$ y $z_t$, respectivamente. Por supuesto, 
* si $b_{21}≠0$, $\varepsilon_{yt}$  tiene un efecto contemporáneo indirecto en $z_t$, y
* si $b_{12}≠0$, $\varepsilon_{zt}$  tiene un efecto contemporáneo indirecto en $y_t$. 

Las ecuaciones $I$ y $II$ no pueden ser estimadas por MCO ya que $y_t$ tiene un efecto contemporáneo en $z_t$ y $z_t$ tiene un efecto contemporáneo en $y_t$. Las estimaciones de MCO sufrirían un sesgo de simultaneidad ya que los regresores y los términos de error estarían correlacionados.

Es posible transformar el sistema de ecuaciones en una forma compacta:

$Bx_t= \Gamma_0 + \Gamma_1x_{t-1}+\varepsilon_t$ 
donde 
$$B = {\left\lbrack \matrix{1 & b_{12} \cr b_{21} & 1} \right\rbrack}$$
$$x_t = {\left\lbrack \matrix{y_t \cr z_t} \right\rbrack}$$
$$\Gamma_0 = {\left\lbrack \matrix{b_{10} \cr b_{20}} \right\rbrack}$$
$$\Gamma_1 = {\left\lbrack \matrix{\gamma_{11} & \gamma_{12} \cr \gamma_{21} & \gamma_{22}} \right\rbrack}$$
$$\varepsilon_t = {\left\lbrack \matrix{\varepsilon_{yt} \cr \varepsilon_{zt}} \right\rbrack}$$

Al premultiplicar por $B^{−1}$ se obtiene el **modelo VAR en forma estándar** $x_t= A_0 + A_1x_{t-1}+e_t$  donde $A_0=B^{−1}\Gamma_0$, $A_1=B^{−1}\Gamma_1$, y $e_t=B^{−1}\varepsilon_t$

Por propósitos de notación, podemos definir
* $a_{i0}$  como el elemento $i$ del vector $A_0$,
* $a_{ij}$ como el elemento en la fila $i$ y la columna $j$ de la matriz $A_1$, y
* $e_{it}$ como el elemento $i$ del vector $e_t$. 

Usando esta nueva notación, podemos reescribir $x_t= A_0 + A_1x_{t-1}+e_t$ en la forma equivalente:

$i$) $y_t=a_{10}+a_{11}y_{t-1}+a_{12}z_{t-1}+e_{1t}$ 

$ii$) $z_t=a_{20}+a_{21}y_{t-1}+a_{22}z_{t-1}+e_{2t}$

Para distinguir entre los sistemas representados por $I$ y $II$ versus $i$ y $ii$, el primero se llama un **VAR estructural o sistema primitivo** y el segundo se llama un **VAR en forma estándar**. 

Es importante tener en cuenta que los términos de error (es decir, $e_{1t}$ y $e_{2t}$) son compuestos de los dos choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$. Y se puede demostrar, aunque aquí no lo desarrollaremos, que dado que como $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son procesos de ruido blanco, entonces $e_{1t}$ y $e_{2t}$ son ruido blanco

Un objetivo explícito del enfoque de Box-Jenkins es proporcionar una metodología que conduzca a modelos parsimoniosos. El objetivo final de hacer pronósticos precisos a corto plazo se logra mejor eliminando las estimaciones de parámetros insignificantes del modelo. 

Sims (1980) aboga por una estrategia de estimación alternativa. 

Considere la siguiente generalización multivariada de un proceso autorregresivo $x_t= A_0 + A_1x_{t-1}+...+A_px_{t-p}+e_t$ donde 
* $x_t$ es un vector ($n×1$) que reúne las 𝑛 variables incluidas en el $VAR$
* $A_0$ es un vector ($n×1$) de interceptos
* $A_i$ son las matrices ($n×n$) de coeficientes
* $e_t$ es un vector ($n×1$) de los términos de error

La metodología de Sims implica únicamente:
1) la determinación de las variables apropiadas para incluir en el $VAR$: Las variables que se incluirán en el $VAR$ se seleccionan de acuerdo con el modelo económico relevante.
2) la determinación de la longitud de rezagos apropiada: Existen una serie de pruebas (las cuales se explicarán más adelante) para hacer esto. No se hace un intento explícito de "reducir" el número de parámetros estimados. La matriz $A_0$ contiene $n$ parámetros, y cada matriz $A_i$ contiene $n^2$ parámetros; por lo tanto, $n+pn^2$ coeficientes deben ser estimados. 

Sin lugar a dudas, el $VAR$ esta sobreparameterizado ya que muchas de las estimaciones de los coeficientes serán no significativas. Sin embargo, el objetivo es encontrar las interrelaciones importantes entre las variables. Imponer incorrectamente restricciones nulas puede desperdiciar información importante. Además, es probable que los regresores sean altamente colineales, de modo que las pruebas $t$ sobre los coeficientes individuales no son guías confiables para reducir el modelo.



 

SIMS, Christopher. _Macroeconomics and Reality_. Econometrica, Vol. 48, No. 1 (January, 1980); p.1-49.



