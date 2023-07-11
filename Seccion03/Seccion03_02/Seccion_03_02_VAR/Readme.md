# ¿Qué es un modelo VAR?

Considere el sistema bivariado simple[^1]:

[^1]: **Por razones pedagógicas nos centraremos principalemente en el análisis de este sistema _VAR_ dos variables. Este _VAR_ sencillo es útil para ilustrar los sistemas multivariados de orden superior que se presentaran más adelante**

$I$) $y_t=b_{10}-b_{12}z_t+\gamma_{11}y_{t-1}+\gamma_{12}z_{t-1}+\varepsilon_{yt}$  

$II$) $z_t=b_{20}-b_{21}y_t+\gamma_{21}y_{t-1}+\gamma_{22}z_{t-1}+\varepsilon_{zt}$   

donde se supone que 
* tanto { $y_t$ } como { $z_t$ } son series estacionarias; 
* { $\varepsilon_{yt}$ }  y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco con desviaciones estándar $\sigma_y$  y $\sigma_z$, respectivamente; y 
* { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ } son perturbaciones ruido blanco no correlacionadas.

Las ecuaciones $I$ y $II$ constituyen un vector autorregresivo de primer orden $VAR(1)$ porque la longitud de rezago más larga dentro del sistema es $1$. La estructura del sistema incorpora retroalimentación porque permite que $y_t$ y $z_t$ se afecten entre sí. Por ejemplo, $-b_{12}$  es el efecto contemporáneo de un cambio unitario de $z_t$ en $y_t$ y $\gamma_{12}$ es el efecto de un cambio de unitario de $z_{t-1}$ en $y_t$. 

Dado que los términos $\varepsilon_{yt}$ y $\varepsilon_{zt}$ son innovaciones puras (o choques) en $y_t$ y $z_t$, respectivamente. Entonces, 
* si $b_{21}≠0$, $\varepsilon_{yt}$  tiene un efecto contemporáneo indirecto en $z_t$, y
* si $b_{12}≠0$, $\varepsilon_{zt}$  tiene un efecto contemporáneo indirecto en $y_t$. 

Las ecuaciones $I$ y $II$ no pueden ser estimadas por Mínimos Cuadrados Ordinarios ( $MCO$ ) ya que 
* $y_t$ tiene un efecto contemporáneo en $z_t$ y
* $z_t$ tiene un efecto contemporáneo en $y_t$.

Por lo tanto, las estimaciones de $MCO$ sufrirían un sesgo de simultaneidad en donde los regresores y los términos de error estarían correlacionados.

Es posible transformar el sistema de ecuaciones en una forma compacta: $\mathbf{B x_t= \Gamma_0 + \Gamma_1 x_{t-1}+\varepsilon_t}$

donde $\eqalign{\mathbf{B} = {\left\lbrack \matrix{1 & b_{12} \cr b_{21} & 1} \right\rbrack}}$, $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{y_t \cr z_t} \right\rbrack}}$, $\eqalign{\mathbf{\Gamma_0}= {\left\lbrack \matrix{b_{10} \cr b_{20}} \right\rbrack}}$, $\eqalign{\mathbf{\Gamma_1} = {\left\lbrack \matrix{\gamma_{11} & \gamma_{12} \cr \gamma_{21} & \gamma_{22}} \right\rbrack}}$ y $\eqalign{\mathbf{\varepsilon_t} = {\left\lbrack \matrix{\varepsilon_{yt} \cr \varepsilon_{zt}} \right\rbrack}}$

Al premultiplicar por $\mathbf{B^{−1}}$ se obtiene $\mathbf{x_t= A_0 + A_1x_{t-1}+e_t}$  donde $\mathbf{A_0=B^{−1}\Gamma_0}$, $\mathbf{A_1=B^{−1}\Gamma_1}$, y $\mathbf{e_t=B^{−1}\varepsilon_t}$

Para propósitos de notación, podemos definir
* $a_{i0}$ como el elemento $i$ del vector $\mathbf{A_0}$,
* $a_{ij}$ como el elemento que se encuentra en la fila $i$ y en la columna $j$ de la matriz $\mathbf{A_1}$, y
* $e_{it}$ como el elemento $i$ del vector $\mathbf{e_t}$. 

Usando esta nueva notación, podemos reescribir $\mathbf{x_t= A_0 + A_1x_{t-1}+e_t}$ en la forma equivalente:

