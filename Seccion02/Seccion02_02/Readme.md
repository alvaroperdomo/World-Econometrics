## Pruebas de Raíz Unitaria
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

donde los residuos $\hat{e_t}$ provienen de la regresión auxiliar $y_t= \hat{\tau} + hat{\delta} t + hat{e_t}$ y $s^2(l)$ es una estimación de la varianza de largo plazo  $\sigma^2=\displaystyle\lim_{T \to \infty} \displaystyle\frac{E[S_T^2]}{T} $

Siguiendo a Phillips (1987) y Phillips y Perron (1988), $s^2(l)$ se estima como $s^2(l)=\displaystyle\sum_{t=1}^T\hat{e_t}^2$ 

𝑠^2 (𝑙)=(∑_(𝑡=1)^𝑇▒𝑒 ̂_𝑡^2 )/𝑇+2[∑_(𝑗=1)^𝑙▒〖𝑤(𝑗,𝑙) ∑_(𝑡=𝑗+1)^𝑇▒〖𝑒 ̂_𝑡 𝑒 ̂_(𝑡−𝑗) 〗〗]/𝑇 donde:
Las ponderaciones 𝑤(𝑗,𝑙) se pueden establecer iguales a 𝑤(𝑗,𝑙)=1−𝑗/(𝑙+1) ver Newey y West (1987), aunque también son posibles otras ponderaciones. 
La longitud de rezago 𝑙 generalmente se establece proporcional a 𝑇^(1/3), basados en Newey y West (1994).


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

