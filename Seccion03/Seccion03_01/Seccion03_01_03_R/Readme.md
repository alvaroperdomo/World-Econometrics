## SECCIÓN 3.1.3. (R):

# Funciones impulso Respuesta en R

Las funciones impulso-respuesta se obtienen utilizando el siguiente comando:[^1]

[^1]:A este comandos se le pueden incluir más argumentos, pero sólo nos vamos a enfocar en los que se colocan a continuación:

``` r
irf(nombre, impulse = NULL, response = NULL, n.ahead = 10, ortho = TRUE)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **impulse**        | variable o vector de variables a las que se les genera el impulso (**_Opción Predeterminada es "all"**)             |
| **response**       | variable o vector de variables a las que se les evalua la respuesta  (**_Opción Predeterminada es "all"**)          |
| **ortho=TRUE**     | Se calculan los coeficientes impulso-respuesta ortogonalizados                                                      | 


``` r
modelo.irf<-irf(modelo,impulse="x", response="y")
modelo.irf2<-irf(modelo,impulse="x", response="x")
plot(modelo.irf)
plot(modelo.irf2)
```

# Descomposición de Varianza en R

``` r
fevd(nombre, n.ahead=10, ...)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **nombre**         | Nombre del Vector autorregresivo (VAR) que ha sido estimado                                                         |
| **n.ahead**        | Número de periodos para los cuales se va a calcular la descomposición de varianza                                   |

