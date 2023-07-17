## SECCIÓN 2.2.2. (T)
# Prueba de Mínimos Cuadrados Generalizados de Dickey-Fuller (DF-GLS)

Esta prueba, propuesta por Elliott, Rothenberg y Stock (1996) se estructura tal como se explica a continuación:

Considere el modelo de tendencia estacionaria: $y_t=a_0+a_2t+\displaystyle\sum_{i = 0}^{q} \beta_i\varepsilon_t$. En lugar de crear, como en el modelo $ADF$, la primera diferencia de $y_t$, preseleccione una constante cercana a $1$, digamos $\alpha$, y reste $\alpha y_{t-1}$ de $y_t$, así: $\eqalign{y_t-\alpha y_{t-1}=a_0+a_2t+\displaystyle\sum_{i = 0}^{q} \beta_i\varepsilon_t - \alpha[a_0+a_2(t-1)+\displaystyle\sum_{i = -1}^{q} \beta_i\varepsilon_t]}$. 

Por lo tanto, $\tilde{y_t}=a_0(1-\alpha)+a_2 [\alpha + (1- \alpha)t] + e_t$ para $t=2,\dots,T$ donde $\tilde{y_t}=y_t-\alpha y_{t-1}$ y $e_t$ es un término de error estacionario. Para $t=1$, la diferencia no es factible por lo que se asume $\tilde{y_1}=y_1$

Para obtener las estimaciones deseadas de $a_0$ y $a_2$ estime por Mínimos Cuadrados Ordinarios $\tilde{y_t}=a_0 z_{1t}+a_2 z_{2t} + e_t$ donde $z_{1t}=(1- \alpha)$ y $z_{2t}=\alpha + (1- \alpha)t$. El punto a tener en cuenta es que las estimaciones de $a_0$ y $a_2$ se pueden usar para calcular $y_t^d = y_t - \tilde{a_0} - \tilde{a_2}t$

En el segundo paso del procedimiento, se estima: $\Delta y_t^d = \gamma y_{t-1}^d + \varepsilon_t$. Si existe una correlación serial en los residuos, la forma aumentada de la prueba se puede estimar como $\eqalign{\Delta y_t^d = \gamma y_{t-1}^d + \sum_{i=1}^p c_i \Delta y_{t-i}^d + \varepsilon_t}$

Elliott, Rothenberg y Stock (1996) recomiendan seleccionar la longitud del rezago $p$ utilizando el _Criterio Bayesiano de Schwartz_. La hipótesis nula de una raíz unitaria puede rechazarse si se encuentra el $\gamma \ne 0$. 

Los valores críticos de la prueba dependen de si se incluye una tendencia en la prueba. 
* Si hay un intercepto pero no una tendencia, los valores críticos son precisamente los mismos de la prueba de Dickey-Fuller. En esencia, utiliza los valores críticos de Dickey-Fuller como si no hubiera un intercepto en el proceso generador de datos. 
* Si hay una tendencia, los valores críticos dependen del valor de $\alpha$ seleccionado para construir la variable $\tilde{y_t}$. Elliott, Rothenberg y Stock (1996) informan que el valor de $\alpha$ que parece proporcionar la mejor potencia global es $\alpha=(1-\displaystyle\frac{7}{t})$  para el caso de un intercepto y  $\alpha=(1-\displaystyle\frac{13.5}{t})$ si hay un intercepto y una tendencia. 

Elliott, Rothenberg y Stock (1996) muestran que la prueba $DF-GLS$ mejora el poder de la prueba $ADF$. 

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
