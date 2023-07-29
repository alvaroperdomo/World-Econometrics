<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.1. (T)
# ¿Qué es y para qué sirve la cointegración?

En los modelos univariados la tendencia estocástica de una serie de tiempo puede eliminarse por diferenciación, y las series estacionarias resultantes se pueden estimar utilizando las técnicas univariadas de Box-Jenkins analizadas previamente en la sección 2. Anteriormente, la práctica convencional era generalizar esta idea a los modelos multivariados y diferenciar todas las variables no estacionarias utilizadas en un análisis de regresión para luego estimar un modelo $VAR$ en diferencias. Sin embargo, hoy en día la forma adecuada de tratar las variables no estacionarias no es tan sencilla en un contexto multivariado. Es bastante posible que haya una combinación lineal de variables integradas que sea estacionaria y que por lo tanto configuren una relación de largo plazo; si esto ocurre se dice que tales variables están **cointegradas**[^1]. 

[^1]: **En cierto sentido, el uso del término equilibrio no es el mejor porque la teoría económica y la econometría usan el término de diferentes maneras. En teoría económica usualmente se habla de equilibrio al referirse a una igualdad entre transacciones deseadas y reales. En econometría el equilibrio hace referencia a cualquier relación a largo plazo entre variables no estacionarias. La cointegración no requiere que la relación a largo plazo sea generada por las fuerzas del mercado o por las reglas de comportamiento de los individuos. Según Engle y Granger (1987) la relación de equilibrio puede ser causal, conductual o simplemente una relación de forma reducida entre variables con tendencias similares.** 

Para entender mejor lo que es la cointegración considere la siguiente situación: 

Asuma que hay $n$ variables $x_{1t}, x_{2t}, \dots, x_{nt}$ integradas del mismo orden que conforman un equilibrio de largo plazo representado por $\eqalign{\sum_{i=1}^n \beta_i x_{it}=0}$. Es decir, $\mathbf{B x_t}=0$ donde $\eqalign{\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}}$ y $\eqalign{\mathbf{x_t} = {\left\lbrack \matrix{x_{1t} \cr x_{2t} \cr \dots \cr x_{nt} } \right\rbrack}}$

La desviación del equilibrio a largo plazo, llamado error de equilibrio es $e_t$, por lo tanto $e_t=\mathbf{B x_t}$. **Si el equilibrio es significativo, $e_t$ debe ser estacionario**. 

Engle y Granger (1987) proporcionan la siguiente definición de cointegración: Se dice que los componentes del vector $\mathbf{x_t}$ son cointegrados de orden $d,b$, denotado por $\mathbf{x_t}\sim CI(d,b)$  si 
1) Todos los componentes de $\mathbf{x_t}$ son integrados de orden $d$.
2) Existe un vector $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ tal que la combinación lineal $\mathbf{B x_t}=0$  está integrada de orden $(d-b)$ donde $b>0$. El vector $\mathbf{B}$ se llama el vector de cointegración.

Hay cuatro puntos importantes a tener en cuenta sobre la definición:
1) **La cointegración se refiere a una combinación lineal de variables no estacionarias**.

   Por lo tanto, el vector de cointegración no es único. Si $\mathbf{B} = {\left\lbrack \matrix{\beta_1 & \beta_2 & ... & \beta_n} \right\rbrack}$ es un vector de cointegración, entonces para cualquier parámetro $\lambda \not= 0$, $\mathbf{\lambda B} = {\left\lbrack \matrix{\lambda\beta_1 & \lambda\beta_2 & \dots & \lambda\beta_n} \right\rbrack}$ también es un vector de cointegración.

   Normalmente, una de las variables se usa para normalizar el vector de cointegración fijando su coeficiente en $1$. Por ejemplo, para normalizar el vector de cointegración con respecto a $x_{2t}$, simplemente se selecciona un $\tilde{\lambda}=\frac{1}{\beta_2}$ de tal forma que $\mathbf{\tilde{\lambda} B} = {\left\lbrack \matrix{ \frac{\beta_1}{\beta_2} & 1 & \dots & \frac{\beta_n}{\beta_2}} \right\rbrack}$.

2) **La cointegración se refiere a variables que están integradas en el mismo orden**.

   Esto no implica que todas las variables integradas estén cointegradas:

   * No es para nada raro que un conjunto de variables integradas de orden $d$ [es decir, variables que son $I(d)$] no está compuesto por variables cointegradas.[^2]
   * **Si dos variables son integradas de órdenes diferentes, no pueden estar cointegradas.**

   Sin embargo, **es posible encontrar relaciones de equilibrio entre grupos de variables que están integradas de diferentes órdenes.** Suponga que $x_{1t}$ y $x_{2t}$ son $I(2)$ y que las otras variables en consideración son $I(1)$. Como tal, no puede haber una relación de cointegración entre $x_{1t}$ (o $x_{2t}$) y $x_{3t}$. Sin embargo, si $x_{1t}$ y $x_{2t}$ son $CI(2,1)$, existe una combinación lineal de la forma $\beta_1x_{1t}+\beta_2x_{2t}$ que es $I(1)$, la cual es posible que esté cointegrada con algunas de las otras variables $I(1)$ del grupo.

[^2]: **Tal falta de cointegración implica que no hay un equilibrio a largo plazo entre las variables, de modo que puedan desviarse arbitrariamente una de la otra.**


3) **Puede haber más de un vector de cointegración independiente para un conjunto de variables $I(1)$.**[^3]

   Si $\mathbf{x_t}$ tiene $n$ componentes no estacionarios, puede haber hasta $n-1$ vectores de cointegración linealmente independientes. Por lo tanto, tenga en cuenta que si $\mathbf{x_t}$ contiene solo dos variables, puede haber a lo sumo un vector de cointegración independiente.

[^3]: **El número de vectores de cointegración se denomina rango de cointegración del vector.**

4) **La mayor parte de la literatura sobre cointegración se centra en el caso en el que cada variable tiene una sola raíz unitaria.**

   La razón es que la regresión tradicional o el análisis de series de tiempo se aplica cuando las variables son $I(0)$ y pocas variables económicas están integradas en un orden superior a $1$. 

Para el análisis de la existencia de cointegración las metodologías más utilizadas son la prueba de Engle y Granger (1987) y la prueba de Johansen (1988). En las siguientes subsecciones se explica cómo funcionan ambas pruebas.

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

| [Subsección: 3.2 - Cointegración y estimación de Modelos](../Readme.md) |[Siguiente Subsección: 3.2.2. Pruebas de Raíz Unitaria](../Seccion03_02_02/Readme.md) |
|----------------------------------------------------------------------|-----------------------------------------------------------------------------------------|

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
