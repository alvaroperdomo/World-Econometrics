# Pruebas de Raíz Unitaria

Para esta sección hay que instalar los paquetes
* [fUnitRoots](https://cran.r-project.org/web/packages/fUnitRoots/index.html)
* [urca](https://cran.r-project.org/web/packages/urca/index.html)

``` r
install.packages('fUnitRoots')
install.packages('urca')
```

| Pruebas                                                                                 | Explicación general de la prueba|  Aplicación en R |
|-----------------------------------------------------------------------------------------|:-------------------------------:|:----------------:|
| Las Pruebas DF y ADF: _Pruebas de Dickey Fuller y Aumentada de Dickey Fuller_           |  X                              |             X    | 
| La Prueba KPSS: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_                       |  X                              |             X    |   
| La Prueba ERZ: _Prueba de Elliott, Rothenberg y Stock_                                  |  X                              |             X    |
| Pruebas de Cambio Estructural - P: _Prueba de Perron y ZA: _Prueba de Zivot y Andrews_  |  X                              |             X    |


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

##### Aplicando las pruebas DF en R
Dado la prueba ADF es una extensión de la prueba DF, entonces la aplicación de la prueba DF en R se explica después de que se explique la prueba ADF

### Prueba Aumentada de Dickey-Fuller - ADF
Tradicionalmente la prueba más utilizada es la **Prueba Aumentada de Dickey-Fuller - ADF**

Esta prueba consiste en estimar estas tres especificaciones

1) $$\Delta y_t = \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
2) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + \varepsilon_t$$
3) $$\Delta y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i \Delta y_{t-i+1} + a_2 t + \varepsilon_t$$

En las cuales se debe contrastar la hipótesis nula $\gamma=0$. Para escoger la especificación correcta, se debe tener en cuenta la significancia tanto de $a_0$ como de $t$ y el número de rezagos óptimos ($p$) dentro de las sumatorias se puede escoger utilizando el críterio de Akaike.

**Anotación 1:** Si el valor estimado de $\gamma \notin [-2,0]$, entonces no es necesario hacer prueba de raíz unitaria porque la serie es explosiva

**Anotación 2:** La prueba ADF esta sesgada hacía el no rechazo de la hipótesis nula $\gamma=0$. Por lo tanto, es aconsejable complementar el análisis con hipótesis de más potencia o que tengan el sesgo opuesto. 

##### Aplicando las pruebas DF y ADF en R

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
| **title**               | caracteres que permite darle un título a la prueba                                                              |
| **description**         | cadena de caracteres que permite una breve descripción                                                          | 

La prueba se va a hacer con respecto al **PIB per cápita de Colombia** utilizando el siguiente código:

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

#### Aplicando las pruebas DF-GLS en R

Para llevar a cabo la prueba DF ofrecemos dos opciones:

**Primera Opción:** Utilice el comando **unitrootTest**
La estructura para hacer la prueba DF-GLS es:

``` r
urersTest(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"), lag.max = 4, doplot = TRUE)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                      |
|                         | el número máximo de rezagos utilizados para probar el truncamiento del rezago decendente para la "prueba P", utilizando el método Bayesiano  |   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para "DF-GLS"                                           |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                | 
|                         | **"constant"** - **_Opción Predeterminada_**                                                                                                 |
|                         | **"trend"**                                                                                                                                  |
| **lag.max**             | el número máximo de rezagos utilizados en la prueba                                                                                          |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico **_Opción Predeterminada_**                                                                   |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

**Segunda Opción:** Utilice el comando **ur.ers**
``` r
ur.ers(x, type = c("DF-GLS", "P-test"), model = c("constant", "trend"),lag.max = 4)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **lag.max**             | esta opción puede significar dos cosas:                                                                                                      |
|                         | el número máximo de rezagos utilizados para probar el truncamiento del rezago decendente para la "prueba P", utilizando el método Bayesiano  |   
|                         | el número máximo de diferencias rezagadas que se incluirán en la regresión de prueba para "DF-GLS"                                           |
| **type**                | cadena de caracteres que describa el tipo de regresión de raíz unitaria. Las opciones válidas son:                                           |
|                         | **"DF-GLS"** - **_Opción Predeterminada_**                                                                                                   |
|                         | **"P-test"**                                                                                                                                 |
| **model**               | El modelo determinista utilizado para eliminar la tendencias:                                                                                | 
|                         | **"constant"** - **_Opción Predeterminada_**                                                                                                 |
|                         | **"trend"**                                                                                                                                  |

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

