## Pruebas de Raíz Unitaria
Tradicionalmente la prueba más utilizada es la **Prueba Aumentada de Dickey-Fuller - ADF**

En esta prueba David Dickey y Wayne Fuller proponen estimar

$$y_t = a_0 + \gamma y_{t-1} +\sum_{i = 2}^{p} \beta_i y_{t-i+1} + a_2 t + \varepsilon_t$$



## Pruebas de Raíz Unitaria en R

Para esta sección hay que instalar el paquete [fUnitRoots](https://cran.r-project.org/web/packages/fUnitRoots/index.html)

``` r
install.packages('fUnitRoots')
```

| Subsecciones                                                                                        | Alcance                                                              | Dedicación,<br>5.5 horas  | 
|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|:-------------------------:|
| [La Prueba ADF: _Prueba Aumentada de Dickey Fuller_](Section01/WhatIsLTWB)                          | Explicación general de la prueba                                     |             1.5           | 
| [La Prueba KPSS: _Prueba de Kwiatkowski, Phillips, Schmidt y Shin_](Section01/Requirement)          | Explicación general de la prueba                                     |             0.5           |   
| [La Prueba ERZ: _Prueba de Elliott, Rothenberg y Stock_](Section01/Requirement)                     | Explicación general de la prueba                                     |             0.5           |  
| [La Prueba ZA: _Prueba de Zivot y Andrews_](Section01/CaseStudy)                                    | Explicación general de la prueba                                     |             0.5           |       

