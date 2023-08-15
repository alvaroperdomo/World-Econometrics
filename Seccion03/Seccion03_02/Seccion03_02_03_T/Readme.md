<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.3:
# La Metodología de Johansen

La metodología de Johansen se resume en cuatro pasos:
1) Realice una prueba de todas las variables para evaluar el de integración del modelo
2) Estime el modelo y determine el número de vectores de cointegración
3) Analice el(los) vector(es) de cointegración normalizados y la velocidad de los coeficientes de ajuste.
4) Desarrolle las pruebas de impulso-respuesta, descomposición de varianza y causalidad en el modelo de corrección de errores

El paso (1) ya se analizo en la sección 2.2 por lo cual no se ahondara mucho en esta sección. Los pasos (2) y (3) si seran objeto de análisis de esta sección. El paso (4) es equivalente al análisis que se hizo para el modelo VAR en la sección, por lo cual tampoco se ahondara mucho en esta sección. A continuación, se abordara los pasos (2) y (3) de la metodología de Johansen a partir de la respuesta a algunas preguntas importantes y al final de la sección se hara un resumen de los cuatro pasos considerados.  
   
## ¿En qué supera la metodología de Johansen a la metodología de Engle y Granger?
Los estimadores de máxima verosimilitud de Johansen (1988): 
* evitan el uso de estimadores de dos pasos,
* pueden estimar y probar la presencia de múltiples vectores de cointegración, y
* permiten probar versiones restringidas de los vectores de cointegración y la velocidad de los parámetros de ajuste.[^1] 

[^1]: **A menudo, es interesante determinar si es posible verificar una teoría probando restricciones en las magnitudes de los coeficientes estimados**

## ¿Cuáles son las bases de la metodología de Johansen para definir el número de vectores de cointegración de un modelo multivariado de series de tiempo?
El procedimiento de Johansen (1988) se basa en gran medida en la relación entre el rango de una matriz y sus raíces características. Este no es más que una generalización multivariada de la prueba $DF$ utilizada para analizar la presencia de raíz unitaria en los modelos univariados. 

En el caso univariado, es posible ver que la estacionariedad de { $y_t$ } depende de $a_1$; es decir, dados $y_t=a_1y_{t-1}+\varepsilon_t$ o $\Delta y_t=(a_1-1)y_{t-1}+\varepsilon_t$:
* Si $(a_1-1)=0$, el proceso { $y_t$ } tiene una raíz unitaria.
* Descartando el caso en el que { $y_t$ } es explosivo, si $(a_1-1)≠0$ podemos concluir que la secuencia { $y_t$ } es estacionaria.

Las tablas de Dickey-Fuller proporcionan los estadísticos apropiados para probar formalmente la hipótesis nula $(a_1-1)=0$.

Consideremos la generalización al caso multivariado con $n$ variables; asuma que el vector $\mathbf{x_t}$ de $n$ variables, se comporta como $\mathbf{x_t=A_1x_{t-1}+\varepsilon_t}$  así que $\mathbf{\Delta x_t=A_1x_{t-1}-x_{t-1}+\varepsilon_t=(A_1-I)x_{t-1}+\varepsilon_t=\pi x_{t-1}+\varepsilon_t}$ donde 
* $\mathbf{\varepsilon_t}$ es un vector ( $n\times 1$ ),
* $\mathbf{A_1}$ es una matriz ( $n\times n$ ) de parámetros, 
* $\mathbf{I}$ es una matriz identidad ( $n\times n$ ),
* $\mathbf{\pi}$ es una matriz ( $n\times n$ ) definida como $\mathbf{(A_1-I)}$.

El rango de $\mathbf{\pi}$ es igual al número de vectores de cointegración. En este punto vale la pena descartar dos casos especiales:

1) Por analogía con el caso univariado, si $\mathbf{\pi}$ tiene solo ceros, de modo que el **$rango(\pi)=0$**, entonces a pesar de que todas las $n$ secuencias { $x_{it}$ } tengan raíz unitaria, no hay una combinación lineal de las mismas que sea estacionaria. En otras palabras, **las variables no se cointegran**.
2) Descartando la presencia de raíces características mayores que $1$ y con un **$rango(\pi)=n$**, se obtiene que $\mathbf{\Delta x_t=\pi x_{t-1} + \varepsilon_t}$ es un sistema convergente de ecuaciones en diferencias, de modo que todas **las variables son estacionarias**.

