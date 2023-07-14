## SECCIÓN 2.1: 
# Series Estacionarias y No Estacionarias

## Los procesos estacionarios
Un proceso estocástico que tiene una media y una varianza finita es estacionario en covarianza si para todo $t$ y $t-s$ tiene:
1) **Media constante**: $E(y_t)=E(y_{t-s})=\mu $
2) **Varianza constante**: $E[(y_{t}- \mu)^2]=E[(y_{t-s}- \mu)^2]=\sigma_{y}^2$
3) **Autocovarianzas de la misma amplitud constantes**: $E[(y_{t}- \mu)(y_{t-s}- \mu)] = E[(y_{t-j}- \mu)(y_{t-j-s}- \mu)] = \gamma_{s}$

donde $\mu, \sigma_{y}^2$, y $\gamma_{s}$ son parametros constantes, y $E(x)$ es el valor medio teórico de $x$.

## Los procesos ruido blanco
Un caso especial de procesos estacionarios que es importante tener en consideración son los procesos ruido blanco.

Una secuencia { $\varepsilon_t$ } es un proceso de **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones de la secuencia. 

Formalmente, la secuencia { $\varepsilon_t$ } es un proceso de ruido blanco si para cada período de tiempo $t$:
1) $E(\varepsilon_t)=E(\varepsilon_{t-1})= · · · =0$
2) $E(\varepsilon_{t}^2)=E(\varepsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\varepsilon_{t}}^2=\sigma_{\varepsilon_{t-1}}^2= · · · =\sigma^2$ ]
3) $E(\varepsilon_{t}\varepsilon_{t-s})=E(\varepsilon_{t-j}\varepsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\varepsilon_{t}\varepsilon_{t-s}}=\sigma_{\varepsilon_{t-j}\varepsilon_{t-j-s}}=0$ ]

En el resto del curso, { $\varepsilon_t$ } siempre se referirá a un proceso de ruido blanco y $\sigma^2$  se referirá a la varianza de ese proceso. Cuando sea necesario hacer referencia a dos o más procesos de ruido blanco, se usarán símbolos como { $\varepsilon_{1t}$ } y { $\varepsilon_{2t}$ }. 

## Los procesos ARMA
* Un proceso autorregresivo de orden $p$, también conocido como $AR(p)$ se define así: $y_t=a_0+\displaystyle\sum_{i = 1}^{p}a_iy_{t-i}+\varepsilon_{t}$
* Un proceso de media móvil de orden $q$, también conocido como $MA(q)$ se define así: $y_t=a_0+\displaystyle\sum_{i = 1}^{q} \beta_i\varepsilon_{t-i}+\varepsilon_{t}$
* Es posible combinar un proceso de media móvil con un proceso autorregresivo para obtener un modelo autorregresivo de media móvil ($ARMA$). Por ejemplo, un modelo $ARMA(p,q)$, se escribe como: $y_t=a_0+\displaystyle\sum_{i = 1}^{p}a_iy_{t-i}+\displaystyle\sum_{i = 1}^{q} \beta_i\varepsilon_{t-i}+\varepsilon_{t}$
    
Observe que si $q=0$, obtenemos un proceso $AR(p)$. Y si $p=0$, obtenemos un proceso $MA(q)$. 

## Requisito principal para poder aplicar la metodología de Box y Jenkins en el análisis univariado de series de tiempo
Según la metodología de Box y Jenkins, el análisis univariado de series de tiempo se hace a partir de la construcción de modelos $ARMA$ de series estacionarias. **¿Por qué es importante el supuesto de estacionariedad en el análisis?** A modo de ejemplo, en el Anexo 1 se explica qué implicaciones tiene la estacionariedad de una serie sobre su representación $AR(1)$

## ¿Qué hacer si una serie no es estacionaria?

Apliquele la transformación adecuada. El procedimiento más común es sacandole diferencias a la serie original, en particular:

