<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.4 (T):
# Pruebas de Hipótesis Adicionales en Modelos $VEC$

## ¿Cómo determinar la longitud de rezagos del $VEC$?

El procedimiento más común es estimar el $VAR$ de los datos no diferenciados. Luego se utilizan las mismas pruebas de longitud de rezagos que en un $VAR$ tradicional. Comience con el rezago mas largo que considere razonable y verifique si se puede acortar. Por ejemplo, si queremos probar si los rezagos $2$ a $4$ son importantes, podemos estimar los siguientes dos $VAR$:
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+A_2 x_{t-2}+A_3 x_{t-3}+A_4 x_{t-4}+e_{1t}}$
   * $\mathbf{x_t=A_0+A_1 x_{t-1}+e_{2t}}$

donde 
   * $\mathbf{x_t}$ es un vector de variables ( $n\times 1$ ),
   * $\mathbf{A_0}$ es la matriz ( $n\times 1$ ) de interceptos,
   * $\mathbf{A_1}$ son matrices ( $n\times n$  de coeficientes, y
   * $\mathbf{e_{1t}}$ y $\mathbf{e_{2t}}$ son vectores ( $n\times 1$ ) de términos de error.
   
   Calcule el primer sistema con cuatro rezagos de cada variable en cada ecuación y encuentre la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_4}$. Ahora estime la segunda ecuación usando solo un rezago de cada variable en cada ecuación y encuentre la matriz de varianzas y covarianzas de los residuos $\mathbf{\Sigma_1}$. Aunque trabajamos con variables no estacionarias, podemos realizar pruebas de longitud de rezagos utilizando el estadístico de prueba de razón de verosimilitud recomendado por [Sims (1980)](https://github.com/alvaroperdomo/World-Econometrics/tree/main/Referencias): $(T-c)(\ln{|\mathbf{\Sigma_1}|}-\ln{|\mathbf{\Sigma_4}|})$ donde 
* $T$ es el número de observaciones,
* $c$ es el número de parámetros en el sistema no restringido y
* $\ln{|\mathbf{\Sigma_i}|}$ es el logaritmo natural del determinante de $\mathbf{\Sigma_i}$.

Siguiendo a Sims, use la distribución $\chi^2$ con grados de libertad igual al número de restricciones de los coeficientes. Como cada $\mathbf{A_i}$ tiene $n^2$ coeficientes, la restricción $\mathbf{A_2=A_3=A_4=0}$  implica $3n^2$ restricciones . 

Generalizando la prueba de razón de verosimilitud, como en el caso de cualquier $VAR$, sea $\Sigma_u$ y $\Sigma_r$ las matrices de varianzas y covarianzas de los sistemas no restringidos y restringidos, respectivamente. Suponga que $c$ denota el número máximo de regresores contenidos en la ecuación más larga. El estadístico de prueba de la **razón de verosimilitud** $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ se puede comparar con una distribución $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. 

Alternativamente, al igual que en los modelos $VAR$, puede usar el **Criterio de Información de Akaike multivariado** o el **Criterio Bayesiano de Schwartz multivariado** para determinar la longitud del rezago. Si desea probar la longitud del rezago para una sola ecuación, una prueba $F$ es apropiada. 

## Los interceptos en los modelos VEC

En la prueba de Johansen, es importante determinar correctamente la forma de los regresores deterministas. Por ejemplo, los valores críticos de los estadísticos $\lambda_{traza}$ y $\lambda_{max}$ son más pequeños sin ningún tipo de regresores deterministas y más grandes con un intercepto en el vector de cointegración. 

En lugar de plantear con cautela la forma de $A_0$, es posible probar formas restringidas del vector [^1]. La idea clave de todas las pruebas de hipótesis es que **si hay $r$ vectores de cointegración, solo estas $r$ combinaciones lineales de las variables son estacionarias**. Todas las demás combinaciones lineales son no estacionarias. Por lo tanto, suponga que se reestima el modelo restringiendo los parámetros de $\pi$. Si las restricciones no son vinculantes, debe encontrar que el número de vectores de cointegración no ha disminuido. 

[^1]: **Uno de los aspectos más interesantes del procedimiento de Johansen es que permite probar formas restringidas de los $VEC$.**
Para probar la presencia de un intercepto en el VEC en oposición al intercepto no restringido $A_0$, estime las dos formas del modelo. Denote 

* las raíces características ordenadas de la matriz no restringida $\pi$ por $\hat{\lambda_i},\dots,\hat{\lambda_n}$, y
* las raíces características del modelo con los interceptos en los vectores de cointegración por $\hat{\lambda_i^* },\dots,\hat{\lambda_n^* }$. 

Suponga que la forma no restringida del modelo tiene $r$ raíces características distintas de cero.  Asintóticamente, el estadístico 
$-T\displaystyle\sum_{i=r+1}^n[\ln{(1-\hat{\lambda_i^* )}}-\ln{(1-\hat{\lambda_n})}]$ tiene una distribución $\chi^2$ con $(n-r)$ grados de libertad. La intuición detrás de la prueba es que todos los valores de $\ln{1-\hat{\lambda_i^*}}$ y $\ln{1-\hat{\lambda_n}}$ deben ser equivalentes si la restricción no es vinculante. Por lo tanto, valores pequeños del estadístico de prueba implican que está permitido incluir el intercepto en el vector de cointegración. Sin embargo, la probabilidad de encontrar una combinación lineal estacionaria de las 𝑛 variables es mayor con el intercepto en el vector de cointegración que si el intercepto está ausente de este. 

Por lo tanto, un valor grande de $\hat{\lambda_{r+1}^* }$ [y en consecuencia de $-T \ln{(1-\hat{\lambda_i^* )}}$], implica que la restricción infla artificialmente el número de vectores de cointegración. Entonces, como lo demuestra [Johansen (1991)](https://github.com/alvaroperdomo/World-Econometrics/tree/main/Referencias), si el estadístico de prueba es suficientemente grande, es posible rechazar la hipótesis nula de un intercepto en el vector de cointegración y concluir que hay una tendencia lineal en las variables. 

## ¿Por qué se recomienda no hacer pruebas de causalidad de Granger y de exogeneidad en en los modelos los $VEC$?

[Sims, Stock y Watson (1990)](https://github.com/alvaroperdomo/World-Econometrics/tree/main/Referencias) establecen que no se pueden realizar pruebas de causalidad de Granger en un sistema cointegrado usando una prueba $F$ estándar. Cuando el $rango(\pi)=0$ entonces $\mathbf{\Delta x_t= \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$. En este caso (es decir, en un $VAR$ en diferencias), sólo hay variables estacionarias. Por lo tanto, las pruebas de causalidad de Granger se pueden realizar utilizando una distribución $F$ estándar. Sin embargo, si las variables están cointegradas, $\mathbf{\Delta x_t=\pi^* x_{t-1} + \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$, una prueba de causalidad de Granger involucra los coeficientes de $\pi$. Dado que estos coeficientes multiplican las variables no estacionarias, no es apropiado usar un estadístico $F$ para probar la causalidad de Granger. Después de todo, si el $rango(\pi)=0$, es imposible escribir las restricciones de la prueba como restricciones en un conjunto de variables $I(0)$. 

[Sims, Stock y Watson (1990)](https://github.com/alvaroperdomo/World-Econometrics/tree/main/Referencias), también descartan por razones similares las pruebas de exogeneidad en bloque. 

---
---
# Preguntas de selección múltiple

1. **¿Por qué Sims, Stock y Watson (1990) recomiendan no hacer pruebas de causalidad de Granger y de exogeneidad en en los modelos los $VEC$?**
 
   a) Porque un modelo $VEC$ se basa en un modelo $VAR$ en primeras diferencias.

   b) Porque estas pruebas se basan en la prueba $F$ estándar.

   c) Porque el $VEC$ puede incluir series que no esten contegradas.

   d) Todas las anteriores.
 
2. **¿Cómo determinar la longitud de rezagos del $VEC$?:**

   a) Utilizando el **Criterio de Información de Akaike multivariado**.

   b) Utilizando el **Criterio Bayesiano de Schwartz multivariado**.

   c) Utilizando la **prueba de razón de verosimilitud**.

   d) Todos los anteriores.
---
---  

| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |[Subsección: 3.2.4.(R)  Pruebas de Hipótesis Adicionales de Modelos VEC en _R_](../Seccion03_02_04_R/Readme.md) |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
