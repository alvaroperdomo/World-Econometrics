## Pruebas de Raíz Unitaria
### Prueba de Dickey-Fuller - DF
La ecuación $y_t = a_1 y_{t-1} + \varepsilon_t$ tiene raíz unitaria si $a_1=1$. 

Note que si resta $y_{t-1}$ a ambos lados de la ecuación original se obtiene $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$ donde $\gamma=a_1-1$

Por supuesto, probar la hipótesis $a_1=1$ es equivalente a probar la hipótesis $\gamma=0$. 

Dickey y Fuller (1979) consideran tres ecuaciones de regresión diferentes que pueden usarse para probar la presencia de una raíz unitaria:
1) $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$ **(Modelo de Paseo Aleatorio)**
2) $\Delta y_t = a_0 + \gamma y_{t-1} + \varepsilon_t$ **(Modelo de Paseo Aleatorio con Intercepto)**
3) $\Delta y_t = a_0 + \gamma y_{t-1} + a_2t + \varepsilon_t$ **(Modelo de Paseo Aleatorio con Intercepto y Tendencia Lineal)**

La diferencia entre las tres regresiones se refiere a la presencia de los elementos deterministas $a_0$ y $a_2t$. 

La hipótesis de interés en todas las regresiones es si $\gamma=0$. Cuando esto ocurre, la secuencia { $y_t$ } contiene una raíz unitaria. 

La prueba implica estimar una (o más) de las ecuaciones anteriores utilizando MCO para obtener el valor estimado de $\gamma$ y el error estándar asociado. 

La comparación del estadístico $t$ resultante con el valor apropiado informado en las tablas de Dickey-Fuller permite al investigador determinar si acepta o rechaza la hipótesis nula $\gamma=0$. 

Tenga en cuenta que los valores críticos de los estadísticos $t$ dependen de si se incluye una intercepto y/o una tendencia temporal en la regresión. 

En su estudio de Monte Carlo, Dickey y Fuller (1979) encontraron que los valores críticos para $\gamma=0$ dependen de la forma de la regresión y del tamaño de la muestra. Los estadísticos llamados $\tau$, $\tau_\mu$ y $\tau_\tau$ son los estadísticos apropiados para usar en $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$, $\Delta y_t = a_0 + \gamma y_{t-1} + \varepsilon_t$ y $\Delta y_t = a_0 + \gamma y_{t-1} + a_2t + \varepsilon_t$ y respectivamente.

Dickey y Fuller (1981) proporcionan tres estadísticos $F$ adicionales (llamados $\phi_1$, $\phi_2$  y $\phi_3$) para probar hipótesis conjuntas sobre los coeficientes. 
* La hipótesis nula $\gamma=a_0=0$ se prueba usando el estadístico $\phi_1$. 
* la hipótesis nula $\gamma=a_0=a_2=0$ se prueba usando el estadístico $\phi_2$, y 
* la hipótesis nula $\gamma=a_2=0$ se prueba usando el estadístico $\phi_3$. 

Los estadísticos $\phi_1$, $\phi_2$, y $\phi_3$ se construyen exactamente de la misma manera que las pruebas $F$: $$\phi_i=\frac{\frac{SRC(restringido)-SRC(no restringido)}{r}}{\frac{SRC(no restringido)}{(T-k)}}$$
donde
* $SRC(restringido)$ y $SRC(no restringido)$: es la suma de los residuos al cuadrado de los modelos restringido y no restringido, respectivamente.
* $r$: es el número de restricciones
* $T$: es el número de observaciones utilizables
* $k$: es el número de parámetros estimados en el modelo sin restricciones
* $T-k$: es el número de grados de libertad en el modelo sin restricciones.


### Prueba Aumentada de Dickey-Fuller - ADF
Tradicionalmente la prueba más utilizada es la **Prueba Aumentada de Dickey-Fuller - ADF**

Esta prueba consiste en estimar estas tres especificaciones

1) $$\Delta y_t = \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
2) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
3) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t$$

En las cuales se debe contrastar la hipótesis nula $\gamma=0$. Para escoger la especificación correcta, se debe tener en cuenta la significancia tanto de $a_0$ como de $t$ y el número de rezagos óptimos ($p$) dentro de las sumatorias se puede escoger utilizando el críterio de Akaike.

