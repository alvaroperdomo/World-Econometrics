## Los procesos ruido blanco
Una secuencia { $\epsilon_t$ } es un proceso de **ruido blanco** si cada valor en la secuencia tiene una media de cero, una varianza constante, y no está correlacionado con todas las demás realizaciones. 

Formalmente, si $E(x)$ es el valor medio teórico de $x$, entonces la secuencia { $\epsilon_t$ } es un proceso de ruido blanco si para cada período de tiempo $t$:
* $E(\epsilon_t)=E(\epsilon_{t-1})= · · · =0$
* $E(\epsilon_{t}^2)=E(\epsilon_{t-1}^2)= · · · =\sigma^2$ ó [ $\sigma_{\epsilon_{t}}^2=\sigma_{\epsilon_{t-1}}^2= · · · =\sigma^2$ ]
* $E(\epsilon_{t}\epsilon_{t-s})=E(\epsilon_{t-j}\epsilon_{t-j-s})=0,  \forall j\neq s$ ó [ $\sigma_{\epsilon_{t}\epsilon_{t-s}}=\sigma_{\epsilon_{t-j}\epsilon_{t-j-s}}=0$ ]

## Los procesos estacionarios
Un proceso estocástico que tiene una media y una varianza finita es estacionario en covarianza si para todo $t$ y $t-s$,
1) 𝐸(𝑦_𝑡 )=𝐸(𝑦_(𝑡−𝑠))=𝜇 (Media constante)
2) 𝐸 [(𝑦_𝑡− 𝜇)^2 ]=𝐸〖(𝑦_(𝑡−𝑠)− 𝜇)〗^2  = 𝜎_𝑦^2(Varianza constante)
3) 𝐸 [(𝑦_𝑡−𝜇) (𝑦_(𝑡−𝑠)−𝜇)]=𝐸[(𝑦_(𝑡−𝑗)−𝜇) (𝑦_(𝑡−𝑗−𝑠)−𝜇)]=𝛾_𝑠(autocovarianzas de la misma amplitud constante) donde 𝜇, 𝜎_𝑦^2, y 𝛾_𝑠 son todas constantes.
![image](https://github.com/alvaroperdomo/World-Econometrics/assets/127871747/9e7de65f-925c-4d51-abfd-0df692fe95ec)

