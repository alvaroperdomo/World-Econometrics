<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SECCIÓN 2.3.2
# Estimación de un $ARMA$ - Ejemplos simulados 

Una comparación de la función de autocorrelación $FAC$ y la función de autocorrelación parcial $FACP$ muestrales con las de varios procesos $ARMA(p,q)$ teóricos puede sugerir varios modelos plausibles. Por medio de dos ejemplos sencillos, vamos a mostrar cómo se identifica el proceso $ARMA(p,q)$ generador de una variable.

## Estimación de un modelo AR(1)
En este ejemplo se generaron $200$ números aleatorios $\varepsilon_t$ distribuidos normalmente con una varianza teórica igual $1$. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=0.7y_{t-1}+\varepsilon_t$ y la condición inicial $y_0=0$. Note que la serie { $y_t$ } que se construye es estacionaria, por lo que es factible aplicar la metodología de Box-Jenkins. 

Para ello se utilizo el siguiente código de $R$:

```r
rm(list = ls())

library(stats) # Esta librería permite generar números aleatorios con distribución normal

set.seed(150) # Se establece una semilla para la generación de números aleatorios (puedes usar cualquier número entero)

n <- 200 # Se establece la longitud de la secuencia

epsilon <- rnorm(n, mean = 0, sd = 1) # Se crea un vector para almacenar los valores simulados de epsilon(t)

y <- numeric(n) # Se crea un vector para almacenar los valores simulados de y(t)

y[1] <- 0 # Se establece y(0) = 0

# Se establece la fórmula para generar los valores de y(t)
for (t in 2:n) {
  y[t] <- 0.7 * y[t-1] + epsilon[t]
}

plot(1:n, y, type = "l", xlab = "t", ylab = "y(t)", col = "blue") # Gráfico de y(t)

y <- ts(y) # Con el comando ts() se identifica a la variable "y" como una serie de tiempo

```
El gráfico de $y_t$ esta representado por:

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/99b3d3f2-b1b7-4edc-9471-8a9705f00387)

A continuación se van a desarrollar cada uno de los pasos de la metodología Box-Jenkins explicados en la sección 2.3.1. 

### Identificación
Las figuras de abajo muestran la $FAC$ en  y la $FACP$ muestrales de { $y_t$ } (en el eje vertical se muestran, respectivamente, el valor de las autocorrelaciones totales y parciales de la muestra para cada uno de los rezagos que se presentan en el eje horizontal). 

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/02f95011-4d78-4ab5-87ff-81624747a451)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/caad4385-a45e-4496-8ff7-fbdefc2b8db7)


Continuando con el código de $R$ de esta sección, los gráficos de la $FAC$ y la $FACP$ de $y_t$ se generaron con los comandos

```r
acf_plot <- autoplot(acf(y, plot = FALSE)) # Se calcula la función de autocorrelación y se genera el gráfico sin mostrarlo
acf_plot + labs(x = "Rezagos", y = "FAC") # Se personalizan las etiquetas de los ejes

pacf_plot <- autoplot(pacf(y, plot = FALSE)) # Se calcula la función de autocorrelación parcial y se genera el gráfico sin mostrarlo
pacf_plot + labs(x = "Rezagos", y = "FACP") # Se personalizan las etiquetas de los ejes
```

En la práctica, nunca conocemos el verdadero proceso de generación de datos. Suponga que se nos presentan los $200$ valores muestrales de la simulación que estamos manejando en esta sección y se nos pide descubrir el verdadero proceso $ARMA(p,q)$ utilizando la metodología de Box-Jenkins. 

El primer paso podría ser comparar la $FAC$ y la $FACP$ muestrales de $y_t$ con las de los diversos modelos teóricos. 

#### La parte autorregresiva ($AR$) del proceso generador de datos
La $FACP$ se utiliza para determinar el componente $AR(p)$ del $ARMA(p,q)$. Identifique los rezagos de la $FACP$ de $y_t$ que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{200}}=0.14$ o inferiores a $-0.14$ [^1]. Note que el pico más grande, por mucho, corresponde al rezago $1$ y vale $0.68$. esto sugiere un potencial compomemte $AR(1)$ dentro del modelo $ARMA(p,q)$. Todas las otras $FACP$ (excepto la del rezago $9$ que es aproximadamente $0.17$) son muy pequeñas.

[^1]: **En ocasiones los gráficos de la _FAC_ y  la _FACP_ incluyen el rezago _0_ , el cuál por definición siempre debe ser igual a $1$.** 

Para conocer el valor númerico exacto de la $FACP$ de $y_t$ en los rezagos $1$ y $8$ se utilizaron los comandos:

