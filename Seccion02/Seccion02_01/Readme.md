## SECCIÓN 2.1: 
# Series Estacionarias y No Estacionarias

## Los procesos estacionarios
Dado un proceso estocástico de una variable $y_t$ que tiene una media y una varianza finita, se afirma que es estacionario en covarianza si para todo $t$ y $t-s$ donde $s=0,1,2,\dots$, se cumple:
1) **Media constante**: $E(y_t)=E(y_{t-s})=\mu $
2) **Varianza constante**: $E[(y_{t}- \mu)^2]=E[(y_{t-s}- \mu)^2]=\sigma_{y}^2$
3) **Autocovarianzas de la misma amplitud constantes**: $E[(y_{t}- \mu)(y_{t-s}- \mu)] = E[(y_{t-j}- \mu)(y_{t-j-s}- \mu)] = \gamma_{s}$

donde $\mu, \sigma_{y}^2$, y $\gamma_{s}$ son parametros constantes, y $E(x)$ es el valor esperado (o valor medio teórico) de $x$.

## Los procesos ruido blanco
Un caso especial de procesos estacionarios que es importante tener en consideración son los procesos ruido blanco.

Una secuencia { $\varepsilon_t$ } es un proceso **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones de la secuencia. 

Formalmente, la secuencia { $\varepsilon_t$ } es un proceso ruido blanco si para todo $t$ y $t-s$ donde $s=0,1,2,\dots$, se cumple:
1) $E(\varepsilon_t)=E(\varepsilon_{t-1})= · · · =0$
2) $E(\varepsilon_{t}^2)=E(\varepsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\varepsilon_{t}}^2=\sigma_{\varepsilon_{t-1}}^2= · · · =\sigma^2$ ]
3) $E(\varepsilon_{t}\varepsilon_{t-s})=E(\varepsilon_{t-j}\varepsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\varepsilon_{t}\varepsilon_{t-s}}=\sigma_{\varepsilon_{t-j}\varepsilon_{t-j-s}}=0$ ]

En el resto del curso, { $\varepsilon_t$ } siempre se referirá a un proceso de ruido blanco y $\sigma^2$  se referirá a la varianza de ese proceso. Cuando sea necesario hacer referencia a dos o más procesos que son ruido blanco, se usarán símbolos como { $\varepsilon_{1t}$ } y { $\varepsilon_{2t}$ }. 

## Los procesos ARMA
Dados unos parametros $a_i$ y $\beta_i$ donde $i$ es un número natural:

* Un proceso autorregresivo de orden $p$, también conocido como $AR(p)$ se define así: $y_t=a_0+\displaystyle\sum_{i = 1}^{p}a_iy_{t-i}+\varepsilon_{t}$.
* Un proceso de media móvil de orden $q$, también conocido como $MA(q)$ se define así: $y_t=a_0+\displaystyle\sum_{i = 1}^{q} \beta_i\varepsilon_{t-i}+\varepsilon_{t}$
* Es posible combinar un proceso de media móvil con un proceso autorregresivo para obtener un modelo autorregresivo de media móvil ($ARMA$). Por ejemplo, un modelo $ARMA(p,q)$, se escribe como: $y_t=a_0+\displaystyle\sum_{i = 1}^{p}a_iy_{t-i}+\displaystyle\sum_{i = 1}^{q} \beta_i\varepsilon_{t-i}+\varepsilon_{t}$
    
Observe que si $q=0$, obtenemos un proceso $AR(p)$. Y si $p=0$, obtenemos un proceso $MA(q)$. 

## Requisito principal para poder aplicar la metodología de Box y Jenkins en el análisis univariado de series de tiempo
Para el análisis univariado, se va a seguir la metodología de Box y Jenkins (1975). Según esta metodología, el análisis univariado de series de tiempo se hace a partir de la construcción de modelos $ARMA$ de series estacionarias. A modo de ejemplo, en el Anexo 1 se explica qué requisitos debe cumplir un proceso $AR(1)$ para ser estacionario. 

La explicación de los requisitos para obtener procesos estacionarios en representaciones $ARMA(p,q)$ es más complejas que para un proceso AR(1). En general, note que un modelo $ARMA(p,q)$ es la representación de una ecuación en diferencias. Por lo tanto, para que esta ecuación no sea explosiva se requiere que todos los valores caracteristicos (o también llamados los valores propios) que resuelven esta ecuación en diferencias sean menores que el valor absoluto de $1$ (usualmente, a este requisito se le denomina _estar dentro del circulo unitario_).[^1]  

