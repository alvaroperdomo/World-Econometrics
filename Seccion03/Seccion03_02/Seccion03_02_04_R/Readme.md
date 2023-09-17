<div align="center"><a href="https://www.escuelaing.edu.co/es/investigacion-e-innovacion/centro-de-estudios-oikonimia/" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/Centros_%20de_Estudios_Oikonom%C3%ADa.jpg" alt="R.LTWB" width="100%" border="0" /></a></div>

## SUBSECCIÓN 3.2.4 (R):
# Pruebas de Hipótesis de modelos $VEC$ en $R$
## 1) Número de rezagos
Para determinar el número óptimo de rezagos dentro del VEC se utiliza el comando:

``` r
VARorder(x,maxp=10)
```
| **Argumentos**     | **Descripción**                                                                                                     | 
|--------------------|---------------------------------------------------------------------------------------------------------------------|
| **x**              | vector de variables de series de tiempo que incluye las variables endógenas utilizadas en el VEC                    |
| **maxp**           | el número máximo de rezagos utilizados en la prueba (la **_Opción Predeterminada es 13_**)                          |

## 2) Gráfico de la $FAC$ y la $FACP$ de los residuos de la prueba de cointegración de Johansen
Recuerde que los residuos del la prueba de cointegración de Johansen tienen que ser ruido blanco ruido blanco. Entonces, el siguiente comando permite ver el gráfico de la $FAC$ y de la $FACP$ para verificar que los mismos no esten correlacionados:

``` r
plotres(nombre)
```
| **Argumentos**     | **Descripción**                                                        | 
|--------------------|------------------------------------------------------------------------|
| **nombre**         | nombre del $VEC$ que se estimó  con el comando "ca.jo"                 |

## 3) Análisis de exogeneidad débil sobre las variables endógenas modelo $VEC$

Primero se tiene que crear una matriz en donde se recree la presencia de la endógeneidad. Por ejemplo, asuma que esta estimando un modelo "VEC" con tres variables y queremos saber si la segunda variable es exógena, entonces proponemos la siguiente matriz (2x3) que hemos llamada Matriz_A: [^1]
[^1]: **La matriz para probar la exogeneidad de la primera variable es Matriz_1 <- t(matrix(c(0, 0, 1, 0, 0, 1), nrow = 2, ncol = 3)). La matriz para probar la exogeneidad de la tercera variable es Matriz_1 <- t(matrix(c(1, 0, 0, 1, 0, 0), nrow = 2, ncol = 3))**


``` r
Matriz_A <- t(matrix(c(1, 0, 0, 0, 0, 1), nrow = 2, ncol = 3))

```
Al calcularla observe que al copiar _Matriz_A_ $R$ nos reporta

``` r
> Matriz_A
     [,1] [,2]
[1,]    1    0
[2,]    0    0
[3,]    0    1
```

Posteriormente se utliza el comando _alrtest_ así:

``` r
new_nombre <-alrtest(z = nombre, A = Matriz_A, r = 1)
summary(new_nombre)
```

Y se observa el p-value del estadístico en:

``` r
The value of the likelihood ratio test statistic:
"NÚMERO" distributed as chi square with "NÚMERO" df.
The p-value of the test statistic is: "NÚMERO"
```

Si este valor es menor al 5% (1% ó 10%) entonces se rechaza la hipótesis nula de exogeneidad débil, en caso contrario no se rechaza.

| **Argumentos**     | **Descripción**                                                        | 
|--------------------|------------------------------------------------------------------------|
| **new_nombre**     | nombre del $VEC$ restringido que se va a estimar                       |
| **nombre**         | nombre del $VEC$ que se estimó  con el comando "ca.jo"                 |
| **r**              | número de vectores de cointegración que tiene el $VEC$ original        |


## 4) Funciones impulso-respuesta y análisis de descomposición de varianza
Para calcular las funciones impulso-respuesta y los análisis de dedescomposición de varianza, primero se tiene que transformar el modelo $VEC$ en su equivalente modelo $VAR$ utilizando el comando _vec2var_ así:
``` r
nombre_VAR <- vec2var(nombre_VEC, r = 1)
```

| **Argumentos**     | **Descripción**                                                        | 
|--------------------|------------------------------------------------------------------------|
| **nombre_VAR**     | nombre que va a tener el modelo $VAR$                                  |
| **nombre_VEC**     | nombre del $VEC$ que se estimó  con el comando "ca.jo"                 |
| **r**              | número de vectores de cointegración que tiene el $VEC$ original        |

Posteriormente, ya teniendo la representación $VAR$ se hace el mismo tipo de análisis que se explico en la sección 3.1.4.(R).


---
---
# Preguntas de selección múltiple

1. **¿En el comando _**VARorder**_ ¿cuál es el número máximo de rezagos utilizado en la prueba?:**
 
   a) $0$.

   b) $5$.

   c) $10$.

   d) Ninguno de los anteriores.
 
2. **¿Cuál comando permite ver el gráfico de la $FAC$ y de la $FACP$ de los residuos de la prueba de cointegración de Johansen para verificar que los mismos no estén correlacionados?:**

   a) grafVEC.

   b) VARorder.

   c) plotres.

   d) Ninguno de los anteriores.
---
---

| [Subsección: 3.2 - Cointegración y estimación de Modelos _VEC_](../Readme.md) |[Subsección: 3.2.4.(T)  Pruebas de Hipótesis Adicionales de Modelos _VEC_](../Seccion03_02_04_T/Readme.md) |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|

<div align="center"><a href="https://enlace-academico.escuelaing.edu.co/psc/FORMULARIO/EMPLOYEE/SA/c/EC_LOCALIZACION_RE.LC_FRM_ADMEDCO_FL.GBL" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/IconCEHBotonCertificado.png" alt="World-Econometrics" width="260" border="0" /></a></div>

##

<div align="center"><a href="http://www.escuelaing.edu.co" target="_blank"><img src="https://github.com/alvaroperdomo/World-Econometrics/blob/main/.icons/banner-pie-de-pagina.jpg" alt="Support by" width="100%" border="0" />

</div>