```r
pacf_result <- pacf(y, plot = FALSE) # Se calcula la función de autocorrelación parcial

pacf_lag_1 <- pacf_result$acf[1]   # Valor de la autocorrelación parcial para el rezago 1
print(pacf_lag_1)

pacf_lag_8 <- pacf_result$acf[8]   # Valor de la autocorrelación parcial para el rezago 8
print(pacf_lag_8)
```
Obteniendose:
```r
> pacf_lag_1 <- pacf_result$acf[1]   # Valor de la autocorrelación parcial para el rezago 1
> print(pacf_lag_1)
[1] 0.6784294
> pacf_lag_8 <- pacf_result$acf[8]   # Valor de la autocorrelación parcial para el rezago 8
> print(pacf_lag_8)
[1] 0.1651488
```
#### La parte de media móvil ($MA$) del proceso generador de datos
La $FAC$ se utiliza para determinar el componente $MA(q)$ del $ARMA(p,q)$. Identifique los rezagos de la $FAC$, del rezago $1$ en adelante, que estan aislados y que son superiores a $\frac{2}{\sqrt{T}}=\frac{2}{\sqrt{200}}=0.14$ o inferiores a $-0.14$. Lo que se observa en la $FAC$ es un decaimieto progresivo (por ejemplo, los primeros tres rezagos de la $FAC$ son $0.68$, $0.45$ y $0.27$). Por lo tanto, parece que no se genera un proceso $MA(q)$ dentro de nuestro $ARMA(p,q)$. 

Para saber el valor de la $FAC$ de $y_t$ en los rezagos $1$, $2$ y $3$ se utilizaron los comandos:

```r
acf_result <- acf(y, plot = FALSE) # Se calcula la función de autocorrelación total

acf_lag_1 <- acf_result$acf[2]   # Valor de la autocorrelación total para el rezago 1
print(acf_lag_1)

acf_lag_2 <- acf_result$acf[3]   # Valor de la autocorrelación total para el rezago 2
print(acf_lag_2)

acf_lag_3 <- acf_result$acf[4]   # Valor de la autocorrelación total para el rezago3
print(acf_lag_3)
```
Obteniendose:
```r
> acf_result <- acf(y, plot = FALSE) # Se calcula la función de autocorrelación total
> 
> acf_lag_1 <- acf_result$acf[2]   # Valor de la autocorrelación total para el rezago 1
> print(acf_lag_1)
[1] 0.6784294
> 
> acf_lag_2 <- acf_result$acf[3]   # Valor de la autocorrelación total para el rezago 2
> print(acf_lag_2)
[1] 0.4482265
> 
> acf_lag_3 <- acf_result$acf[4]   # Valor de la autocorrelación total para el rezago3
> print(acf_lag_3)
[1] 0.2704874
```

### Estimación y verificación de diagnóstico
Aunque sabemos que los datos se generaron en realidad a partir de un proceso de $AR(1)$, es ilustrativo comparar las estimaciones de dos modelos diferentes. Suponga que estimamos un modelo $AR(1)$ y un modelo $ARMA(1,1)$. Por lo tanto, consideramos los dos modelos tentativos:

* **Modelo 1 - $AR(1)$:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 2 - $ARMA(1,1)$:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_{1}\varepsilon_{t1} $

La estimación en $R$ de ambos modelos, de una vez haciendo la verificación de diagnóstico, se hace con los siguientes comandos:

```r
Modelo_1<- Arima(y, order=c(1,0,0))  # Se calcula la función AR(1) sin mostrarla
Modelo_2<- Arima(y, order=c(1,0,))  # Se calcula la función MA(1) sin mostrarla. 

summary(Modelo_1) # Se reportan los resultados del Modelo_1
summary(Modelo_2) # Se reportan los resultados del Modelo_2

AIC(Modelo_1,Modelo_2) # Se reportan los Criterios de Información de Akaike para todos los modelos considerados
BIC(Modelo_1,Modelo_2) # Se reportan los Criterios Bayesianos de Schwartz para todos los modelos considerados

ggtsdiag(Modelo_1, gof.lag = 30) +  labs(subtitle = "Modelo 1") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 1.
ggtsdiag(Modelo_2, gof.lag = 30) +  labs(subtitle = "Modelo 2") # Con este comando se pueden hacer pruebas sobre los residuos del Modelo 2.
```
Obteniendose 

