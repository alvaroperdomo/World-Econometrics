Para llevar a cabo un análisis univariado de series de tiempo siguiendo la metodología de Box y Jenkins (). Se necesita que la variable a analizar sea estacionaria. 

## Los procesos estacionarios
Un proceso estocástico que tiene una media y una varianza finita es estacionario en covarianza si para todo $t$ y $t-s$,
1) Media constante: $E(y_t)=E(y_{t-s})=\mu $
2) Varianza constante: $E[(y_{t}- \mu)^2]=E[(y_{t-s}- \mu)^2]=\sigma_{y}^2$
3) Autocovarianzas de la misma amplitud constantes: $E[(y_{t}- \mu)(y_{t-s}- \mu)] = E[(y_{t-j}- \mu)(y_{t-j-s}- \mu)] = \gamma_{s}$

donde $\mu, \sigma_{y}^2$, y $\gamma_{s}$ son todas constantes.

Un caso especial de un proceso estacionario que es importante tener en consideración son los procesos ruido blanco.

## Los procesos ruido blanco
Una secuencia { $\varepsilon_t$ } es un proceso de **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones. 

Formalmente, si $E(x)$ es el valor medio teórico de $x$, entonces la secuencia { $\epsilon_t$ } es un proceso de ruido blanco si para cada período de tiempo $t$:
* $E(\varepsilon_t)=E(\varepsilon_{t-1})= · · · =0$
* $E(\varepsilon_{t}^2)=E(\varepsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\varepsilon_{t}}^2=\sigma_{\varepsilon_{t-1}}^2= · · · =\sigma^2$ ]
* $E(\varepsilon_{t}\varepsilon_{t-s})=E(\varepsilon_{t-j}\varepsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\varepsilon_{t}\varepsilon_{t-s}}=\sigma_{\varepsilon_{t-j}\varepsilon_{t-j-s}}=0$ ]




