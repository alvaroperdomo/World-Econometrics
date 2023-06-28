Para llevar a cabo un análisis univariado de series de tiempo siguiendo la metodología de Box y Jenkins (1976). Se necesita que la variable a analizar sea estacionaria. 

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

##### Observe que un proceso ruido blanco, es un caso particular de un proceso estacionario.  

En el resto del curso, { $\varepsilon_t$ } siempre se referirá a un proceso de ruido blanco y $\sigma^2$  se referirá a la varianza de ese proceso. Cuando sea necesario hacer referencia a dos o más procesos de ruido blanco, se usarán símbolos como { $\varepsilon_{1t}$ } { $\varepsilon_{2t}$ }. 

## Los procesos ARMA
Es posible combinar un proceso de media móvil con un proceso autorregresivo para obtener un modelo autorregresivo de media móvil (_ARMA_). Por ejemplo, un modelo _ARMA(p,q)_, se escribe como:

$$y_t=a_0+\sum_{i = 1}^{p}a_iy_{t-i}+\sum_{i = 0}^{q} \beta_i\varepsilon_{t-i}$$
    
* Seguimos la convención de normalizar unidades para que siempre $\beta_0=1$. 
* Si las raíces características de esta ecuación están todas en el círculo unitario, { $y_t$ } se llama un modelo $ARMA(p,q)$. 
* Si $q=0$, obtenemos un proceso autorregresivo de orden $p$: $AR(p)$. 
* Si $p=0$, obtenemos un proceso de media móvil de orden $q$: $MA(q)$. 

##### En un modelo $ARMA$, es perfectamente posible que $p$ y/o $q$ sean infinitos. 

## Restricciones de estacionariedad para un modelo $AR(1)$

Sea $y_t=a_0+a_1y_{t-1}+\varepsilon_t$ donde $\varepsilon_t$ es ruido blanco.

Suponga que el proceso comenzó en el período $0$, de modo que $y_0$ es una condición inicial determinista. 

La solución a esta ecuación es 

* $$y_t=a_0\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0+\sum_{i = 0}^{t-1}\varepsilon_{t-i}$$
* $$y_{t+s}=a_0\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0+\sum_{i = 0}^{t+s-1}\varepsilon_{t+s-i}$$

**Por lo tanto, esta ecuación es estacionaria si** 
1. **$t$ es grande (por lo tanto, si una muestra es generada por un proceso que ha comen-zado recientemente, las realizaciones pueden no ser estacionarias) y**
2. **$|a_1|<1$** 

La prueba de esto es:

El valor esperado de la misma es $$Ey_t=a_0\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0$$ y $$Ey_{t+s}=a_0\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0$$ para todo $s$

(1) El valor medio de $y_t$ es finito e independiente del tiempo: $$Ey_t=Ey_{t+s}=\frac{a_0}{1-a_1}=\mu$$ para todo $s$

(2) La varianza de $y_t$ es finita e independiente del tiempo: $$E(y_t-\mu)^2=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)^2=\sigma^2(1+a_1+a_1^2+a_1^4+ …)^2=\frac{\sigma^2}{1-a_1^2}$$ 

(3) Las autovarianzas de $y_t$ son finitas e independientes del tiempo: $$E(y_t-\mu)(y_{t-s}-\mu)=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)(\varepsilon_{t-s}+a_1\varepsilon_{t-s-1}+a_1^2\varepsilon_{t-s-2}+a_1^4\varepsilon_{t-s-4}+ …)=\sigma^2a_1^s(1+a_1+a_1^2+a_1^4+ …)=\frac{\sigma^2a_1^s}{1-a_1^2}$$ 

## ¿Qué hacer si una serie no es estacionaria?

Apliquele la transformación adecuada. 

1. Saquele la primera diferencia a la serie original:

    * Si una serie $y_t$ es estacionaria, se dice que es integrada de orden $0$ y el modelo de serie que la identifica se le suele denotar como $ARMA(p,q)$ (o como $ARIMA(p,0,q)$) donde $p$ y $q$ son los componentes autorregresivo y de media móvil del modelo, respectivamente. 
    * Si una serie $y_t$ no es estacionaria, pero su primera diferencia $\Delta y_t = y_t-y_{t-1}$ si es estacionaria, se dice que es integrada de orden $1$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,1,q)$
    * Si una serie $y_t$ y su primera diferencia $\Delta y_t$ no son estacionarias, pero su segunda diferencia $\Delta_2 y_t= \Delta y_t - \Delta y_{t-1}$ si es estacionaria, se dice que es integrada de orden $2$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,2,q)$

## Referencias
BOX, George y JENKINS, Gwilym. _Time Series Analysis: Forecasting and Control_. (1975); 575 p.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>







