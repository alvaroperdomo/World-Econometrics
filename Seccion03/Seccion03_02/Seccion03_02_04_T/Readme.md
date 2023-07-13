## SECCIÓN 3.2.4:
# Pruebas de Hipótesis Adicionales en modelos VEC

## ¿Cómo determinar la longitud de rezagos del VEC?

La manera más sencilla de comprender las pruebas de longitud de rezagos es considerar el sistema en la forma $\mathbf{\Delta x_t=\pi^* x_{t-1} + \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$. Independientemente del rango de $\pi$, todos los $\Delta x_{t-i}$ son variables estacionarias. Entonces, a partir de Sims, Stock y Watson (1990), sabemos que los coeficientes de interés en las variables estacionarias de media cero se pueden probar utilizando una distribución normal. Dado que la longitud del rezago depende únicamente de los valores de los diversos $\pi_i$, una distribución $\chi^2$ es apropiada para probar cualquier restricción relacionada con la longitud del rezago. Como en el caso de cualquier $VAR$, sea $\Sigma_u$ y $\Sigma_r$ las matrices de varianzas y covarianzas de los sistemas no restringidos y restringidos, respectivamente. Suponga que $c$ denota el número máximo de regresores contenidos en la ecuación más larga. El estadístico de prueba de la **razón de verosimilitud** $(T-c)(\ln{|\mathbf{\Sigma_r}|}-\ln{|\mathbf{\Sigma_u}|})$ se puede comparar con una distribución $\chi^2$ con grados de libertad igual al número de restricciones en el sistema. 

Alternativamente, puede usar el **Coeficiente de Información de Akaike multivariado** o el **Coeficiente Bayesiano de Schwartz multivariado** para determinar la longitud del rezago. Si desea probar la longitud del rezago para una sola ecuación, una prueba $F$ es apropiada. 

## ¿Por qué se recomienda no hacer pruebas de causalidad de Granger y de exogeneidaden en los modelos los VEC?

Sims, Stock y Watson (1990) establecen que no se pueden realizar pruebas de causalidad de Granger en un sistema cointegrado usando una prueba $F$ estándar. Cuando el $rango(\pi)=0$ entonces $\mathbf{\Delta x_t= \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$. En este caso (es decir, en un $VAR$ en diferencias), sólo hay variables estacionarias. Por lo tanto, las pruebas de causalidad de Granger se pueden realizar utilizando una distribución $F$ estándar. Sin embargo, si las variables están cointegradas, $\mathbf{\Delta x_t=\pi^* x_{t-1} + \displaystyle\sum_{i=1}^{p-1} + \varepsilon_t}$, una prueba de causalidad de Granger involucra los coeficientes de $\pi$. Dado que estos coeficientes multiplican las variables no estacionarias, no es apropiado usar un estadístico $F$ para probar la causalidad de Granger. Después de todo, si el $rango(\pi)=0$, es imposible escribir las restricciones de la prueba como restricciones en un conjunto de variables $I(0)$. 

Sims, Stock y Watson (1990), también descartan por razones similares las pruebas de exogeneidad en bloque. 