## ¿Qué criterio utilizar para incluir el intercepto en el modelo multivariado de series de tiempo?
Hay varias formas de generalizar $\mathbf{\Delta x_t=\pi x_{t-1}+\varepsilon_t}$. Por ejemplo, la ecuación se modifica fácilmente si se considera la presencia de un intercepto; simplemente asuma $\mathbf{\Delta x_t=A_0 + \pi x_{t-1}+\varepsilon_t}$ donde $\mathbf{A_0}={\left\lbrack \matrix{ a_{10} \cr a_{20} \cr \dots \cr a_{n0} } \right\rbrack}$. El efecto de considerar los diversos $a_{i0}$ es que permite la posibilidad de incluir una tendencia lineal en el proceso de generación de datos. Lo ideal es incluir el intercepto si las variables exhiben una tendencia que claramente  aumenta o a disminuye. Aquí, el rango de $\pi$ se puede ver como el número de relaciones de cointegración existentes en los datos sin tendencia. En el largo plazo, $\mathbf{\pi x_{t-1}=0}$ tal que el valor esperado de cada secuencia { $\Delta x_{it}$ } es $a_{i0}$. Al agregar todos estos cambios en el tiempo se obtiene la expresión determinista $a_{i0}t$.

Las tres figuras de abajo ilustran los efectos de incluir un intercepto en el proceso de generación de datos[^2]. 

[^2]: **Al final de esta subsección se encuentra el código en _R_ utilizado para elaborar los gráficos de esta subsección**

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/61f77162-6c4b-48bd-962b-b30739b38ba5)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/9056163b-4170-4c92-aead-86bd1ccafe00)
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/9f86e779-3f92-4119-b664-f597eeda05cf)

En las figuras, se generan dos secuencias aleatorias, { $\varepsilon_{yt}$ } y { $\varepsilon_{zt}$ }, con $100$ observaciones cada una. Por otro lado, se asume $y_0=z_0=0$, y que los siguientes $100$ valores de las secuencias { $y_t$ } y { $z_t$ } son $\eqalign{ \left\lbrack \matrix{\Delta y_t \cr \Delta z_t} \right\rbrack = \left\lbrack \matrix{-0.2 & 0.2 \cr 0.2 & -0.2} \right\rbrack} \left\lbrack \matrix{y_{t-1} \cr z_{t-1}} \right\rbrack + \left\lbrack \matrix{\varepsilon_{yt} \cr \varepsilon_{zt}} \right\rbrack$ de modo que la relación de cointegración es $y_t=z_t$

* En la primera figura, puede ver que cada secuencia se asemeja a un proceso aleatorio y que ninguno se aleja demasiado del otro.
* En la figura del centro se agregan interceptos de manera que $a_{10}=a_{20}=0.1$; ahora cada serie tiende a aumentar $0.1$ unidades en cada período. Además del hecho de que cada secuencia comparte la misma tendencia estocástica, tenga en cuenta que cada una también tiene la misma tendencia de tiempo determinista. El hecho de que cada una tenga la misma tendencia determinista no es el resultado de la equivalencia entre $a_{10}$ y $a_{20}$; ya que $y_t$ y $z_t$ están cointegradas, la solución general a $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ requiere que cada uno tenga la misma tendencia lineal.
* Para verificar esto, la última figura establece $a_{10}=0.1$ y $a_{20}=0.4$. De nuevo, las secuencias tienen las mismas tendencias estocásticas y deterministas.

## El término constante en los vectores de cointegración
A continuación veremos que manipulando apropiadamente los elementos de $A_0$ es posible incluir una constante en los vectores de cointegración sin necesidad de incluir una tendencia de tiempo determinista al sistema.

Una forma de incluir una constante en las relaciones de cointegración es restringir los valores de los diversos $a_{i0}$. Por ejemplo, si $rango(\pi)=1$, las filas de $\pi$ pueden diferir sólo en un escalar, por lo que es posible escribir cada secuencia { $\Delta x_{it}$ } en $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)}) + a_{10} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)}) + a_{20} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)}) + a_{n0} + \varepsilon_{nt}$

donde $s_i$ es un escalar tal que $s_i \pi_{1j}= \pi_{ij}$  

