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

[^1]: **De todas formas, esta prueba no es infalible, por lo que se recomienda acompañarla, como se verá más adelante, con otras pruebas de raíz unitaria.** 

Esta prueba es una generalización de la prueba $DF$ y consiste en estimar estas tres especificaciones:

1) $\eqalign{\Delta y_t = \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t}$
2) $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t}$
3) $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t}$

En las cuales se debe contrastar la hipótesis nula $\gamma=0$. Para determinar el número de rezagos óptimos ($p$) dentro de las sumatorias se puede escoger utilizando el Críterio de Información de Akaike (o el Críterio Bayesiano de Schwartz). Entre más bajo sea el valor de cualquiera de los dos criterios, la especificación del modelo en cuanto al número de rezagos sera mejor.

## El problema con la potencia de la prueba $DF$ y $ADF$

La potencia de una prueba es igual a la probabilidad de rechazar una hipótesis nula falsa. Simulaciones de Monte Carlo han demostrado que la potencia de las diversas versiones de la prueba $DF$ y (y de la prueba $ADF$) puede ser muy bajo. Como tal, estas pruebas indican con demasiada frecuencia que una serie contiene una raíz unitaria. El problema es que, en muestras finitas, cualquier proceso estacionario con tendencia se puede aproximar arbitrariamente bien mediante un proceso de raíz unitaria y cualquier proceso de raíz unitaria se puede aproximar arbitrariamente bien mediante un proceso estacionario con tendencia.

Podría parecer razonable probar la hipótesis $\gamma=0$ utilizando el más general de los modelos: $\Delta y_t = a_0 + \gamma y_{t-1} + a_2t + \varepsilon_t$. Después de todo, si el verdadero proceso es un proceso aleatorio, esta regresión debe encontrar que $a_0=\gamma=a_2=0$. Esta forma de pensar tiene los siguientes dos problemas:

(1) La presencia de los parámetros estimados adicionales reduce los grados de libertad y la potencia de la prueba. La menor potencia significa que no se podrá rechazar la hipótesis de una raíz unitaria cuando, de hecho, no hay una raíz unitaria presente. 

(2) El estadístico apropiado ($\tau$, $\tau_\mu$ 0 $\tau_\tau$) para la prueba $\gamma=0$ depende de los regresores están incluidos en el modelo. En las tablas de Dickey-Fuller, para un nivel dado de significancia, los intervalos de confianza en torno a $\gamma=0$ se expanden bastante si se incluye en el modelo un intercepto y una tendencia temporal. Esto es diferente del caso en el que { $y_t$ } es estacionario. Cuando { $y_t$ } es estacionario, la distribución del estadístico $t$ no depende de la presencia de otros regresores. Por lo tanto, es importante usar la regresión que imite el proceso real de generación de datos. La omisión inadecuada del intercepto o de la tendencia de tiempo puede hacer que el poder de la prueba sea cero. 

Campbell y Perron (1991) encuentran los siguientes resultados con respecto a las pruebas de raíz unitaria $DF$ y $ADF$:

(i) Si la regresión estimada incluye regresores deterministas que no están en el proceso real de generación de datos, la potencia de la prueba de raíz unitaria contra una alternativa estacionaria disminuye a medida que se agregan regresores deterministas adicionales. Por lo tanto, no es deseable incluir regresores que no estén en el proceso de generación de datos.

(ii) Si la regresión estimada omite una variable importante de tendencia determinista presente en el proceso real de generación de datos, como la expresión $a_2t$ en $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t}$, la potencia de la prueba del estadístico $t$ cae a cero a medida que aumenta el tamaño de la muestra. Si la regresión estimada omite una variable sin tendencia (como un intercepto), el estadístico $t$ es consistente, pero la potencia de la muestra finita se ve afectada adversamente y disminuye a medida que aumenta la magnitud del coeficiente en el componente omitido. Por lo tanto, no es deseable omitir los regresores que están en el proceso de generación de datos.

La implicación directa de estos hallazgos es que se puede no rechazar la hipótesis nula de una raíz unitaria debido a una falta de especificación sobre la parte determinista de la regresión. **Muy pocos o demasiados regresores pueden llevar a una falla en la prueba al momento de rechazar la hipótesis nula de una raíz unitaria**. 

**¿Cómo determinar si se debe incluir una tendencia de tiempo o el intercepto en la realización de las pruebas?** 

El problema clave es que las pruebas de raíz unitaria están condicionadas a la presencia de regresores deterministas y las pruebas para la presencia de regresores deterministas están condicionadas a la presencia de una raíz unitaria. 

Aunque nunca podemos estar seguros de que estamos incluyendo los regresores deterministas apropiados en nuestro modelo econométrico, hay algunas pautas útiles.

1. **Grafique siempre los datos**. La inspección visual puede ayudarlo a determinar si hay una tendencia clara en los datos.
2. **Sea claro acerca de la hipótesis nula apropiada y la hipótesis alternativa**. Cuando realice una prueba de $DF$ o $ADF$, siempre calcule el modelo según la hipótesis alternativa e imponga la restricción implícita en la hipótesis nula. Dado que la hipótesis nula es que la serie tiene una raíz unitaria, siempre estime la serie como si fuera estacionaria o con tendencia estacionaria. Por ejemplo, si el gráfico de una serie es creciente (o decreciente). El problema es si la serie es estacionaria o contiene una raíz unitaria más un termino de intercepto. El modelo a estimar tiene la forma $\eqalign{\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t}$. Luego, pruebe las restricciones $\gamma=0$ y/o $\gamma=a_2=0$. No es necesario estimar un modelo sin $a_2t$ ya que la hipótesis alternativa no está representada en dicha especificación.
3. **No es deseable rechazar la hipótesis nula cuando la serie tiene realmente una raíz unitaria o aceptar incorrectamente hipótesis nula de una raíz unitaria cuando la serie es estacionaria o con tendencia estacionaria**. Sin embargo, cualquier prueba implica la posibilidad de cometer tales errores. Como tal, no es deseable realizar pruebas innecesarias. Por ejemplo, si si el gráfico de una serie es claramente creciente (o decreciente) tiene poco sentido probar la restricción $\gamma=a_0=a_2=0$ ya que la variable claramente aumenta (o disminuye) con el tiempo. Probar una restricción en un modelo que ya ha sido restringido crea la posibilidad de agravar sus errores. 

Dado que las pruebas $DF$ y $ADF$ estan sesgadas hacía el no rechazo de la hipótesis nula $\gamma=0$. PEs aconsejable complementar el análisis de raíz unitaria con hipótesis de más potencia (como la prueba $ADF-GLS$) o que tengan el sesgo opuesto (como la prueba $KPSS$). 

**Anotación importante en cuanto a las pruebas $DF$ y $ADF$**: Si el valor estimado de $\gamma \notin [-2,0]$, entonces no es necesario hacer prueba de raíz unitaria porque la serie no cumple la condición $|a_1|<1$ y por lo tanto no es estacionaria

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>


| [Retornar: 2.2. Pruebas de Raíz Unitaria](../Readme.md) | [:house: Inicio](../../../README.md) | [2.2.(R) Aplicación en R de las Pruebas DF-GLS](../Seccion02_02_02_R/Readme.md) |
|---------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------|