```r
> summary(Modelo_1)   # Se reportan los resultados del Modelo_1
Series: y 
ARIMA(1,0,0) with non-zero mean 

Coefficients:
         ar1    mean
      0.6752  0.1289
s.e.  0.0516  0.2077

sigma^2 = 0.9379:  log likelihood = -276.68
AIC=559.36   AICc=559.48   BIC=569.25

Training set error measures:
                      ME      RMSE       MAE  MPE MAPE      MASE      ACF1
Training set 0.000604357 0.9636119 0.7719439 -Inf  Inf 0.9465626 0.0182812
> summary(Modelo_2) # Se reportan los resultados del Modelo_2
Series: y 
ARIMA(0,0,1) with non-zero mean 

Coefficients:
         ma1    mean
      0.5384  0.1278
s.e.  0.0455  0.1153

sigma^2 = 1.139:  log likelihood = -295.96
AIC=597.91   AICc=598.04   BIC=607.81

Training set error measures:
                       ME     RMSE       MAE  MPE MAPE     MASE      ACF1
Training set 0.0003588097 1.061824 0.8465603 -Inf  Inf 1.038058 0.2193961
> 
> AIC(Modelo_1,Modelo_2) # Se reportan los Criterios de Información de Akaike para todos los modelos considerados
         df      AIC
Modelo_1  3 559.3574
Modelo_2  3 597.9132
> BIC(Modelo_1,Modelo_2) # Se reportan los Criterios Bayesianos de Schwartz para todos los modelos considerados
         df      BIC
Modelo_1  3 569.2524
Modelo_2  3 607.8082
```
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/4134f837-f314-482a-99b3-9e260a18084c)

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/b06f9ec4-94ba-431c-9bc3-7b5046620ec4)


La tabla de abajo y los dos gráficos de arriba resumen los resultados de las dos estimaciones

| Indicadores                               | Modelo 1                | Modelo 2                | 
|-------------------------------------------|:-----------------------:|:-----------------------:|
| $\hat{a}_1$  <br> (Error estándar)        |$0.6752$ <br> ($0.0516)$ |                         | 
| $\hat{\beta}_{1}$  <br> (Error estándar)  |                         |$0.5384$ <br> ($0.0455$) | 
| Criterio de Información de Akaike         |$559.3574$               |$597.9132$               | 
| Criterio Bayesianode Schwartz             |$569.2524$               |$607.8082$               | 

**Análicemos el Modelo 1**

El coeficiente del **Modelo 1** satisface la condición de estabilidad $|\hat{a}_1| < 0$ y tiene un error estándar bajo (es decir, el coeficente estimado es menor a dos desviaciones estandar del valor de $|\hat{a}_1|$).

Como una verificación de diagnóstico útil, arriba en el gráfico de la $FAC$ de los residuos estimados del Modelo 1. Note que las barras de las autocorrelaciones de todos los rezagos superiores a $0$ son inferiores a $0.14$. Es decir, los residuos estimados parecen ser ruido blanco. Confirmando lo anterior, en el gráfico de la prueba de Ljung-Box sobre los residuos estimados del Modelo 1 note que los p-values son superiores al 5%. Esto es  una fuerte evidencia de que el modelo $AR(1)$ se ajusta bien con los datos. Después de todo, si las autocorrelaciones de los residuos fueran significativas, el modelo $AR(1)$ no usaría toda la información disponible sobre los movimientos en la serie { $y_t$ }

**Análicemos el Modelo 2**

Al examinar los resultados para el **Modelo 2**, observe que la estimación del $\hat{\beta}_1$ es de biuena calidad (es decir, el coeficiente estimado es menor a dos desviaciones estandar del valor de $|\hat{\beta}_1|$). Sin embargo, el Modelo 2 tiene problemas de autocorrelación en los residuos por lo que no serían ruido blanco. Los problemas de autocorrelación se evidencian en:
* El gráfico de la $FAC$ de sus residuos estimados. Note que las barras de los rezagos $1$ y $2$ son muy superiores a $0.14$.
* El  gráfico de la prueba de Ljung-Box sobre los residuos estimados del Modelo 1, note que los p-values son inferiores al 5%. Es decir, se rechaza la hipótesis nula de que los residuos de la estimación no estan corelacionados.

Con los siguientes comandos puede ver, para los residuos del Modelo 2, los resultados detallados de la prueba de Lung-Box cuando se hace para los rezagos $1$ a $8$, $1$ a $16$ y $1$ a $24$:

```r
Box.test(Modelo_1$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_1$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_1$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box

Box.test(Modelo_2$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_2$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box
Box.test(Modelo_2$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box
```

Obteniendose 
```r
> Box.test(Modelo_2$residuals, lag=8, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_2$residuals
X-squared = 56.172, df = 8, p-value = 2.611e-09

> Box.test(Modelo_2$residuals, lag=16, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_2$residuals
X-squared = 62.731, df = 16, p-value = 1.803e-07

> Box.test(Modelo_2$residuals, lag=24, type="Ljung-Box") # Test de Ljung-Box

	Box-Ljung test

data:  Modelo_2$residuals
X-squared = 64.588, df = 24, p-value = 1.398e-05
```
Por último, note que en la comparación de los valores del _Criterio de Información de Akaike_ y del _Criterio Bayesiano de Schwartz_ de los dos modelos sugiere que es mucho mejor el **Modelo 1** al ser comparado con el **Modelo 2**. Por lo tanto, apunta a la elección del **Modelo 1**.

