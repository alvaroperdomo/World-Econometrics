# Las series estacionarias

Para llevar a cabo un análisis univariado de series de tiempo siguiendo la metodología de Box y Jenkins (1976). Se necesita que la variable a analizar sea estacionaria. 

## Los procesos estacionarios
Un proceso estocástico que tiene una media y una varianza finita es estacionario en covarianza si para todo $t$ y $t-s$ se tiene:
1) **Media constante**: $E(y_t)=E(y_{t-s})=\mu $
2) **Varianza constante**: $E[(y_{t}- \mu)^2]=E[(y_{t-s}- \mu)^2]=\sigma_{y}^2$
3) **Autocovarianzas de la misma amplitud constantes**: $E[(y_{t}- \mu)(y_{t-s}- \mu)] = E[(y_{t-j}- \mu)(y_{t-j-s}- \mu)] = \gamma_{s}$

donde $\mu, \sigma_{y}^2$, y $\gamma_{s}$ son parametros constantes.

## Los procesos ruido blanco
Un caso especial de un proceso estacionario que es importante tener en consideración son los procesos ruido blanco.

Una secuencia { $\varepsilon_t$ } es un proceso de **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones. 

Formalmente, si $E(x)$ es el valor medio teórico de $x$, entonces la secuencia { $\epsilon_t$ } es un proceso de ruido blanco si para cada período de tiempo $t$:
1) $E(\varepsilon_t)=E(\varepsilon_{t-1})= · · · =0$
2) $E(\varepsilon_{t}^2)=E(\varepsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\varepsilon_{t}}^2=\sigma_{\varepsilon_{t-1}}^2= · · · =\sigma^2$ ]
3) $E(\varepsilon_{t}\varepsilon_{t-s})=E(\varepsilon_{t-j}\varepsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\varepsilon_{t}\varepsilon_{t-s}}=\sigma_{\varepsilon_{t-j}\varepsilon_{t-j-s}}=0$ ]

En el resto del curso, { $\varepsilon_t$ } siempre se referirá a un proceso de ruido blanco y $\sigma^2$  se referirá a la varianza de ese proceso. Cuando sea necesario hacer referencia a dos o más procesos de ruido blanco, se usarán símbolos como { $\varepsilon_{1t}$ } { $\varepsilon_{2t}$ }. 

## Los procesos ARMA
Es posible combinar un proceso de media móvil con un proceso autorregresivo para obtener un modelo autorregresivo de media móvil (_ARMA_). Por ejemplo, un modelo _ARMA(p,q)_, se escribe como:

$$y_t=a_0+\sum_{i = 1}^{p}a_iy_{t-i}+\sum_{i = 0}^{q} \beta_i\varepsilon_{t-i}$$
    
* Seguimos la convención usual de normalizar unidades para que siempre $\beta_0=1$. 
* Si $q=0$, obtenemos un proceso autorregresivo de orden $p$: $AR(p)$. 
* Si $p=0$, obtenemos un proceso de media móvil de orden $q$: $MA(q)$. 

##### En un modelo $ARMA$, es perfectamente posible que $p$ y/o $q$ sean infinitos. 

Según la metodología de Box y Jenkins, el análisis univariado de series de tiempo se hace a partir de la construcción de modelos $ARMA$ de series estacionarias.

En el Anexo 1 se explica, a modo de ejemplo, qué implicaciones tiene la estacionariedad de una serie sobre su representación $AR(1)$

## ¿Qué hacer si una serie no es estacionaria?

Apliquele la transformación adecuada. 

El procedimiento más común es sacandole la primera diferencia a la serie original, en particular:

* Si una serie $y_t$ es estacionaria, se dice que es integrada de orden $0$ y el modelo de serie que la identifica se le suele denotar como $ARMA(p,q)$ (o como $ARIMA(p,0,q)$) donde $p$ y $q$ son los componentes autorregresivo y de media móvil del modelo, respectivamente.
* Si una serie $y_t$ no es estacionaria, pero su primera diferencia $\Delta y_t = y_t-y_{t-1}$ si es estacionaria, se dice que es integrada de orden $1$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,1,q)$
* Si una serie $y_t$ y su primera diferencia $\Delta y_t$ no son estacionarias, pero su segunda diferencia $\Delta_2 y_t= \Delta y_t - \Delta y_{t-1}$ si es estacionaria, se dice que es integrada de orden $2$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,2,q)$

Si la serie $y_t$ no es estacionacionaria ni en niveles, ni al sacarle la primera y segunda diferencia, entonces es mejor manejar la serie como un polinomio. En el Anexo 2 se explica cómo funciona esta transformación polinomica.  

