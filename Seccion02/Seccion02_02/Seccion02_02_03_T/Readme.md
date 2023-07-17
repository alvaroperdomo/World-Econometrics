## SECCIÓN 2.2.3. (T)
# Prueba de Kwiatkowski, Phillips, Schmidt y Shin - KPSS

Dado que la potencia de las pruebas de raíz unitaria no es particularmente alta, también puede ser interesante aplicar pruebas en donde la hipótesis nula es de estacionariedad, para evitar que podamos concluir erróneamente que una serie de tiempo tiene una raíz unitaria debido a las propiedades estadísticas de la prueba $ADF$. Una prueba que toma la estacionariedad como hipótesis nula es la de Kwiatkowski, Phillips, Schmidt y Shin (1992) [ $KPSS$ ]. 

La prueba $KPSS$ se basa en la idea de descomponer una serie de tiempo en la suma de :
* una tendencia determinística $\delta_t$, 
* un paseo aleatorio $S_t$ o tendencia estocástica (en otras palabras, $S_t=\displaystyle\sum_{i=1}^{t} \varepsilon_i = S_{t-1} + \varepsilon_t$ con $S_t=0$), y  
* un proceso de error estacionario $u_t$. 

Es decir, $y_t = \delta t + S_t + u_t$

Cuando la varianza de $\varepsilon_t$, denotada como $\sigma^2$, es igual a cero, el componente de paseo aleatorio $S_t$ se vuelve una constante por lo que la serie de tiempo $y_t = \delta t + S_t u_t$ en este caso es estacionaria. La hipótesis nula de estacionariedad que se va a probar en la $KPSS$ está dada por $\sigma^2=0$.

En Kwiatkowski, Phillips, Schmidt y Shin (1992), el estadístico de prueba se calcula como $\eqalign{\hat{\eta}=\frac{1}{T^2 s^2(l)}\sum_{i=1}^T (\sum_{i=1}^t \hat{e_i})^2}$ donde 
* los residuos $\hat{e_t}$ provienen de la regresión auxiliar $y_t= \hat{\tau} + \hat{\delta} t + \hat{e_t}$ y
* $s^2(l)$ es una estimación de la varianza de largo plazo  $\sigma^2=\displaystyle\lim_{T \to \infty} \displaystyle\frac{E[S_T^2]}{T} $

Siguiendo a Phillips (1987) y Phillips y Perron (1988), $s^2(l)$ se estima como $s^2(l)=\displaystyle\frac{\displaystyle\sum_{t=1}^T\hat{e_t}^2}{T} + \frac{2\displaystyle\sum_{j=1}^lw(j,l)\displaystyle\sum_{t=j+1}^T\hat{e_t}\hat{e_{t-j}}}{T}$ donde:
* Las ponderaciones $w(j,l)$ se pueden establecer iguales a $w(j,l)=1-\frac{j}{l+1}$ como establecen Newey y West (1987), aunque también son posibles otras ponderaciones. 
* La longitud de rezago l generalmente se establece proporcional a $T^{1/3}$, basados en Newey y West (1994).

La distribución asintótica del estadístico de prueba $\hat{\eta}$, tal como se explica en Kwiatkowski, Phillips, Schmidt y Shin (1992) depende de si la serie tiene tendencia o no.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