**Anotación 1:** Si el valor estimado de $\gamma \notin [-2,0]$, entonces no es necesario hacer prueba de raíz unitaria porque la serie es explosiva

**Anotación 2:** La prueba ADF esta sesgada hacía el no rechazo de la hipótesis nula $\gamma=0$. Por lo tanto, es aconsejable complementar el análisis con hipótesis de más potencia o que tengan el sesgo opuesto. 

### Prueba de Mínimos Cuadrados Generalizados de Dickey-Fuller (DF-GLS)
Elliott, Rothenberg y Stock (1996) muestran que es posible mejorar el poder de la prueba al estimar el modelo utilizando algo cercano a las primeras diferencias. 

Considere el modelo de tendencia estacionaria: $y_t=a_0+a_2t+B(L) \varepsilon_t$. En lugar de crear la primera diferencia de $y_t$, Elliott, Rothenberg y Stock preseleccionan una constante cercana a 1, digamos $\alpha$, y restan $\alpha y_{t-1}$  de $y_t$ para obtener $\tilde{y_t}=a_0+a_2 t - \alpha a_0 - \alpha a_2 (t-1) + e_t$ pata $t=2,...,T$ donde $\tilde{y_t}=y_t-\alpha y_{t-1}$ y $e_t$ es un término de error estacionario.

Para $t=1$, la diferencia no es factible por lo que se asume $\tilde{y_1}=y_1$

Para obtener las estimaciones deseadas de $a_0$ y $a_2$ estime por MCO $\tilde{y_t}=a_0 z_{1t}+a_2 z_{2t} + e_t$ donde $z_{1t}=(1- \alpha)$ y $z_{2t}=\alpha + (1- \alpha)t$

El punto importante es que las estimaciones de $a_0$ y $a_2$ se pueden usar para calcular $y_t^d = y_t - \tilde{a_0} - \tilde{a_2}t$

En el segundo paso del procedimiento, se estima: $\Delta y_t^d = \gamma y_{t-1}^d + \varepsilon_t$. Si existe una correlación serial en los residuos, la forma aumentada de la prueba se puede estimar como $$\Delta y_t^d = \gamma y_{t-1}^d + \sum_{i=1}^p c_i \Delta y_{t-i}^d + \varepsilon_t$$

Elliott, Rothenberg y Stock (1996) recomiendan seleccionar la longitud del rezago $p$ utilizando el _Criterio Bayesiano de Schwartz_. La hipótesis nula de una raíz unitaria puede rechazarse si se encuentra el $\gamma \ne 0$. 

Los valores críticos de la prueba dependen de si se incluye una tendencia en la prueba. 
* Si hay un intercepto pero no una tendencia, los valores críticos son precisamente los mismos de la prueba de Dickey-Fuller. En esencia, utiliza los valores críticos de Dickey-Fuller como si no hubiera un intercepto en el proceso generador de datos. 
* Si hay una tendencia, los valores críticos dependen del valor de $\alpha$ seleccionado para construir la variable $\tilde{y_t}$. Elliott, Rothenberg y Stock (1996) informan que el valor de $\alpha$ que parece proporcionar la mejor potencia global es $\alpha=(1-\displaystyle\frac{7}{t})$  para el caso de un intercepto y  $\alpha=(1-\displaystyle\frac{13.5}{t})$ si hay un intercepto y una tendencia. 

### Prueba de Kwiatkowski, Phillips, Schmidt y Shin - KPSS

Dado que la potencia de las pruebas de raíz unitaria no es particularmente alta, también puede ser interesante aplicar pruebas en donde la hipótesis nula es de estacionariedad, para evitar que podamos concluir erróneamente que una serie de tiempo tiene una raíz unitaria debido a las propiedades estadísticas de la prueba aumentada de Dickey-Fuller.
Una prueba que toma la estacionariedad como hipótesis nula es la de Kwiatkowski, Phillips, Schmidt y Shin (1992) [KPSS]. 