## ANEXO 1: ¿Bajo qué condiciones un modelo $AR(1) es estacionario? 

Sea $y_t=a_0+a_1y_{t-1}+\varepsilon_t$ donde $\varepsilon_t$ es ruido blanco.

Suponga que el proceso comenzó en el período $0$, de modo que $y_0$ es una condición inicial determinista. 

La solución a esta ecuación es 

* $$y_t=a_0\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0+\sum_{i = 0}^{t-1}\varepsilon_{t-i}$$
* $$y_{t+s}=a_0\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0+\sum_{i = 0}^{t+s-1}\varepsilon_{t+s-i}$$

**Por lo tanto, esta ecuación es estacionaria si** 
1. **$t$ es grande (por lo tanto, si una muestra es generada por un proceso que ha comenzado recientemente, las realizaciones pueden no ser estacionarias) y**
2. **$|a_1|<1$** 

La prueba de esto es:

El valor esperado de la misma es $$Ey_t=a_0\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0$$ y $$Ey_{t+s}=a_0\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0$$ para todo $s$

(1) El valor medio de $y_t$ es finito e independiente del tiempo: $$Ey_t=Ey_{t+s}=\frac{a_0}{1-a_1}=\mu$$ para todo $s$

(2) La varianza de $y_t$ es finita e independiente del tiempo: $$E(y_t-\mu)^2=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)^2=\sigma^2(1+a_1+a_1^2+a_1^4+ …)^2=\frac{\sigma^2}{1-a_1^2}$$ 

(3) Las autovarianzas de $y_t$ son finitas e independientes del tiempo: $$E(y_t-\mu)(y_{t-s}-\mu)=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)(\varepsilon_{t-s}+a_1\varepsilon_{t-s-1}+a_1^2\varepsilon_{t-s-2}+a_1^4\varepsilon_{t-s-4}+ …)=\sigma^2a_1^s(1+a_1+a_1^2+a_1^4+ …)=\frac{\sigma^2a_1^s}{1-a_1^2}$$ 

## ANEXO 2: ¿XXX? 
A veces se puede usar la diferenciación para transformar un modelo no estacionario en un modelo estacionario con una representación 𝐴𝑅𝑀𝐴. Esto no significa que todos los modelos no estacionarios puedan transformarse en modelos $ARMA$ de buen comportamiento mediante la diferenciación apropiada. 

Considere, por ejemplo, un modelo que es la suma de una tendencia determinista y un componente de ruido puro: $y_t=y_0+a_1t+\varepsilon_t$

Note que la primera diferencia de $y_t$ no se comporta bien porque $\Delta y_t=a_1+\varepsilon_t-+\varepsilon_{t-1}$

En este caso, $\Delta y_t$ no es invertible en el sentido de que $\Delta y_t$ no puede expresarse como un proceso autorregresivo. La invertibilidad de un proceso estacionario requiere que el componente 𝑀𝐴 no tenga una raíz unitaria.

En cambio, una forma adecuada de transformar este modelo es estimar la ecuación $y_t=a_0+a_1t+\varepsilon_t$. Al restar los valores estimados de $y_t$ de las series observadas se obtienen los valores estimados de la serie { $\varepsilon_t$ }. 

En términos generales, una serie de tiempo puede tener la tendencia polinomial $y_t=a_0+a_1t+a_2t^2+a_3t^3+...+a_nt^n+e_t$ donde { $e_t$ }  es un proceso estacionario. El **detrending** se logra estimando { $y_t$ } con respecto a una tendencia de tiempo polinomial determinista. 

### ¿Cuál es el grado apropiado del polinomio? Criterios: 

* La práctica común es estimar la ecuación utilizando el mayor valor de $n$ que es considerado razonable. Si el estadístico $t$ indica que $a_n$ es cero, considere una tendencia polinomial de orden $n-1$. Continúe reduciendo el orden de la tendencia polinomial hasta encontrar un coeficiente distinto de cero.
* Las pruebas $F$ se pueden usar para determinar si un grupo de coeficientes, por ejemplo, de $a_{n-i}$  a $a_n$, es estadísticamente diferente de cero.
* Los Criterios de Información de Akaike y el Criterio Bayesiano de Schwartz se pueden usar para reconfirmar el grado apropiado del polinomio. 

Al restar los valores estimados de la secuencia { $y_t$ } de los valores reales se obtiene una estimación de la secuencia estacionaria { $e_t$ }. El proceso de **detrending** puede luego modelarse utilizando métodos tradicionales (como la estimación 𝐴𝑅𝑀𝐴).



<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>