##### Aplicando la prueba KPSS en R

Para llevar a cabo la prueba KPSS ofrecemos dos opciones:

**Primera Opción:** Utilice el comando **unitrootTest**

``` r
urkpssTest(x, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** - **_Opción Predeterminada_**                                                                                                       |
|                         | **"tau"**                                                                                                                                    |
| **lags**                | el máximo número de rezagos:                                                                                                                 |
| **use.lag**             | cadena de caracteres que especifica el número de rezagos                                                                                     |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico - **_Opción Predeterminada_**                                                                 |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

**Segunda Opción:** Utilice el comando **ur.kpss*
``` r
ur.kpss(y, type = c("mu", "tau"), lags = c("short", "long", "nil"), use.lag = NULL)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **type**                | Las opciones válidas son:                                                                                                                    |
|                         | **"mu"** - **_Opción Predeterminada_**                                                                                                       |
|                         | **"tau"**                                                                                                                                    |
| **lags**                | el máximo número de rezagos                                                                                                                  |
| **use.lag**             | número de rezagos especificados por el usuario                                                                                               |


### El cambio estructural

Al realizar pruebas de raíz unitaria, se debe tener especial cuidado si se sospecha que ha ocurrido un cambio estructural. 

Cuando hay cambios estructurales, los diversos estadísticos de la prueba ADF están sesgados hacia el no rechazo de una raíz unitaria. 

Como ejemplo, considere la situación en la que hay un cambio único en la media de una secuencia estacionaria. En el gráfico  de abajo la secuencia { $y_t$ } se construyó de manera que fuera estacionaria alrededor de una media de 0 para $t=0, …, 50$ y luego lo fuera alrededor de una media de 6 para $t=51, …, 100$. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/5e70a54a-0564-4c7a-bde3-9812c475657d)

La secuencia se formó:
* Simulando 100 valores distribuidos normalmente e independientemente para la secuencia { $\varepsilon_t$ }.  
* Asumiendo $y_0=0$. 
* Los siguientes 100 valores en la secuencia fueron generados usando la fórmula: $y_t=0.5y_{t-1}+ \varepsilon_t + D_L$ donde 

$$
D_L=\left\[\
\begin{array}{ll}
0 & \text{para  } t=1,...,50 \\
3 & \text{para  } t=51,...,100
\end{array}
\right.
$$

El subíndice $L$ indica que el nivel de la variable dummy $D_L$ cambia. 
##### En la práctica, el cambio estructural puede no ser tan evidente como se muestra en la figura de arriba.

Sin embargo, ésta es útil para ilustrar el problema de usar una prueba de Dickey-Fuller: 
* La línea recta del gráfico es la mejor estimación de M.C.O ($y_t=a_0+a_2t+e_t$, donde $a_0<0$ y $a_2>0$) plantea erróneamente que la serie tiene una tendencia determinista. 
* La forma correcta de estimar $y_t=0.5y_{t-1}+\varepsilon_t+D_L$ es un modelo AR(1) en el que se permita  que la intersección cambie al incluir la variable dummy $D_L$. 
* Sin embargo, suponga que se estima la ecuación: $y_t=a_0+a_1y_{t-1}+e_t+D_L$. Como se puede inferir de la figura, el valor estimado de $a_1$ está necesariamente sesgado hacia 1. 

La razón de este sesgo ascendente es que:
* el valor estimado de $a_1$ captura la propiedad de que los valores "bajos" de $y_t$ (es decir, aquellos que fluctúan alrededor de 0) están seguidos por otros valores "bajos" y 
* los valores "altos" (es decir, aquellos que fluctúan alrededor de una media de 6) son seguidos por otros valores "altos". 

Para una demostración formal, tenga en cuenta que a medida que $a_1$ se acerca a 1, $y_t=a_0+a_1y_{t-1}+e_t$ se acerca a un paseo aleatorio con intercepto. Sabemos que la solución del paseo aleatorio con intercepto incluye una tendencia determinista; es decir,  $y_t=y_0+a_0t+\displaystyle\sum_{i=1}^t \varepsilon_i$

Por lo tanto, la ecuación mal especificada $y_t=a_0+a_1y_{t-1}+e_t$ tenderá a imitar la línea de tendencia mostrada en la figura de arriba a sesgando $a_1$ hacia 1. Este sesgo en $a_1$ significa que la prueba ADF está sesgada hacia la aceptación de la hipótesis nula de una raíz unitaria, aunque la serie es estacionaria dentro de cada uno de los subperíodos.

Por supuesto, un proceso de raíz unitaria también puede exhibir un cambio estructural. La figura de abajo simula un paseo aleatorio con un cambio estructural que se produce en $t=51$. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/452c868c-1da9-447c-be1d-bb36eb69e366)