La prueba KPSS se basa en la idea de descomponer una serie de tiempo en la suma de :
* una tendencia determinística $\delta_t$, 
* un paseo aleatorio $S_t$ o tendencia estocástica (en otras palabras, $S_t=\displaystyle\sum_{i=1}^{t} \varepsilon_i = S_{t-1} + \varepsilon_t$ con $S_t=0$), y  
* un proceso de error estacionario $u_t$. 

Es decir, $$y_t = \delta t + S_t + u_t$$

Cuando la varianza de $varepsilon_t$, denotada como $\sigma^2$, es igual a cero, el componente de paseo aleatorio $S_t$ se vuelve una constante por lo que la serie de tiempo $y_t = \delta t + u_t$ en este caso es estacionaria. 

La hipótesis nula de estacionariedad que se va a probar en la KPSS está dada por $\sigma^2=0$.

En Kwiatkowski, Phillips, Schmidt y Shin (1992), el estadístico de prueba se calcula como 
$$\hat{\eta}=\displaystyle\frac{1}{T^2 s^2(l)} \displaystyle\sum_{i=1}^T (\displaystyle\sum_{i=1}^t \hat{e_i})^2$$ 

donde los residuos $\hat{e_t}$ provienen de la regresión auxiliar $y_t= \hat{\tau} + \hat{\delta} t + \hat{e_t}$ y $s^2(l)$ es una estimación de la varianza de largo plazo  $\sigma^2=\displaystyle\lim_{T \to \infty} \displaystyle\frac{E[S_T^2]}{T} $

Siguiendo a Phillips (1987) y Phillips y Perron (1988), $s^2(l)$ se estima como $s^2(l)=\displaystyle\frac{\displaystyle\sum_{t=1}^T\hat{e_t}^2}{T} + \frac{2\displaystyle\sum_{j=1}^lw(j,l)\displaystyle\sum_{t=j+1}^T\hat{e_t}\hat{e_{t-j}}}{T}$ donde:
* Las ponderaciones $w(j,l)$ se pueden establecer iguales a $w(j,l)=1-\frac{j}{l+1}$ ver Newey y West (1987), aunque también son posibles otras ponderaciones. 
* La longitud de rezago l generalmente se establece proporcional a $T^{1/3}$, basados en Newey y West (1994).

La distribución asintótica del estadístico de prueba $\hat{\eta}$, tal como se explica en Kwiatkowski, Phillips, Schmidt y Shin (1992) depende de si la serie tiene tendencia o no.

### Prueba de Perron

Al realizar pruebas de raíz unitaria, se debe tener especial cuidado si se sospecha que ha ocurrido un cambio estructural. 

Cuando hay cambios estructurales, los diversos estadísticos de la prueba ADF están sesgados hacia el no rechazo de una raíz unitaria. 

Como ejemplo, considere la situación en la que hay un cambio único en la media de una secuencia estacionaria. En el gráfico  de abajo la secuencia { $y_t$ } se construyó de manera que fuera estacionaria alrededor de una media de 0 para $t=0, …, 50$ y luego lo fuera alrededor de una media de 6 para $t=51, …, 100$. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5e70a54a-0564-4c7a-bde3-9812c475657d)



## Pruebas de Raíz Unitaria en R

Para esta sección hay que instalar el paquete [fUnitRoots](https://cran.r-project.org/web/packages/fUnitRoots/index.html)

``` r
install.packages('fUnitRoots')
```

| Subsecciones                                                                                        | Alcance                                                              | Dedicación,<br>5.5 horas  | 
|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|:-------------------------:|
| [La Prueba ADF: _Prueba Aumentada de Dickey Fuller_](Section01/WhatIsLTWB)                          | Explicación general de la prueba                                     |             1.5           | 
| [La Prueba KPSS: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_](Section01/Requirement)          | Explicación general de la prueba                                     |             0.5           |   
| [La Prueba ERZ: _Prueba de Elliott, Rothenberg y Stock_](Section01/Requirement)                     | Explicación general de la prueba                                     |             0.5           |  
| [La Prueba ZA: _Prueba de Zivot y Andrews_](Section01/CaseStudy)                                    | Explicación general de la prueba                                     |             0.5           |       