Si $a_{i0}$ se puede restringir tal que $a_{i0}=s_ia_{10}$, todas las secuencias { $\Delta x_{it}$ } se pueden escribir con la constante incluida en el vector de cointegración:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + a_{10}) + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + a_{20}) + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + a_{20}) + \varepsilon_{nt}$

o en la forma compacta $\mathbf{\Delta x_t= A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ donde 

$\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$, 
$\eqalign{\mathbf{x_{t-1}^*} = {\left\lbrack \matrix{x_{1(t-1)} \cr x_{2(t-1)} \cr \dots \cr x_{n(t-1)} } \right\rbrack}}$,     y 

$\eqalign{\mathbf{ \pi* } = {\left\lbrack \matrix{\pi_{11} & \pi_{12} & \dots & \pi_{1n} \cr \pi_{21} & \pi_{22} & \dots & \pi_{2n} \cr \dots & \dots & \dots & \dots \cr \pi_{n1} & \pi_{n2} & \dots & \pi_{nn} } \right\rbrack}}$ 

La característica interesante de $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ es que la tendencia lineal se elimina del sistema. En esencia, los diversos $a_{i0}$ se han modificado de tal manera que la solución general para cada { $x_{it}$ } no contiene una tendencia temporal. La solución al conjunto de ecuaciones en diferencias representadas por $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$ es tal que se espera que todos los $\Delta x_{it}$ sean iguales a cero cuando $\pi_{11} x_{1(t-1)} + \pi_{12} x_{2(t-1)} + \dots + \pi_{1n} x_{n(t-1)} + a_{i0} =0$.

Para resaltar la diferencia entre $\mathbf{\Delta x_t=A_0+ \pi x_{t-1}+\varepsilon_t}$ y $\mathbf{\Delta x_t=A_0 + \pi^* x_{t-1}^* + \varepsilon_t}$, la figura de abajo ilustra las consecuencias de utilizar $a_{10}=0.1$ y $a_{20}=-0.1$.  

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/9a5403c8-ef5f-4723-80df-15e71bcc3cbe)

Se puede ver que ninguna secuencia contiene una tendencia determinista. De hecho, para los datos que se muestran en la figura, la tendencia desaparecerá siempre que seleccionemos valores de los interceptos manteniendo la relación $a_{10}=-a_{20}$.

**Algunos econometristas prefieren incluir un intercepto en el vector de cointegración junto con un intercepto fuera de éste.** Esto tiene sentido si las variables contienen un intercepto y si la teoría económica sugiere que el vector de cointegración contiene un intercepto. Sin embargo, debe quedar claro que el intercepto en el vector de cointegración no se identifica en presencia de un intercepto fuera de este. Después de todo, alguna parte del intercepto no restringido siempre puede incluirse en el vector de cointegración. 

En términos del ejemplo anterior, el sistema siempre puede escribirse como:

$\Delta x_{1t}= (\pi_{11}x_{1(t-1)} + \pi_{12}x_{2(t-1)} + \dots + \pi_{1n}x_{1(t-1)} + b_{10}) + b_{11} + \varepsilon_{1t}$

$\Delta x_{2t}=s_2(\pi_{21}x_{2(t-1)} + \pi_{22}x_{2(t-1)} + \dots + \pi_{2n}x_{2(t-1)} + b_{20}) + b_{21} + \varepsilon_{2t}$

$\dots$

$\Delta x_{nt}=s_n(\pi_{n1}x_{n(t-1)} + \pi_{n2}x_{n(t-1)} + \dots + \pi_{nn}x_{n(t-1)} + b_{n0}) + b_{n1} + \varepsilon_{nt}$

donde  $s_i b_{10} + b_{11}= a_{10}$  

Todo lo que se hace es dividir $a_{10}$ en dos partes y colocar una parte dentro de la relación de cointegración. Es necesaria alguna estrategia de identificación ya que la proporción del intercepto a incluir en el vector de cointegración es arbitraria. 

De los gráficos previos, note que es necesario una constante fuera de la relación de cointegración para capturar los efectos de una tendencia sostenida de las variables a aumentar (o a disminuir). La mayoría de los investigadores incluyen interceptos si los datos se asemejan a los gráficos donde ambos interceptos eran 0.1 o donde los interceptos eran 0.1 y 0.4. De lo contrario, incluyen interceptos en el vector de cointegración o excluyen por completo a los regresores deterministas. 