## Estimación de un modelo $ARMA(1,1)$

Utilizemos un segundo ejemplo para ver cómo la $FAC$ y la $FACP$ muestrales sirven para identificar un modelo $ARMA(1,1)$. En este ejemplo se generaron $100$ números aleatorios $\varepsilon_t$ distribuidos normalmente. Comenzando con $t=1$, los valores de $y_t$ se generaron usando la fórmula $y_{t-1}=-0.7y_{t-1}+\varepsilon_t+0.7\varepsilon_{t-1}$ y la condición inicial $y_0=0$ y $\varepsilon_0=0$. 

A continuación se van a desarrollar cada uno de los pasos de la metodología Box-Jenkins explicados en la sección 2.3.1. 

### Identificación
Las figuras de abajo muestran la $FAC$ y la $FACP$ muestrales. Las mismas nos dan idea de un $ARMA(1,1)$, aunque el decaimiento de la $FAC$ no muestra el componente $MA(1)$ de una forma demasiado clara.

![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/49d0c43b-c2a0-4f54-a68c-baa66931aafd)

### Estimación y verificación de diagnóstico
Sin embargo, si se desconoce el verdadero proceso de generación de datos, uno podría tener ciertas dudas acerca del modelo real. Entonces, se pueden analizar diferentes modelos como los siguientes:

* **Modelo 3 - $AR(1)$:** $y_t=a_1y_{t-1}+\varepsilon_t$
* **Modelo 4 - $ARMA(1,1)$:** $y_t=a_1y_{t-1}+\varepsilon_t+\beta_1\varepsilon_{t-1}$
* **Modelo 5 - $ARMA(2)$:** $y_t=a_1y_{t-1}+a_2y_{t-2}+\varepsilon_t$

| Indicadores                               | Modelo 3               | Modelo 4               |Modelo 5                |
|-------------------------------------------|:----------------------:|:----------------------:|:----------------------:|
| $\hat{a}_1$  <br> (Error estándar)        |$-0.835$ <br> $(0.053)$ |$-0.679$ <br> $(0.076)$ |$-1.160$ <br> $(0.093)$ | 
| $\hat{a}_2$  <br> (Error estándar)        |                        |                        |$-0.378$ <br> $(0.092)$ |
| $\hat{\beta}_1$  <br> (Error estándar)    |                        |$-0.676$ <br> $(0.081)$ |                        |
| Criterio de Información de Akaike         |$496.5$                 |$471.0$                 |$482.8$                 |  
| Criterio Bayesiano de Schwartz            |$499.0$                 |$476.2$                 |$487.9$                 |  
| Ljung-Box Estadístico Q para los residuos <br> (nivel de significancia en paréntesis)      | $Q(8) = 26.19 (0.000)$ <br> $Q(24) = 41.10 (0.001)$ |$Q(8) = 3.86 (0.695)$ <br> $Q(24) = 14.23 (0.892)$ | $Q(8) = 11.44 (0.057)$ <br> $Q(24) = 22.59 (0.424)$ | 

Al examinar la tabla, note que todos los $\hat{a_1}$ son muy significativos; cada uno de los valores estimados se desvía al menos ocho desviaciones estándar de cero. 

Está claro que el modelo $AR(1)$ es inapropiado. Los estadísticos $Q$ para el **Modelo 3** indican que existe una autocorrelación significativa en los residuos (es decir, el nivel de significancia, de cada uno de los estadísticos $Q$ es inferior al 5%).

El modelo estimado $ARMA(1,1)$ (es decir. el **Modelo 4**) no sufre este problema. Además, tanto el _Criterio de Información de Akaike_ como el _Criterio Bayesiano de Schwartz_ seleccionan el **Modelo 4** sobre el **Modelo 3**. 

El mismo tipo de razonamiento indica que el **Modelo 4** es preferible al **Modelo 5**. Tenga en cuenta que, para cada modelo, los coeficientes estimados son altamente significativos y las estimaciones puntuales implican convergencia. Aunque el estadístico $Q$ en $24$ rezagos indica que estos dos modelos no tienen residuos correlacionados, el estadístico $Q$ de $8$ rezagos indica una correlación serial en los residuos del $Modelo 5$. Por lo tanto, el modelo $AR(2)$ no capta la dinámica de corto plazo como lo hace el modelo $ARMA(1,1)$. También tenga en cuenta que tanto el _Criterio de Información de Akaike_ como el _Criterio Bayesiano de Schwartz_ seleccionan el **Modelo 4**.


<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