[^1]: En el caso de un modelo _AR(1)_ el valor caracteristico que resuelve a la ecuación es coincidente con el coeficiente que multiplica al valor rezagado de la variable analizada. Por lo tanto, en el Anexo 1, vamos a aprovechar esta caracteristica para ver cómo se muestra la estacionariedad en este caso.

En series de tiempo, una forma de ver si una variable cumple el requisito de estacionariedad es a través de las pruebas de raíz unitaria. En la sección 2.2 vamos a explicar teóricamente cómo funcionan este tipo de pruebas y cómo se aplican en $R$.

## ¿Qué hacer si una serie no es estacionaria?

Apliquele la transformación adecuada. El procedimiento más común es sacandole diferencias a la serie original, en particular:

* Si una serie { $y_t$ } es estacionaria, se dice que es integrada de orden $0$ y el modelo de serie que la identifica se le suele denotar como $ARMA(p,q)$ (o como $ARIMA(p,0,q)$ ) donde $p$ y $q$ son los componentes autorregresivo y de media móvil del modelo, respectivamente.
* Si una serie { $y_t$ } no es estacionaria, pero su primera diferencia $\Delta y_t = y_t-y_{t-1}$ si es estacionaria, se dice que es integrada de orden $1$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,1,q)$
* Si una serie { $y_t$ } y su primera diferencia $\Delta y_t$ no son estacionarias, pero su segunda diferencia $\Delta_2 y_t= \Delta y_t - \Delta y_{t-1}$ si es estacionaria, se dice que es integrada de orden $2$ y el modelo de serie que la identifica se le suele denotar como $ARIMA(p,2,q)$
* En consecuencia, en un modelo $ARIMA(p,i,q)$ la $I$ significa el número de veces que hubo que diferenciar la serie { $y_t$ } para obtener una serie estacionaria. A la $I$ también se le llama el orden de integración de la serie.

En la gran mayoría de las variables económicas, la serie { $y_t$ } es estacionaria en niveles o en primeras diferencias, en algunos casos es estacionaria en segundas diferencias. Sin embargo, si después de la segunda diferencia una serie no resulta estacionaria, entonces se puede plantear la opción de transformar la serie como la estimación de un polinomio. En el Anexo 2 se explica cómo funciona esta transformación polinómica.  

## ANEXO 1: ¿Bajo qué condiciones un modelo $AR(1)$ es estacionario? 

Sea $y_t=a_0+a_1y_{t-1}+\varepsilon_t$ donde $\varepsilon_t$ es ruido blanco.

Suponga que el proceso comenzó en el período $0$, de modo que $y_0$ es una condición inicial determinista. Entonces, 
* $y_1=a_0+a_1y_0+\varepsilon_1$
* $y_2=a_0+a_1y_1+\varepsilon_2=a_0+a_1[a_0+a_1y_0+\varepsilon_1]+\varepsilon_2=a_0(1+a_1)+a_1^2y_0+a_1\varepsilon_1+\varepsilon_2$ donde en la segunda igualdad se reemplaza $y_1$.
* $y_3=a_0+a_1y_2+\varepsilon_3=a_0+a_1[a_0(1+a_1)+a_1^2y_0+a_1\varepsilon_1+\varepsilon_2]+\varepsilon_3=a_0(1+a_1+a_1^2)+a_1^3y_0+a_1^2\varepsilon_1+a_1\varepsilon_2+\varepsilon_3$ donde en la segunda igualdad se reemplaza $y_2$

Si se sigue este mismo proceso iterativo, entonces la solución a esta ecuación en los periodos $t$ y $t+s$ es: 

* $y_t=a_0\displaystyle\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0+\displaystyle\sum_{i = 0}^{t-1}\varepsilon_{t-i}$
* $y_{t+s}=a_0\displaystyle\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0+\displaystyle\sum_{i = 0}^{t+s-1}\varepsilon_{t+s-i}$