**Si no está seguro, puede usar los métodos que se describen más adelante para probar si las desviaciones se pueden restringir adecuadamente**. R permite incluir una tendencia de tiempo determinista en el modelo. Claro que es mejor evitar el uso de la tendencia como variable explicativa a menos que tenga una buena razón para incluirla en el modelo.[^3] 

Hasta el momento se analizado el ejenmplo en donde el modelo sólo tiene un proceso autorregresivo de orden $1$, ¿qué se puede decir acerca de los procesos autorregresivos de orden superior? 

[^3]: **Johansen (1994) discute el papel de los regresores deterministas en una relación de cointegración.**

## Componentes autorregresivos de orden superior

Al igual que con la prueba $ADF$, el modelo multivariado también puede generalizarse para permitir un proceso autorregresivo de orden superior. 

Considere $x_t = A_1x_{t-1} + A_2x_{t-2}  + \dots +  A_px_{t-p} + \varepsilon_t$ donde $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$ y $\mathbf{\varepsilon_t}$ es un vector n-dimensional distribuido de forma independiente e idéntica con media cero y matriz de varianza $\Sigma_\varepsilon$.

Como se hizo con la prueba $ADF$ en el modelo univariado, con las sumas apropiadas, la ecuación $\mathbf{x_t= \displaystyle\sum_{i=1}^{p} A_i x_{t-i} + \varepsilon_t}$ se puede transformar en: $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ donde $\mathbf{\pi = \displaystyle\sum_{i=1}^p A_i-I}$ y $\mathbf{\pi_i =- \displaystyle\sum_{j=i+1}^p A_j}$ 

La característica clave a tener en esta nueva ecuación es el rango de la matriz $\mathbf{\pi}$ ; **el rango de $\mathbf{\pi}$ es igual al número de vectores cointegrantes independientes**:

* si $\mathbf{rango(\pi)=0}$, la matriz es nula y $\mathbf{\Delta x_t= \pi x_{t-1} +  \displaystyle\sum_{i=1}^{p-1} \pi_i \Delta x_{t-i} + \varepsilon_t}$ es el modelo $VAR$ en las primeras diferencias.
* si $\mathbf{rango(\pi)=n}$, todas las variables dentro del $VAR$ son estacionarias.
* si $\mathbf{rango(\pi)=1}$, hay un solo vector de cointegración y la expresión $\mathbf{\pi x_{t-1}}$ es el término de corrección de errores.
* si $\mathbf{1 \lt rango(\pi) \lt n}$, hay múltiples vectores de cointegración.

El número de distintos vectores de cointegración se puede obtener al verificar la significancia de las raíces características de $\mathbf{\pi}$. 
Sabemos que el rango de una matriz es igual al número de sus raíces características que difieren de cero. Suponga que se obtiene la matriz $\mathbf{\pi}$ y se ordenan las $n$ raíces características de forma que $\lambda_1>\lambda_2>\dots>\lambda_n$. 

Si las variables en $\mathbf{x_t}$ no están cointegradas, $\mathbf{rango(\pi)=0}$ y todas las raíces características serán iguales a cero. Como $\ln{(1)}=0$, entonces cada una de las expresiones $\ln{(1 - \lambda_i)}$ será igual a cero si las variables no están cointegradas. 

De manera similar, si $\mathbf{rango(\pi)=1}$, $0 < 𝜆_1 < 1$, entonces $\ln{(1 - \lambda_1)} < 0$ y $\ln{(1 - \lambda_2)} = \ln{(1 - \lambda_3)} = \dots = \ln{(1 - \lambda_n)} = 0$.

En la práctica, solo podemos obtener estimaciones de $\mathbf{\pi}$ y de sus raíces características. La prueba para el número de raíces características que son significativamente diferentes de $1$ se puede realizar utilizando los siguientes dos estadísticos de prueba propuestos por Johansen (1988):

* $\lambda_{traza}(r)=-T\displaystyle\sum_{r+1}^n \ln{(1-\hat{\lambda_i})}$ 
* $\lambda_{max}(r,r+1)=-T\ln{(1-\hat{\lambda_{r+1}})}$

donde
* $\hat{\lambda_i}$ son los valores estimados de las raíces características (también llamados valores propios) obtenido de la matriz $\mathbf{\pi}$ estimada 
* $T$ es el número de observaciones utilizables