* Si una serie { $y_t$ } es estacionaria, se dice que es integrada de orden $0$ y el modelo de serie que la identifica se le suele denotar como $ARMA(p,q)$ (o como $ARIMA(p,0,q)$ ) donde $p$ y $q$ son los componentes autorregresivo y de media móvil del modelo, respectivamente.
* Si una serie { $y_t$ } no es estacionaria, pero su primera diferencia $\Delta y_t = y_t-y_{t-1}$ si es estacionaria, se dice que es integrada de orden $1$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,1,q)$
* Si una serie { $y_t$ } y su primera diferencia $\Delta y_t$ no son estacionarias, pero su segunda diferencia $\Delta_2 y_t= \Delta y_t - \Delta y_{t-1}$ si es estacionaria, se dice que es integrada de orden $2$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,2,q)$
* En consecuencia, en un modelo $ARIMA(p,i,q)$ la $I$ significa el número de veces que hubo que diferenciar la serie { $y_t$ } para obtener una serie estacionaria.

En la gran mayoría de las variables económicas, la serie { $y_t$ } es estacionaria en niveles o en primeras diferencias, en algunos casos es estacionaria en segundas diferencias. Sin embargo, si después de la segunda diferencia una serie no resulta estacionaria, entonces se puede transformar la serie como la estimación de un polinomio. En el Anexo 2 se explica cómo funciona esta transformación polinómica.  

## ANEXO 1: ¿Bajo qué condiciones un modelo $AR(1)$ es estacionario? 

Sea $y_t=a_0+a_1y_{t-1}+\varepsilon_t$ donde $\varepsilon_t$ es ruido blanco.

Suponga que el proceso comenzó en el período $0$, de modo que $y_0$ es una condición inicial determinista. Entonces, 
* $y_1=a_0+a_1y_0+\varepsilon_1$
* $y_2=a_0+a_1y_1+\varepsilon_2=a_0+a_1[a_0+a_1y_0+\varepsilon_1]+\varepsilon_2=a_0(1+a_1)+a_1^2y_0+a_1\varepsilon_1+\varepsilon_2$ donde en la segunda igualdad se reemplaza $y_1$.
* $y_3=a_0+a_1y_2+\varepsilon_3=a_0+a_1[a_0(1+a_1)+a_1^2y_0+a_1\varepsilon_1+\varepsilon_2]+\varepsilon_3=a_0(1+a_1+a_1^2)+a_1^3y_0+a_1^2\varepsilon_1+a_1\varepsilon_2+\varepsilon_3$ donde en la segunda igualdad se reemplaza $y_2$

Si se sigue este mismo proceso iterativo, entonces la solución a esta ecuación en los periodos $t$ y $t+s$ es: 

* $y_t=a_0\displaystyle\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0+\displaystyle\sum_{i = 0}^{t-1}\varepsilon_{t-i}$
* $y_{t+s}=a_0\displaystyle\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0+\displaystyle\sum_{i = 0}^{t+s-1}\varepsilon_{t+s-i}$

**Por lo tanto, esta ecuación es estacionaria si** 
1. **$t$ es grande [^1] y**
2. **$|a_1|<1$** 

[^1]: **En consecuencia, si una muestra es generada por un proceso que ha comenzado recientemente, las realizaciones pueden no ser estacionarias. Empíricamente, en los análisis econométicos de series de tiempo anuales, es deseable tener al menos 30 grados de libertad en las estimaciones (los grados de libertad son iguales al número de años que tiene la serie menos el número de parámetros que se estiman).** 

La prueba de esto es:

El valor esperado de la misma es $$Ey_t=a_0\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0$$ y $$Ey_{t+s}=a_0\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0$$ para todo $s$

(1) El valor medio de $y_t$ es finito e independiente del tiempo: $$Ey_t=Ey_{t+s}=\frac{a_0}{1-a_1}=\mu$$ para todo $s$

(2) La varianza de $y_t$ es finita e independiente del tiempo: $$E(y_t-\mu)^2=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)^2=\sigma^2(1+a_1+a_1^2+a_1^4+ …)^2=\frac{\sigma^2}{1-a_1^2}$$ 

(3) Las autovarianzas de $y_t$ son finitas e independientes del tiempo: $$E(y_t-\mu)(y_{t-s}-\mu)=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)(\varepsilon_{t-s}+a_1\varepsilon_{t-s-1}+a_1^2\varepsilon_{t-s-2}+a_1^4\varepsilon_{t-s-4}+ …)=\sigma^2a_1^s(1+a_1+a_1^2+a_1^4+ …)=\frac{\sigma^2a_1^s}{1-a_1^2}$$ 

## ANEXO 2: La transformación polinomica 
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