**Por lo tanto, estas ecuaciones (obtenidas de un proceso $AR(1)$ son estacionarias si** 
1. **$t$ es grande [^2] y**
2. **$|a_1|<1$** 

[^2]: **En consecuencia, si una muestra es generada por un proceso que ha comenzado recientemente, las realizaciones pueden no ser estacionarias. Empíricamente, en los análisis econométicos de series de tiempo anuales, es deseable tener al menos 30 grados de libertad en las estimaciones (los grados de libertad son iguales al número de años que tiene la serie menos el número de parámetros que se estiman).** 

La prueba de esto es bastante sencilla; sin embargo, primero noten los siguientes dos puntos:

* El valor esperado de la serie { $y_t$ } es $Ey_t=a_0\displaystyle\sum_{i = 0}^{t-1}a_1^i+a_1^ty_0$ y $Ey_{t+s}=a_0\displaystyle\sum_{i = 0}^{t+s-1}a_1^i+a_1^{t+s}y_0$ para todo $s$.
* Dados $y_t$, $y_{t+s}$, $Ey_t$ y $Ey_{t+s}$ entonces $y_t-Ey_t=\displaystyle\sum_{i = 0}^{t-1}\varepsilon_{t-i}$ y $y_{t+s}-Ey_{t+s}=\displaystyle\sum_{i = 0}^{t+s-1}\varepsilon_{t+s-i}$

Por lo tanto, sólo si $t \to \infty$ y $|a_1|<1$, la serie { $y_t$ }se vuelve estacionaria porque:

(1) El valor medio de $y_t$ es finito e independiente del tiempo: $Ey_t=Ey_{t+s}=\frac{a_0}{1-a_1}=\mu$ para todo $s$

(2) La varianza de $y_t$ es finita e independiente del tiempo: $E(y_t-\mu)^2=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)^2=\sigma^2(1+a_1+a_1^2+a_1^4+ …)^2=\frac{\sigma^2}{1-a_1^2}$ 

(3) Las autovarianzas de $y_t$ son finitas e independientes del tiempo: $E(y_t-\mu)(y_{t-s}-\mu)=E(\varepsilon_t+a_1\varepsilon_{t-1}+a_1^2\varepsilon_{t-2}+a_1^4\varepsilon_{t-4}+ …)(\varepsilon_{t-s}+a_1\varepsilon_{t-s-1}+a_1^2\varepsilon_{t-s-2}+a_1^4\varepsilon_{t-s-4}+ …)=\sigma^2a_1^s(1+a_1+a_1^2+a_1^4+ …)=\frac{\sigma^2a_1^s}{1-a_1^2}$ 

## ANEXO 2: La transformación polinomica 
En términos generales, una serie de tiempo puede tener la tendencia polinomial $y_t=a_0+a_1t+a_2t^2+a_3t^3+...+a_nt^n+e_t$ donde { $e_t$ } es un proceso estacionario. La transformación polinomica se logra estimando { $y_t$ }, utilizando Mínimos Cuadrados Ordinarios, con respecto a una tendencia de tiempo polinomial determinista. 

Al restar los valores estimados de la secuencia { $y_t$ } de los valores reales se obtiene una estimación de la secuencia estacionaria { $e_t$ }. Ya con esta serie estacionaria se puede proceder a hacerle un análisis $ARMA$ utilizando la metodología de Box y Jenkins.

### ¿Cuál es el grado apropiado del polinomio que se estima? Criterios: 

* La práctica común es estimar la ecuación utilizando el mayor valor de $n$ que es considerado razonable. Si el estadístico $t$ indica que $a_n$ es cero, considere una tendencia polinomial de orden $n-1$. Continúe reduciendo el orden de la tendencia polinomial hasta encontrar un coeficiente distinto de cero.
* Igualmente, las pruebas $F$ también se pueden usar para determinar si un grupo de coeficientes, por ejemplo, de $a_{n-i}$  a $a_n$, es estadísticamente diferente de cero.
* Por último, los Criterios de Información de Akaike y el Criterio Bayesiano de Schwartz se pueden usar para reconfirmar el grado apropiado del polinomio. 


<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Anterior Sección: 2. Análisis Univariado de Series de Tiempo](../Readme.md) | [:house: Inicio](../../README.md) |[Siguiente Sección: 2.2. Pruebas de Raíz Unitaria](../Seccion02_02/Readme.md) |
|-------------------------------------------------------------------------------------------------------------|--------------------------------|-------------------------------------------------------------------------------------------|