Esta segunda simulación utilizó:
* Los mismos 100 valores para la secuencia { $\varepsilon_t$ } y 
* la condición inicial $y_0=2$. 
* Las 100 realizaciones siguientes de la secuencia $y_t$ se construyeron como $y_t=y_{t-1}+\varepsilon_t+D_P$ donde $D_P$ es una variable dummy de pulso tal que en el periodo 51 $D_P(51)=4$  y en todos los demás valores $D_P=0$.
Aquí, el subíndice $P$ se refiere al hecho de que hay un único pulso en la variable dummy. 

En un proceso de raíz unitaria, un único pulso en la dummy tendrá un efecto permanente en el nivel de la secuencia { $y_t$ }. En $t=51$, el pulso en la dummy es equivalente a un choque \varepsilon_{t+51}$ de cuatro unidades adicionales. 

Por lo tanto, el choque de una sola vez a $D_P(51)$ tiene un efecto permanente en el valor medio de la secuencia para $t \ge 51$. 

Este sesgo en las pruebas de Dickey-Fuller se confirmó en un experimento de Monte Carlo. 

Perron (1989) generó 10.000 repeticiones de un proceso de la misma naturaleza que $y_t=0.5y_{t-1}+\varepsilon_t+D_L$. * Cada replica la formó:
* generando 100 valores distribuidos normalmente e independientemente para la secuencia { $\varepsilon_t }. 
* Para cada una de las 10,000 series replicadas, usó MCO para estimar una regresión en la forma de $y_t=a_0+a_1y_{t-1}+e_t$. 

Perron descubrió que los valores estimados de $a_1$ estaban sesgados hacia 1. Además, el sesgo se hizo más pronunciado a medida que aumentaba la magnitud del cambio.

## Prueba de Perron
Volviendo a las dos gráficas de arriba, puede haber casos en los que a ojo sin ayuda no se puede detectar fácilmente la diferencia entre los tipos alternativos de secuencias. 

Un procedimiento econométrico para probar las raíces unitarias en presencia de una cambio estructural implica dividir la muestra en dos partes y usar las pruebas ADF en cada parte. 

El problema con este procedimiento es que los grados de libertad para cada una de las regresiones resultantes disminuyen. 

Perron (1989) desarrolla un procedimiento formal para probar la presencia de raíz unitaria cuando hay un cambio estructural en el período de tiempo $t=\tau+1$. 

Considere la hipótesis nula de un salto de una sola vez en el nivel de un proceso de raíz unitaria frente a la hipótesis alternativa de un cambio de una sola vez en el intercepto de un proceso estacionario en tendencia. 
Formalmente, dejemos que las hipótesis nula y alternativa sean:
* $H_0: y_t= a_0 + y_{t-1}+ \mu_1 D_P + \varepsilon_t$ donde 

