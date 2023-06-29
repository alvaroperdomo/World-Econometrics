# Prueba de Dickey-Fuller - DF

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

# Prueba Aumentada de Dickey-Fuller - ADF
Tradicionalmente la prueba más utilizada es la **Prueba Aumentada de Dickey-Fuller - ADF**

Esta prueba consiste en estimar estas tres especificaciones

1) $$\Delta y_t = \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
2) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
3) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t$$

En las cuales se debe contrastar la hipótesis nula $\gamma=0$. Para escoger la especificación correcta, se debe tener en cuenta la significancia tanto de $a_0$ como de $t$ y el número de rezagos óptimos ($p$) dentro de las sumatorias se puede escoger utilizando el críterio de Akaike.

**Anotación 1:** Si el valor estimado de $\gamma \notin [-2,0]$, entonces no es necesario hacer prueba de raíz unitaria porque la serie es explosiva

**Anotación 2:** La prueba ADF esta sesgada hacía el no rechazo de la hipótesis nula $\gamma=0$. Por lo tanto, es aconsejable complementar el análisis con hipótesis de más potencia o que tengan el sesgo opuesto. 

### Aplicando las pruebas DF y ADF en R

Para llevar a cabo la prueba DF ofrecemos tress opciones:

**Primera Opción:** Utilice el comando **unitrootTest**
La estructura para hacer la prueba DF es:

``` r
unitrootTest(x, lags = 1, type = c("nc", "c", "ct"), title = NULL, description = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                 | 
|-------------------------|-----------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                           |
| **lags**                | el número máximo de rezagos utilizados en la prueba                                                             |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:              |
|                         |  **"nc"** para una regresión sin intercepto (constante) ni tendencia temporal                                   |
|                         |  **"c"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**|
|                         |  **"ct"** para una regresión con intercepto (constante) y con tendencia temporal                                |
| **title**               | cadena de caracteres que permite darle un título a la prueba                                                    |
| **description**         | cadena de caracteres que permite una breve descripción                                                          | 

``` r
unitrootTest(x, lags = 1, type = c("nc"), title = "Modelo de Paseo Aleatorio", description = NULL)
unitrootTest(x, lags = 1, type = c("c"), title = "Modelo de Paseo Aleatorio con Intercepto", description = NULL)
unitrootTest(x, lags = 1, type = c("ct"), title = "Modelo de Paseo Aleatorio con Intercepto y Tendencia Lineal", description = NULL)
```

**Segunda Opción:** Utilice el comando **urdfTest**
La estructura para hacer la prueba DF es:
``` r
urdfTest(x, lags = 1, type = c("nc", "c", "ct"), doplot = TRUE)
```

| **Argumentos**          | **Descripción**                                                                                                 | 
|-------------------------|-----------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                           |
| **lags**                | el número máximo de rezagos utilizados en la prueba                                                             |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:              |
|                         | **"nc"** para una regresión sin intercepto (constante) ni tendencia temporal                                    |
|                         | **"c"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_** |
|                         | **"ct"** para una regresión con intercepto (constante) y con tendencia temporal                                 |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                             | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico **_Opción Predeterminada_**                                      |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                              |

**Tercera Opción:** Utilice el comando **ur.df**
``` r
ur.df(x, type = c("none", "drift", "trend"), lags = 1, selectlags = c("Fixed", "AIC", "BIC"))
```

| **Argumentos**          | **Descripción**                                                                                                      | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                |
| **lags**                | el número máximo de rezagos utilizados en la prueba                                                                  |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                   |
|                         | **"none"** para una regresión sin intercepto (constante) ni tendencia temporal                                       |
|                         | **"drift"** para una regresión con intercepto (constante) pero sin tendencia temporal - **_Opción Predeterminada_**  |
|                         | **"trend"** para una regresión con intercepto (constante) y con tendencia temporal                                   |
| **selectlags**          | Método escogido para la selección del número de rezagos:                                                             | 
|                         | **"Fixed"** el número de rezagos establecidos en la opción "lags" - **_Opción Predeterminada_**                      |
|                         | **"AIC"** criterio de selección de Akaike (el número máximo de rezagos analizados se establece en la opción "lags")  |
|                         | **"BIC"** criterio de selección Bayesiano (el número máximo de rezagos analizados se establece en la opción "lags")  |
