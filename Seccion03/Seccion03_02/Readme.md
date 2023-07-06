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

Tenga en cuenta que los términos { $\varepsilon_{yt}$ y$\varepsilon_{zt}$  son innovaciones puras (o choques) en $y_t$ y $z_t$, respectivamente. Por supuesto, 
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



