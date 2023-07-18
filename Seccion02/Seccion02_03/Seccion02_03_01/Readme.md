## SECCIÓN 2.3.1
# Las tres etapas de la metodología de Box-Jenkins

La metodología de Box-Jenkins consta de las siguientes tres etapas:

1. **Identificación:** Se examina visualmente
   * el gráfico de la serie de tiempo,
   * la función de autocorrelación ($FAC$)  y
   * la función de autocorrelación parcial ($FACP$).

2. **Estimación:** De acuerdo a la información de los gráficos, se plantean y se estiman modelos potenciales, se examinan los diferentes coeficientes $a_i$ y $\beta_i$ y se evaluan indicadores de parsimonia.

3. **Verificación de diagnóstico:** Se garantiza que los residuos del modelo estimado imiten un proceso de ruido blanco.

A continuación se revisa más en detalle cada uno de las etapas, posteriormente se plantean algunos ejemplos simulados y finalmente se desarrolla un ejemplo utilizando la basa de datos Indicadores de Desarrollo Económico del Banco Mundial.

## 1. IDENTIFICACIÓN
La trayectoria temporal de la serie proporciona información sobre:
* valores atípicos,
* valores faltantes y
* cambios estructurales en los datos.

Así mismo, las variables no estacionarias pueden tener una tendencia pronunciada o parecer serpentear sin una media o varianza constante a largo plazo. Los valores faltantes y los valores atípicos se pueden corregir en este punto. 

La $FACP$ y la $FAC$ se utilizan para determinar los componentes $AR()$ y $MA()$ del Modelo $ARMA()$, respectivamente. Más adelante, en la sección de ejemplos, se puede ver más en detalle esta cuestión. 

## 2. ESTIMACIÓN:
De acuerdo a los gráficos de las $FACP$ y $FAC$, se estiman varios potenciales modelos para luego ser comparados.

Una idea fundamental en el enfoque de Box y Jenkins es el principio de parsimonia. La incorporación de coeficientes adicionales aumenta necesariamente el ajuste del modelo (por ejemplo, el valor del $R^2$ aumenta) pero al costo de reducir los grados de libertad. 

Box y Jenkins argumentan que los modelos parsimoniosos producen mejores pronósticos que los modelos sobreparametrizados. Un modelo parsimonioso se adapta bien a los datos sin incorporar ningún coeficiente innecesario. 

El $R^2$ y el promedio de la Suma de los Residuos al Cuadrado son medidas comunes de bondad de ajuste en estimaciones de Mínimos Cuadrados Ordinarios. El problema con estas medidas es que el ajuste necesariamente mejora a medida que se incluyen más parámetros en el modelo. 

La parsimonia sugiere usar el Criterio de Información de Akaike ($CIA$) y/o el Criterio Bayesiano de Schwartz ($CBS$) como medidas más apropiadas del ajuste general del modelo. 

##### Tenga cuidado con las estimaciones que no convergen rápidamente. La mayoría de los paquetes econométricos estiman los parámetros de un modelo $ARMA()$ utilizando un procedimiento de búsqueda no lineal. Si la búsqueda no converge rápidamente, es posible que los parámetros estimados sean inestables. En tales circunstancias, agregar una o dos observaciones adicionales puede alterar en gran medida las estimaciones.

## 3. VERIFICACIÓN DE DIAGNOSTICO: 

La práctica estándar es dibujar los residuos para buscar valores atípicos y evidencia de períodos en los que el modelo no se ajusta bien a los datos. Una práctica común es crear residuos estandarizados dividiendo cada residuo, $\varepsilon_t$ , por su desviación estándar estimada, $\sigma$. 

Si los residuos se distribuyen normalmente, el gráfico de la serie $\frac{\varepsilon_t}{\sigma}$ debe ser tal que no más del $5$% queden fuera de la banda de $-1.96$ a $1.96$. 

Si los residuos estandarizados parecen ser mucho más grandes en algunos períodos que en otros, puede ser evidencia de un cambio estructural. 

Si todos los modelos _ARMA_ plausibles muestran evidencia de un mal ajuste durante una porción razonablemente larga de la muestra, es aconsejable considerar el uso de:
* análisis de intervención,
* análisis de función de transferencia o
* cualquier otro de los métodos de estimación multivariante que veremos más adelante en el curso. 

Si la varianza de los residuos está aumentando, una transformación logarítmica puede ser apropiada. Alternativamente, es posible que se desee mo-delar cualquier tendencia de la varianza utilizando las técnicas $ARCH$ (las cuales serán objeto de otro curso) 

Es particularmente importante que los residuos de un modelo estimado no estén serialmente correlacionados. 

Cualquier evidencia de correlación serial implica un movimiento sistemático en la secuencia { $y_t$ } que no es explicado por los coeficientes $ARMA$ incluidos en el modelo. Por lo tanto, cualquiera de los modelos tentativos que producen residuos no aleatorios debería eliminarse de la consideración. 

Para verificar la correlación en los residuos, construya la $FAC$ y la $FACP$ de los residuos del modelo estimado. Luego puede usar el estadístico Q de Ljung-Box para determinar si alguna o todas las autocorrelaciones o autocorrelaciones parciales de los residuos son estadísticamente significativas.

#### ANOTACIÓN: Algunos programas econométricos informan el resultado de la prueba de Durbin-Watson como un control para la correlación serial de primer orden. Esta prueba está sesgada hacia la búsqueda de una correlación serial en presencia de variables dependientes rezagadas. Por lo tanto, generalmente no se usa en modelos $ARMA$.

Aunque no hay un nivel de significancia que se considere "más apropiado", desconfíe de cualquier modelo que arroje:

1. varias correlaciones residuales que sean marginalmente significativas y
2. un estadístico Q que apenas sea significativo al nivel del $10%$. 

En tales circunstancias, generalmente es posible formular un modelo que tenga un mejor rendimiento.

De forma similar, un modelo puede estimarse sólo sobre una parte del conjunto de datos. El modelo estimado se puede usar para pronosticar los valores conocidos de la serie. La suma de los errores de pronóstico al cuadrado es una forma útil de comparar la idoneidad de los modelos alternativos. Los modelos con pronósticos pobres fuera de la muestra deben ser eliminados. 