El estadístico $\lambda_{traza}$ prueba la hipótesis nula de que el número de diferentes vectores de cointegración es menor o igual que $r$ frente a una alternativa general. Note que $\lambda_{traza}=0$ cuando todos los $\lambda_i=0$. Cuanto más lejos están las raíces características estimadas de cero, más negativo es $(1-\hat{\lambda_i})$  y más grande es el estadístico $\lambda_{traza}$. 

El estadístico $\lambda_{max}$ prueba la hipótesis nula de que el número de vectores de cointegración es $r$ frente a la alternativa de $r+1$ vectores de cointegración. Si el valor estimado de la raíz característica está cerca de cero, $\lambda_{max}$ será pequeño.

Los valores críticos de los estadísticos $\lambda_{traza}$  y $\lambda_{max}$ se obtienen utilizando el enfoque de Monte Carlo. La distribución de estos estadísticos depende de dos cosas:
1) El número de componentes no estacionarios bajo la hipótesis nula (es decir, $n-r$).
2) La forma del vector $A_0$. Es decir: 
    * si no incluye interceptos ni en la ecuación ni en el vector de cointegración.
    * si incluye un intercepto en la ecuación.
    * si incluye un intercepto en el vector de cointegración.

Es importante anotar que estos estadísticos necesitan que sus residuos sean ruido blanco. Cualquier evidencia de que los errores no son ruido blanco generalmente significa que las longitudes de rezago son demasiado cortas. 

## Resumen de la metodología propuesta por Johansen

Utilice los siguientes cuatro pasos cuando implemente el procedimiento Johansen:

1) **Realice una prueba de todas las variables para evaluar su orden de integración.**

   Grafique los datos para ver si es probable que una tendencia de tiempo lineal esté presente en el proceso de generación de datos. Tenga cuidado porque los resultados de la prueba pueden ser bastante sensibles a la longitud del rezago. En la siguiente sección se profundiza más acerca de las pruebas para determinar el número óptimo de rezagos.

2) **Estime el modelo y determine el número de vectores de cointegración (es decir, determien el rango de $\pi$.)**

   R contiene una rutina para estimar el modelo. Aquí, basta con decir que Mínimos Cuadrados Ordinarios no es apropiado porque es necesario imponer restricciones de ecuaciones cruzadas en la matriz. En la mayoría de los casos, puede elegir estimar el modelo en tres formas:
   
   i) con todos los elementos de $\mathbf{A_0}$ iguales a cero,
   
   ii) con una constante en la ecuación, o
   
   iii) con un término constante en el vector de cointegración. 

3) **Analice el(los) vector(es) de cointegración normalizados y la velocidad de los coeficientes de ajuste.**

   Por ejemplo, asuma que si seleccionamos $r=1$, el vector estimado de cointegración $(\beta_0,\beta_1,\beta_2,\beta_3)$ es $\mathbf{\beta_t}=(0.00553, 0.41532, 0.42988, −0.42207)$. Normalizando con respecto a $\beta_1$, el vector de cointegración normalizado es $\mathbf{\beta_t}=(−0.01331, −1.0000, −1.0350, 1.0162)$.

   Asuma que los valores teóricos del vector de cointegración son $(0,−1,−1, 1)$. En consecuencia, considere hacer las siguientes pruebas:

   a) La prueba de $\beta_0=0$ implica una restricción en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con un grado de libertad.

   b) Para restringir el vector de cointegración normalizado tal que $\beta_2=-1$ y $\beta_3=-1$ implica dos restricciones en un vector de cointegración; por lo tanto, la prueba de razón de verosimilitud tiene una distribución $\chi^2$ con dos grados de libertad.

   c) Para probar la restricción conjunta $\mathbf{\beta_t}=(0,−1,−1, 1)$ implica las tres restricciones $\beta_0=0$, $\beta_2=-1$ y $\beta_3=-1$.

5) Finalmente, **las pruebas de impulso-respuesta, descomposición de varianza y causalidad en el modelo de corrección de errores** podrían ayudar a si el modelo estimado parece ser razonable.

## Anexo: Código en $R$ de los gráficos de esta subsección