$i$) $y_t=a_{10}+a_{11}y_{t-1}+a_{12}z_{t-1}+e_{1t}$ 

$ii$) $z_t=a_{20}+a_{21}y_{t-1}+a_{22}z_{t-1}+e_{2t}$

Para distinguir entre los sistemas representados por $I$ y $II$ versus $i$ y $ii$, el primero se llama un **VAR estructural o sistema primitivo** y el segundo se llama un **VAR en forma estándar**. 

Es importante tener en cuenta que los términos de error (es decir, $e_{1t}$ y $e_{2t}$) son compuestos de los dos choques $\varepsilon_{yt}$  y $\varepsilon_{zt}$. Y se puede demostrar, aunque aquí no lo desarrollaremos, que dado que como $\varepsilon_{yt}$  y $\varepsilon_{zt}$ son procesos de ruido blanco, entonces $e_{1t}$ y $e_{2t}$ son ruido blanco

Un objetivo explícito del enfoque de Box-Jenkins es proporcionar una metodología que conduzca a modelos parsimoniosos. El objetivo final de hacer pronósticos precisos a corto plazo se logra mejor eliminando las estimaciones de parámetros insignificantes del modelo. Sin embargo, Sims (1980) aboga por una estrategia de estimación alternativa. Considere la siguiente generalización multivariada de un proceso autorregresivo $\mathbf{x_t=A_0 +  A_1x_{t-1} + \dots + A_p x_{t-p} + e_t}$ donde 
* $\mathbf{x_t}$ es un vector ($n \times 1$) que reúne las 𝑛 variables incluidas en el $VAR$
* $\mathbf{A_0}$ es un vector ($n \times 1$) de interceptos
* $\mathbf{A_i}$ son las matrices ($n \times n$) de coeficientes
* $\mathbf{e_t}$ es un vector ($n \times 1$) de los términos de error

La metodología de Sims implica únicamente:
1) la determinación de las variables apropiadas para incluir en el $VAR$: Las variables que se incluirán en el $VAR$ se seleccionan de acuerdo con el modelo económico relevante.
2) la determinación de la longitud de rezagos apropiada: Existen una serie de pruebas (las cuales se explicarán más adelante) para hacer esto. No se hace un intento explícito de "reducir" el número de parámetros estimados. La matriz $\mathbf{A_0}$ contiene $n$ parámetros, y cada matriz $\mathbf{A_i}$ contiene $n^2$ parámetros; por lo tanto, $n+pn^2$ coeficientes deben ser estimados. 

Sin lugar a dudas, el $VAR$ esta sobreparameterizado ya que muchas de las estimaciones de los coeficientes serán no significativas. Sin embargo, el objetivo es encontrar las interrelaciones importantes entre las variables. Imponer incorrectamente restricciones nulas puede desperdiciar información importante. Además, es probable que los regresores sean altamente colineales, de modo que las pruebas $t$ sobre los coeficientes individuales no son guías confiables para reducir el modelo.

De todas formas, 

## Pronósticos
Una vez que se ha estimado el $VAR$, se puede utilizar como un modelo de pronóstico de múltiples ecuaciones. 

Suponga que estima el modelo de primer orden $\mathbf{x_t=A_0+A_1x_{t-1}+e_t}$ para obtener los valores de los coeficientes en $\mathbf{A_0}$ y en $\mathbf{A_1}$. Si sus datos van hasta el período $T$, es sencillo obtener los pronósticos de un periodo hacia adelante de sus variables utilizando la relación $E_T\mathbf{x_{T+1}=A_0+A_1x_T}$. De manera similar, se puede obtener un pronóstico de dos periodos hacia adelante utilizando la relación $E_T\mathbf{x_{T+2}=A_0+A_1x_{T+1}=A_0+A_1(A_0+A_1x_T)}$. 

Sin embargo, en un modelo de orden superior, puede haber un gran número de coeficientes estimados. Dado que los 𝑉𝐴𝑅 no restringidos están sobreparametrizados, los pronósticos pueden ser poco confiables. Con el fin de obtener un modelo parsimonioso, muchos pronosticadores eliminarían los coeficientes no significativos del $VAR$. Para después volver a estimar el modelo llamado $near-VAR$ mediante el uso de un $SUR$, para utilizar con fines de pronóstico. 
