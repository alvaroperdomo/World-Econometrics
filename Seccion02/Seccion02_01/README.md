## Los procesos ruido blanco
Una secuencia { $\varepsilon_t$ } es un proceso de **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones. 

Formalmente, si $E(x)$ es el valor medio teórico de $x$, entonces la secuencia { $\epsilon_t$ } es un proceso de ruido blanco si para cada período de tiempo $t$:
* $E(\varepsilon_t)=E(\varepsilon_{t-1})= · · · =0$
* $E(\varepsilon_{t}^2)=E(\varepsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\varepsilon_{t}}^2=\sigma_{\varepsilon_{t-1}}^2= · · · =\sigma^2$ ]
* $E(\varepsilon_{t}\varepsilon_{t-s})=E(\varepsilon_{t-j}\varepsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\varepsilon_{t}\varepsilon_{t-s}}=\sigma_{\varepsilon_{t-j}\varepsilon_{t-j-s}}=0$ ]

## Los procesos estacionarios
Un proceso estocástico que tiene una media y una varianza finita es estacionario en covarianza si para todo $t$ y $t-s$,
1) Media constante: $E(y_t)=E(y_{t-s})=\mu $
3) Varianza constante: $E[(y_{t}- \mu)^2]=E[(y_{t-s}- \mu)^2]=\sigma_{y}^2$
4) =E(y_{t-s- \mu }^2)= · · · =\sigma^2
5) 𝐸 [(𝑦_𝑡− 𝜇)^2 ]=𝐸〖(𝑦_(𝑡−𝑠)− 𝜇)〗^2  = 𝜎_𝑦^2(Varianza constante)
6) 𝐸 [(𝑦_𝑡−𝜇) (𝑦_(𝑡−𝑠)−𝜇)]=𝐸[(𝑦_(𝑡−𝑗)−𝜇) (𝑦_(𝑡−𝑗−𝑠)−𝜇)]=𝛾_𝑠(autocovarianzas de la misma amplitud constante) donde 𝜇, 𝜎_𝑦^2, y 𝛾_𝑠 son todas constantes.
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/9e7de65f-925c-4d51-abfd-0df692fe95ec)