``` r
library(tseries)

# Establecer la semilla para la generación de números aleatorios (para reproducibilidad)
set.seed(1224)

# Generar secuencias aleatorias de ruido
n <- 100
epsilon_yt <- rnorm(n)
epsilon_zt <- rnorm(n)

# Inicializar y0 y z0
y <- numeric(n)
z <- numeric(n)

# Matriz de coeficientes
A <- matrix(c(-0.2, 0.2, 0.2, -0.2), nrow = 2, byrow = TRUE)

# Generar series y[t] y z[t] sin interceptos
for (t in 2:n) {
  y[t] <- -0.2 * y[t-1] + 0.2 * z[t-1] + epsilon_yt[t] + y[t-1]
  z[t] <- 0.2 * y[t-1] - 0.2 * z[t-1] + epsilon_zt[t] + z[t-1]
}

# Crear un data frame con las series generadas
data <- data.frame(y = y[-1], z = z[-1])

# Graficar las series generadas
plot(data$y, type = "l", col = "blue", ylab = "Valores", xlab = "Tiempo", main = "Serie sin Interceptos")
lines(data$z, col = "red")
legend("topright", legend = c("y_t", "z_t"), col = c("blue", "red"), lty = 1)

# Generar series y[t] y z[t] con interceptos de 0.1
for (t in 2:n) {
  y[t] <- 0.1 - 0.2 * y[t-1] + 0.2 * z[t-1] + epsilon_yt[t] + y[t-1]
  z[t] <- 0.1 + 0.2 * y[t-1] - 0.2 * z[t-1] + epsilon_zt[t] + z[t-1]
}

# Crear un data frame con las series generadas
data <- data.frame(y = y[-1], z = z[-1])

# Graficar las series generadas
plot(data$y, type = "l", col = "blue", ylab = "Valores", xlab = "Tiempo", main = "Serie con Interceptos de 0.1")
lines(data$z, col = "red")
legend("topright", legend = c("y_t", "z_t"), col = c("blue", "red"), lty = 1)

# Generar series y[t] y z[t] con interceptos de 0.4
for (t in 2:n) {
  y[t] <- 0.4 - 0.2 * y[t-1] + 0.2 * z[t-1] + epsilon_yt[t] + y[t-1]
  z[t] <- 0.4 + 0.2 * y[t-1] - 0.2 * z[t-1] + epsilon_zt[t] + z[t-1]
}

# Crear un data frame con las series generadas
data <- data.frame(y = y[-1], z = z[-1])

# Graficar las series generadas
plot(data$y, type = "l", col = "blue", ylab = "Valores", xlab = "Tiempo", main = "Serie con Interceptos de 0.4")
lines(data$z, col = "red")
legend("topright", legend = c("y_t", "z_t"), col = c("blue", "red"), lty = 1)

# Generar series y[t] y z[t] con interceptos de 0.1 y -0.1
for (t in 2:n) {
  y[t] <- 0.1 - 0.2 * y[t-1] + 0.2 * z[t-1] + epsilon_yt[t] + y[t-1]
  z[t] <- -0.1 + 0.2 * y[t-1] - 0.2 * z[t-1] + epsilon_zt[t] + z[t-1]
}

# Crear un data frame con las series generadas
data <- data.frame(y = y[-1], z = z[-1])

# Graficar las series generadas
plot(data$y, type = "l", col = "blue", ylab = "Valores", xlab = "Tiempo", main = "Serie con Interceptos de 0.1 y -0.1")
lines(data$z, col = "red")
legend("topright", legend = c("y_t", "z_t"), col = c("blue", "red"), lty = 1)


```
---
---
# Preguntas de selección múltiple

1. **¿Cuál de las siguientes razones no es una ventaja de la metodología de Johansen sobre la metodología de Engle y Granger?:**
 
   a) Se evita el uso de estimadores de dos pasos.

   b) Se pueden estimar relaciones de cointegración entre variables integradas de ordenes diferentes.

   c) Se pueden estimar y probar la presencia de múltiples vectores de cointegración.

   d) Se permiten probar versiones restringidas de los vectores de cointegración y la velocidad de los parámetros de ajuste.


2. **¿Al utilizar la prueba de cointegración de Johansen, cuándo se debe estimar un $VAR$ en diferencias?:**
 
   a) Si $\mathbf{rango(\pi)=0}$.

   b) Si $\mathbf{rango(\pi)=n}$.

   c) Si $\mathbf{rango(\pi)=1}$.

   d) Si $\mathbf{1 \lt rango(\pi) \lt n}$.

---
---
| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |[Subsección: 3.2.3.(R)  Aplicación en _R_ de la Metodología de Johansen (2 o más variables)](../Seccion03_02_03_R/Readme.md) |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
