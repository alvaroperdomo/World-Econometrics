## SECCIÓN 2.3.1 (T)
# Las tres etapas de la metodología de Box-Jenkins

La metodología de Box-Jenkins consta de las siguientes tres etapas:

1. **Identificación.** 
2. **Estimación.** De acuerdo a la información de los gráficos, se plantean y se estiman modelos potenciales, se examinan los diferentes coeficientes $a_i$ y $\beta_i$ y se evaluan indicadores de parsimonia.
3. **Verificación de diagnóstico.** Se garantiza que los residuos del modelo estimado imiten un proceso de ruido blanco.

A continuación se revisa más en detalle cada uno de las etapas. 

## 1. Identificación
Se examina visualmente:
* el gráfico de la serie de tiempo,
* la función de autocorrelación ($FAC$)  y
* la función de autocorrelación parcial ($FACP$).

La trayectoria temporal de la serie proporciona información sobre valores atípicos, valores faltantes y cambios estructurales en los datos. Así mismo, las variables no estacionarias pueden tener una tendencia pronunciada o parecer serpentear sin una media o varianza constante a largo plazo. Los valores faltantes y los valores atípicos se pueden corregir en este punto. 

La $FACP$ y la $FAC$ se utilizan para determinar los componentes $AR()$ y $MA()$ del Modelo $ARMA()$, respectivamente. Más adelante, en las secciones de 2.3.2 y 2.3.3, se muestran ejemplos que ayudan a entender mejor cómo opera ésto. 

## 2. Estimación
De acuerdo a los gráficos de las $FACP$ y $FAC$, se estiman varios potenciales modelos para luego ser comparados[^1].

Una idea fundamental en el enfoque de Box y Jenkins es el principio de parsimonia. Box y Jenkins argumentan que los modelos parsimoniosos producen mejores pronósticos que los modelos sobreparametrizados. Un modelo parsimonioso se adapta bien a los datos sin incorporar ningún coeficiente innecesario. El $R^2$ y el promedio de la Suma de los Residuos al Cuadrado son medidas comunes de bondad de ajuste en estimaciones de Mínimos Cuadrados Ordinarios. El problema con estas medidas es que el ajuste necesariamente mejora a medida que se incluyen más parámetros en el modelo. 

La parsimonia sugiere usar el _Criterio de Información de Akaike_ y/o el _Criterio Bayesiano de Schwartz_ como medidas más apropiadas del ajuste general del modelo. En ambos casos, el modelo más parsimonioso es el que reporta un menor valor según el criterio escogido. Hay varias formas equivalentes de calcular ambos criterios, que pueden diferir en valor. Sin embargo, lo importante es que la forma escogida mantenga el orden en la parsimonia del criterio escogido. A continuación se presenta una tabla con diferentes formas de cálculo de ambos criterios.

| Criterio de Información de Akaike   | Criterio Bayesiano de Schwatrz |
|:-----------------------------------:|:------------------------------:|
| $T \ln{SRC} + 2n$                   | $T \ln{SRC} + n \ln{T}$        |
| $2n - 2 \ln(L)$                     | $n \ln{T}  - 2 \ln(L)$         |
| $\eqalign{e^{\frac{2n}{T}}SRC}$     | $\eqalign{T^{\frac{n}{T}}SRC}$  |

donde,
* $T$ es el número de observaciones
* $SRC$ es la suma de residuos al cuadrado
* $n$ es el número de parametros estimados
* $L$ es el máximo valor de la función de verosimilitud para el modelo estimado

[^1]: **Tenga cuidado con las estimaciones que no convergen rápidamente. _R_ al igual que la mayoría de los paquetes econométricos estiman los parámetros de un modelo _ARMA()_ utilizando un procedimiento de búsqueda no lineal. Si la búsqueda no converge rápidamente, es posible que los parámetros estimados sean inestables. En tales circunstancias, agregar una o dos observaciones adicionales puede alterar en gran medida las estimaciones.**

## 3. Verificación de diagnostico.

La práctica estándar es dibujar los residuos para buscar valores atípicos y evidencia de períodos en los que el modelo no se ajusta bien a los datos. Una práctica común es crear residuos estandarizados dividiendo cada residuo, $\varepsilon_t$ , por su desviación estándar estimada, $\sigma$. Si los residuos se distribuyen normalmente, el gráfico de la serie $\frac{\varepsilon_t}{\sigma}$ debe ser tal que no más del $5$% queden fuera de la banda de $-1.96$ a $1.96$. Si los residuos estandarizados parecen ser mucho más grandes en algunos períodos que en otros, puede ser evidencia de un cambio estructural. 

Si todos los modelos _ARMA_ plausibles muestran evidencia de un mal ajuste durante una porción razonablemente larga de la muestra, es aconsejable considerar el uso de hacer un análisis multivariado de series de tiempo[2^]. Si la varianza de los residuos está aumentando, una transformación logarítmica puede ser apropiada. Alternativamente, es posible que se desee modelar cualquier tendencia de la varianza utilizando las técnicas $ARCH$ (las cuales no serán objeto de este curso).

[2^]: **Tambiém se puede considerar la opción de estimar análisis de intervención o funciones de transferencia. Sin embargo, estos temas no serán objeto de análisis en este curso**

Es particularmente importante que los residuos de un modelo estimado no estén serialmente correlacionados. Cualquier evidencia de correlación serial implica un movimiento sistemático en la secuencia { $y_t$ } que no es explicado por los coeficientes $ARMA$ incluidos en el modelo. Por lo tanto, cualquiera de los modelos tentativos que producen residuos no aleatorios debería eliminarse de la consideración. Para verificar la correlación en los residuos, construya la $FAC$ y la $FACP$ de los residuos del modelo estimado. Luego puede usar el estadístico $Q$ de Ljung-Box para determinar si alguna o todas las autocorrelaciones o autocorrelaciones parciales de los residuos son estadísticamente significativas[^2].

[^2]: Algunos programas econométricos informan el resultado de la prueba de Durbin-Watson como un control para la correlación serial de primer orden. Esta prueba está sesgada hacia la búsqueda de una correlación serial en presencia de variables dependientes rezagadas. Por lo tanto, generalmente no se usa en modelos $ARMA$.

Aunque no hay un nivel de significancia que se considere "más apropiado", desconfíe de cualquier modelo que arroje:

1. varias correlaciones de los residuos que sean marginalmente significativas y
2. un estadístico Q que apenas sea significativo al nivel del $10%$. 

En tales circunstancias, generalmente es posible formular un modelo que tenga un mejor rendimiento.

De forma similar, un modelo puede estimarse sólo sobre una parte del conjunto de datos. El modelo estimado se puede usar para pronosticar los valores conocidos de la serie. La suma de los errores de pronóstico al cuadrado es una forma útil de comparar la idoneidad de los modelos alternativos. Los modelos con pronósticos pobres fuera de la muestra deben ser eliminados. 

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Retornar: 2.3. Análisis ARMA (La Metodología de Box-Jenkins)](../Readme.md) | [:house: Inicio](../../../README.md) | [2.3.1.(R) Las tres etapas de la metodología de Box-Jenkins (Aplicación en _R_)](../Seccion02_03_01_R/Readme.md) |
|------------------------------------------------------------------------------|--------------------------------------|------------------------------------------------------------------------------------------------------------------|
