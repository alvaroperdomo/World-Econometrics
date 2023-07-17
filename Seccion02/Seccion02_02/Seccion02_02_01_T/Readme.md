## SECCIÓN 2.2.1. (T)
# Prueba de Dickey-Fuller - $DF$

Cuando en la ecuación $y_t = a_1 y_{t-1} + \varepsilon_t$ se cumple que $a_1=1$, se dice que ésta ecuación es un **paseo aleatorio** (es decir, $y_t$ es una variable no estacionaria). Sin embargo, aunque el valor estimado de $a_1$ cumpla $|a_1|<1$, no se puede afirmar que estadisticamente $a_1 \not= 1$. Por lo tanto, es necesario hacer pruebas de raíz unitaria, en donde ese evalua la hipótesis nula $H_0:a_1=1$. Si no se rechaza $H_0$, entonces $y_t$ es no estacionaria (es decir, tiene raíz unitaria); por otro lado, si se rechaza $H_0$, entonces $y_t$ es no estacionaria.

Para la explicación de la prueba $DF$ note que si resta $y_{t-1}$ a ambos lados de la ecuación $y_t = a_1 y_{t-1} + \varepsilon_t$ se obtiene $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$ donde $\gamma=a_1-1$. Por lo tanto, probar la hipótesis $a_1=1$ es equivalente a probar la hipótesis $\gamma=0$. 

Dickey y Fuller (1979) consideran tres ecuaciones de regresión diferentes que pueden usarse para probar la presencia de una raíz unitaria:
1) $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$ **(Modelo de Paseo Aleatorio)**
2) $\Delta y_t = a_0 + \gamma y_{t-1} + \varepsilon_t$ **(Modelo de Paseo Aleatorio con Intercepto)**
3) $\Delta y_t = a_0 + \gamma y_{t-1} + a_2t + \varepsilon_t$ **(Modelo de Paseo Aleatorio con Intercepto y Tendencia Lineal)**

La diferencia entre las tres regresiones se refiere a la presencia de los elementos deterministas $a_0$ y $a_2t$. La hipótesis de interés en todas las regresiones es si $\gamma=0$. Cuando esto ocurre, la serie { $y_t$ } contiene una raíz unitaria. 

La prueba DF implica estimar una (o más) de las ecuaciones anteriores utilizando Mínimos Cuadrados Ordinarios para obtener el valor estimado de $\gamma$ y el error estándar asociado. La comparación del estadístico $t$ resultante con el valor apropiado informado en las tablas de Dickey-Fuller (y posteriormente actualizadas por MacKinnon (1996)) permiten determinar si acepta o rechaza la hipótesis nula $\gamma=0$. 

Los valores críticos de los estadísticos $t$ dependen de si se incluye un intercepto y/o una tendencia temporal en la regresión. En su estudio de Monte Carlo, Dickey y Fuller (1979) encontraron que los valores críticos para $\gamma=0$ dependen de la forma de la regresión y del tamaño de la muestra. A los estadísticos apropiados para usar en $\Delta y_t = \gamma y_{t-1} + \varepsilon_t$, $\Delta y_t = a_0 + \gamma y_{t-1} + \varepsilon_t$ y $\Delta y_t = a_0 + \gamma y_{t-1} + a_2t + \varepsilon_t$ se les llama $\tau$, $\tau_\mu$ y $\tau_\tau$ son y respectivamente.

Dickey y Fuller (1981) proporcionan tres estadísticos $F$ adicionales (llamados $\phi_1$, $\phi_2$  y $\phi_3$) para probar hipótesis conjuntas sobre los coeficientes: 
* La hipótesis nula $\gamma=a_0=0$ se prueba usando el estadístico $\phi_1$. 
* la hipótesis nula $\gamma=a_0=a_2=0$ se prueba usando el estadístico $\phi_2$, y 
* la hipótesis nula $\gamma=a_2=0$ se prueba usando el estadístico $\phi_3$. 

Los estadísticos $\phi_1$, $\phi_2$, y $\phi_3$ se construyen exactamente de la misma manera que las pruebas $F$: $\eqalign{\phi_i=\frac{\frac{SRC(restringido)-SRC(no restringido)}{r}}{\frac{SRC(no restringido)}{(T-k)}}}$ donde:
* $SRC(restringido)$ y $SRC(no restringido)$: es la suma de los residuos al cuadrado de los modelos restringido y no restringido, respectivamente.
* $r$: es el número de restricciones
* $T$: es el número de observaciones utilizables
* $k$: es el número de parámetros estimados en el modelo sin restricciones
* $T-k$: es el número de grados de libertad en el modelo sin restricciones.

En todas las pruebas $DF$ si el estadistico es menor que el valor crítico reportado en las tablas de Dickey-Fuller, entoces se rechaza $H_0$ y se afirma que la serie $y_t$ es estacionaria. En caso contrario, se dice que la serie tiene raíz unitaria (es decir, es no estacionaria).

# Prueba Aumentada de Dickey-Fuller - $ADF$
Tradicionalmente, la prueba de raíz unitaria más utilizada en la literatura econométrica de series de tiempo es la prueba $ADF$[^1].

[^1]: De todas formas, esta prueba no es infalible, por lo que se recomienda acompañarla, como se verá más adelante, con otras pruebas de raíz unitaria. 

Esta prueba es una generalización de la prueba $DF$ y consiste en estimar estas tres especificaciones:

1) $\eqalign{\Delta y_t = \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t}$
2) $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t}$
3) $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t}$

En las cuales se debe contrastar la hipótesis nula $\gamma=0$. Para escoger la especificación correcta, se debe tener en cuenta la significancia tanto de $a_0$ como de $t$. El número de rezagos óptimos ($p$) dentro de las sumatorias se puede escoger utilizando el Críterio de Información de Akaike (o el Críterio Bayesiano de Schwartz).

## Anotaciones importantes en cuanto a la prueba $ADF$ y por consiguiente en cuanto a la prueba $DF$

**Anotación 1:** Si el valor estimado de $\gamma \notin [-2,0]$, entonces no es necesario hacer prueba de raíz unitaria porque la serie no cumpla $|a_1|<1$ y por lo tanto no es estacionaria

**Anotación 2:** La prueba ADF esta sesgada hacía el no rechazo de la hipótesis nula $\gamma=0$. Por lo tanto, es aconsejable complementar el análisis con hipótesis de más potencia (como la prueba $ADF-GLS$) o que tengan el sesgo opuesto (como la prueba $KPSS$). 

| [Retornar: 02. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../README.md) | [02. Aplicación en R de la Pruebas DF y ADF](../Seccion02_02_01_R/Readme.md)  |
|--------------------------------------------------------|--------------------------------|-------------------------------------------------------------------------------|


<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>