$$
D_P=\left\[\
\begin{array}{ccc}
1 & \text{si  } t= \tau+1 \\
0 & \text{si  } t \ne \tau+1 \\
\end{array}
\right.
$$

* $H_1: y_t= a_0 + a_2t+ \mu_2 D_L + \varepsilon_t$ donde 

$$
D_L=\left\[\
\begin{array}{ccc}
1 & \text{si  } t \gt \tau \\
0 & \text{si  } t \le \tau \\
\end{array}
\right.
$$

Bajo la hipótesis nula, { $y_t$ }  es un proceso de raíz unitaria con un salto de una sola vez en el nivel de la secuencia en el período $t=/tau+1. 

Bajo la hipótesis alternativa, { $y_t$ }   es una tendencia estacionaria con un salto de una sola vez en el intercepto. 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/3b118e20-4f18-4cfb-907b-6ee107a30d44)

La figura de arriba puede ayudar a visualizar las dos hipótesis:
* Simulando $y_t=0.2+y_{t-1}+\mu_1 D_P(51) + \varepsilon_t$ y usando 100 realizaciones para la secuencia { $\varepsilon_t$ }, la línea discontinua errática representa la trayectoria en el tiempo de { $y_t$ }  bajo $H_0$. Antes y después del salto en el periodo 51, la secuencia { $y_t$ }  sigue el mismo paseo aleatorio con intercepto. 
* $H_1$  postula que la secuencia { $y_t$ }  es estacionaria alrededor de la línea de tenden-cia discontinua. Hasta $t=\tau$, { $y_t$ }  es estacionaria alrededor de $a_0+a_2t$, y comenzando en $\tau+1$, $y_t$ es estacionaria alrededor de $a_0+a_2t+\mu_2$. Como lo ilustra la línea continua, hay un aumento de una sola vez en el intercepto de la tendencia si $\mu_2 \gt 0$.

El problema econométrico es determinar si una serie observada se modela mejor con $H_0$ o con $H_1$. La implementación de la técnica de Perron (1989) es sencilla:

**PASO 1:** No hay una forma directa de restringir los coeficientes de $H_1$  para obtener $H_0$. Como tal, necesitamos combinar ambas hipótesis de la siguiente manera: $y_t=a_0+a_1y_{t-1}+a_2t+\mu_1 D_P+\mu_2 D_L+ \varepsilon_t$

**PASO 2:** Estime la ecuación formada en el Paso 1. Bajo la hipótesis nula de una raíz unitaria, $a_1=1$. Perron (1989) muestra que, cuando los residuos están idéntica e independientemente distribuidos, la distribución de $a_1$ depende de la proporción de observaciones que se producen antes del cambio.
#### Sea esta proporción $\lambda = \displaystyle\frac{\tau}{T} $

**PASO 3:** Realice las verificaciones de diagnóstico para determinar si los residuos del Paso 2 están serialmente correlacionados. Si hay correlación serial, use la forma aumentada de la regresión $y_t=a_0+a_1y_{t-1}+a_2t+\mu_1 D_P+\mu_2 D_L+ \displaystyle\sum_{i = 1}^{p} \beta_i \Delta y_{t-i} + \varepsilon_t$

**PASO 4:** Calcule el estadístico $t$ para la hipótesis nula $a_1=1$. Este estadístico puede compararse con los valores críticos calculados por Perron. 

Perron generó 5000 series de acuerdo con $H_1$ usando valores de $\lambda$ que van de 0 a 1 en incrementos de 0.1. Para cada valor de $\lambda$, estimó cada una de las regresiones y calculó la distribución muestral de $a_1$. 

Los valores críticos son idénticos a los estadísticos de Dickey-Fuller cuando $\lambda=0$ y $\lambda=1$; en efecto, no hay cambio estructural a menos que $0 \lt \lambda \lt 1$. 

La diferencia máxima entre los dos estadísticos se produce cuando $\lambda=0.5$. 

Para $\lambda=0.5$, el valor crítico del estadístico $t$ en el nivel de significancia del 5% es −3.76 (que es mayor en valor absoluto que el estadístico Dickey-Fuller correspondiente de −3.41). 

Si encuentra un estadístico $t$ mayor que el valor crítico calculado por Perron, es posible rechazar la hipótesis nula de una raíz unitaria. 

La metodología es bastante general, ya que también puede permitir un cambio único en el intercepto o un cambio único tanto en la media como en el intercepto. 

Por ejemplo, es posible probar la hipótesis nula de un cambio permanente en el intercepto frente a la alternativa de un cambio en la pendiente de la tendencia.  

$H_0: y_t= a_0 + y_{t-1} + \mu_2 D_L + \varepsilon_t$ donde 

$$
D_L=\left\[\
\begin{array}{ccc}
1 & \text{para  } t \gt tau \\
0 & \text{para  } t \le tau \\
\end{array}
\right.
$$

$H_1: y_t= a_0 + a_2t + \mu_3 D_T + \varepsilon_t$ donde

$$
D_T=\left\[\
\begin{array}{ccc}
t-\tau & \text{para  } t \gt \tau \\
0 & \text{para  } t \le \tau \\
\end{array}
\right.
$$

En  $H_0$, la secuencia { $y_t$ } se genera por $\Delta y_t = a_0 + \varepsilon_t$ hasta el período $\tau$ y por  $\Delta y_t = a_0 + \mu_2 + \varepsilon_t$ a partir de entonces. 

Si $\mu_2 \gt 0$, la magnitud del intercepto aumenta para $t \gt \tau$. 

De manera similar, se produce una reducción en el intercepto si $\mu_2 \lt 0$.

En $H_1$  se postula una serie estacionaria en tendencia con un cambio uniforme en la pendiente de la tendencia a partir de $t \gt tau$:
* positivo si $\mu_3 \gt 0$ y 
* negativo si $\mu_3 \lt 0$ 

Para ser aún más general, es posible combinar las dos hipótesis previamente analizadas:

$H_0: y_t= a_0 + y_{t-1} + \mu_1 D_P + \mu_2 D_L + \varepsilon_t$

$H_1: y_t= a_0 + a_2t + \mu_2 D_L + \mu_3 D_T + \varepsilon_t$

Nuevamente, el procedimiento implica combinar las hipótesis nula y alternativa en una sola ecuación. Considere $H_0: y_t= a_0 + a_1 y_{t-1} + \mu_1 D_P + \mu_2 D_L + \mu_3 D_T + \varepsilon_t$ 

Compare el estadístico $t$ de la estimación de $a_1$ con el valor crítico calculado por Perron (1989). Si los errores de esta regresión no parecen ser ruido blanco, estime la ecuación en la forma de una prueba aumentada de Dickey-Fuller. El estadístico 𝑡 para la hipótesis nula $a_1=1$ puede compararse con los valores críticos calculados por Perron (1989)

## Prueba de Zivot y Andrews
Se debe tener cuidado al usar el procedimiento de Perron, ya que supone que la fecha del cambio estructural es conocida. 

Zivot y Andrews (1992) muestran que si el momento de cambio estructural es erróneo y se aplica la prueba de Perron, entonces aumenta la probabilidad de que se rechace la presencia de raíz unitaria en momentos en que esta realmente existe. 

Por lo tanto, Zivot y Andrews (1992) proponen una prueba de raíz unita-ria que calcula endógenamente el momento del cambio estructural
Ante ello, plantean el siguiente procedimiento:

**Paso 1:** Calcule el estadístico $t$ para verificar la hipótesis nula $a_1=0$  (es decir, la hipótesis nula de raíz unitaria) para todos los momentos ($\tau$) y para todos los posibles tipos de cambio estructural.
Para el caso de cambio estructural en media y pendiente. $\Delta y_t= a_0 + a_1 y_{t-1} + a_2t + \mu_2 D_L + \mu_3 D_T + \displaystyle\sum_{i = 1}^{p} \beta_i \Delta y_{t-i} +  \varepsilon_t$

**Paso 2:** Escoja el estadístico $|t|$ más alto (es decir, el menos favorable a aceptar $H_0$) y compárelo con los valores críticos propuestos por Zivot y Andrews.

**Paso 3:** Si se rechaza $H_0$, se concluye que no existe raíz unitaria. En caso contrario, se debe identificar el mejor modelo de cambio estructural utilizando la prueba iterativa de Chow.

##### Aplicando la pruebas ZA en R

Para llevar a cabo la prueba ZA ofrecemos dos opciones:

**Primera Opción:** Utilice el comando **urzaTes**

``` r
urzaTest(x, model = c("intercept", "trend", "both"), lag, doplot = TRUE)
```
| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **model**               | cadena de caracteres que especifica si cambio estructural ocurre en                                                                          | 
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el mayor número de variables diferenciadas endógenas rezagadas que se incluirán en la regresión de prueba.                                   |
| **doplot**              | indicador lógico, por defecto VERDADERO. ¿Debe mostrarse un gráfico de diagnóstico?                                                          | 
|                         | **"TRUE"** para mostrar gráfico de diagnostico - **_Opción Predeterminada_**                                                                 |
|                         | **"FALSE"** para no mostrar gráfico de diagnostico                                                                                           |

**Segunda Opción:** Utilice el comando **ur.za*
``` r
ur.za(x, model = c("intercept", "trend", "both"), lag=NULL)
```

| **Argumentos**          | **Descripción**                                                                                                                              | 
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **x**                   | vector o variable de series de tiempo                                                                                                        |
| **model**               | cadena de caracteres que especifica si cambio estructural ocurre en :                                                                        |
|                         | **"intercept"**: intercepto - **_Opción Predeterminada_**                                                                                    |  
|                         | **"trend**: tendencia                                                                                                                        |
|                         | **"both"**: ambos                                                                                                                            |  
| **lag**                 | el mayor número de variables diferenciales endógenas rezagadas que se incluirán en la regresión de prueba                                    |

## Ejemplo utilizando el PIB per cápita de Colombia a precios constantes en pesos

Retomamos parte del código de R que se había utilizado previamente 
``` r
rm(list = ls())
library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)

dat = WDI(indicator= c(PIB_per_capita = "NY.GDP.PCAP.KN"), country=c('CO'), language = "es")
ggplot(dat, aes(year, PIB_per_capita)) + labs(subtitle="$", y="Pesos constantes", x="Años", title="PIB per cápita real de Colombia", caption = "Fuente: 
Construcción propia a partir de los Indicadores de Desarrollo Económico del Banco Mundial")
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/681c14d8-e78a-48b9-b6a9-9bc12aea2e84)

A partir del gráfico se puede comenzar a inferir que la variable no tiene un comportamiento estacionario. Sin embargo, hay que recolectar más evidencia al respecto, para ello primero se visualiza la función de autocorrelación de la variable #PIBpc$ utilizando el siguiente comando: 

Para el gráfico de la $FAC$ se ejecuta el comando
``` r
dat_ <- dat %>% arrange(year)
dat <- na.omit(dat_)
PIBpc_ = subset(dat, select = c(PIB_per_capita))
PIBpc <- ts(PIBpc_, start=1960)
autoplot(acf(PIBpc, plot = FALSE))
```
Obteniendose

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/ef5fe48f-b32e-448d-854f-876589b76e9e)

El decaimiento continuo pero moderado de la $FAC$ da una idea de raíz unitaria. 

A continuación se desarrollan pruebas de raíz unitaria:

### Prueba ADF
``` r
ur_trend.df <- ur.df(y=PIBpc, type = c("trend"), lags = 10, selectlags = c("AIC"))
ur_drift.df <- ur.df(PIBpc, type = c("drift"), lags = 10, selectlags = c("AIC"))
ur_none.df <- ur.df(PIBpc, type = c("none"), lags = 10, selectlags = c("AIC"))

summary(ur_trend.df)
summary(ur_drift.df)
summary(ur_none.df)
```
Obteniendose

``` r
> summary(ur_trend.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression trend 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + tt + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1456590  -140786    16594   176377   898299 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)  
(Intercept)  6.721e+05  2.968e+05   2.265   0.0283 *
z.lag.1     -1.416e-01  6.714e-02  -2.109   0.0404 *
tt           2.959e+04  1.394e+04   2.123   0.0392 *
z.diff.lag1 -1.927e-01  1.679e-01  -1.148   0.2570  
z.diff.lag2  5.828e-01  2.527e-01   2.307   0.0256 *
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 372600 on 46 degrees of freedom
Multiple R-squared:  0.155,	Adjusted R-squared:  0.08154 
F-statistic:  2.11 on 4 and 46 DF,  p-value: 0.09484

Value of test-statistic is: -2.1093 3.2214 2.2795 

Critical values for test statistics: 
      1pct  5pct 10pct
tau3 -4.04 -3.45 -3.15
phi2  6.50  4.88  4.16
phi3  8.73  6.49  5.47

> summary(ur_drift.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression drift 


Call:
lm(formula = z.diff ~ z.lag.1 + 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1607110  -137422     -202   180687   938891 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)  
(Intercept)  2.057e+05  2.068e+05   0.995   0.3249  
z.lag.1     -4.082e-03  1.821e-02  -0.224   0.8236  
z.diff.lag1 -2.266e-01  1.732e-01  -1.308   0.1973  
z.diff.lag2  4.451e-01  2.531e-01   1.758   0.0852 .
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
Residual standard error: 386200 on 47 degrees of freedom
Multiple R-squared:  0.07226,	Adjusted R-squared:  0.01304 
F-statistic:  1.22 on 3 and 47 DF,  p-value: 0.3128


Value of test-statistic is: -0.2241 2.4006 

Critical values for test statistics: 
      1pct  5pct 10pct
tau2 -3.51 -2.89 -2.58
phi1  6.70  4.71  3.86

> summary(ur_none.df)

############################################### 
# Augmented Dickey-Fuller Test Unit Root Test # 
############################################### 

Test regression none 


Call:
lm(formula = z.diff ~ z.lag.1 - 1 + z.diff.lag)

Residuals:
     Min       1Q   Median       3Q      Max 
-1700811  -129976    31119   202167   914060 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)  
z.lag.1      0.012818   0.006565   1.953   0.0567 .
z.diff.lag1 -0.202519   0.171513  -1.181   0.2435  
z.diff.lag2  0.419725   0.251809   1.667   0.1021  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 386200 on 48 degrees of freedom
Multiple R-squared:  0.2787,	Adjusted R-squared:  0.2336 
F-statistic: 6.183 on 3 and 48 DF,  p-value: 0.001222


Value of test-statistic is: 1.9526 

Critical values for test statistics: 
     1pct  5pct 10pct
tau1 -2.6 -1.95 -1.61
```

Dado que en la prueba ADF con intercepto y tendencia, ambos terminos dieron significativos, entonces se decide testear dicha prueba. Los resultados de la misma son:

| Estadistico | Valor      |  1%     |  5%     |  10%    |
|-------------|:----------:|:-------:|:-------:|:-------:|
| tau3        |  $-2.1093$ | $-4.04$ | $-3.45$ | $-3.15$ |
| phi2        |  $3.2214$  |  $6.50$ |  $4.88$ |  $4.16$ |
| phi3        |  $2.2795$  |  $8.73$ |  $6.49$ |  $5.47$ |




# Referencias

* DICKEY, David y FULLER, Wayne. _Distribution of the Estimators for Autoregressive Time Series With a Unit Root_. Journal of the American Statistical Association, Vol. 74, No. 366 (June, 1979); p. 427-431 
* DICKEY, David y  FULLER, Wayne. _Likelihood Ratio Statistics for Autoregressive Time Series with a Unit Root_. Econometrica, Vol. 49, No. 4 (July, 1981); p. 1057-1072
* ELLIOTT, Graham; ROTHENBERG, Thomas y STOCK, James. _Efficient Tests for an Autoregressive Unit Root_. Econometrica, Vol. 64, No. 4 (July, 1996); p. 813-836
* KWIATKOWSKI, Denis; PHILLIPS, Peter; SCHMIDT, Peter y SHIN, Yongcheol. _Testing the Null Hypothesis of Stationarity against the Alternative of a Unit Root: How Sure are we that Economic Time Series Have a Unit Root?_ Journal of Econometrics, Vol. 54, No. 1–3 (October-December, 1992); p 159-178
* NEWEY, Whitney y WEST, Kenneth. _A Simple, Positive Semi-Definite, Heteroskedasticity and Autocorrelation Consistent Covariance Matrix_. Econometrica, Vol. 55, No. 3 (May, 1987); p. 703-708
* NEWEY, Whitney y WEST, Kenneth. _Automatic Lag Selection in Covariance Matrix Estimation_. The Re-view of Economic Studies, Vol. 61, No. 4 (October, 1994); p. 631–653
* PERRON, Pierre. _The Great Crash, the Oil Price Shock, and the Unit Root Hypothesis_. Econometrica, Vol. 57, No. 6 (November, 1989); p. 1361-1401
* PHILLIPS, Peter. _Time Series Regression with a Unit Root_. Econometrica, Vol. 55, No. 2 (March, 1987); p. 277-301 
* PHILLIPS, Peter y PERRON, Pierre.  _Testing for a Unit Root in Time Series Regression_. Biometrika Vol. 75, No. 2 (June, 1988); p. 335-346
* ZIVOT, Eric y ANDREWS, Donald. _Further Evidence on the Great Crash, the Oil-Price Shock, and the Unit-Root Hypothesis_. Journal of Business & Economic Statistics, Vol. 10, No. 3 (July, 1992); p. 251-270





