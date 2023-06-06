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

