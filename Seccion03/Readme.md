## SECCIÓN 3:

# Análisis Multivariado de Series de Tiempo
Los modelos de series de tiempo univariados pueden ser muy útiles para el análisis descriptivo y para el pronóstico fuera de muestra. Sin embargo, son restrictivos en el sentido de que se basan únicamente en la historia de la serie de tiempo y no intentan explotar la información presente en otras variables. 

Dado que a menudo las variables económicas están estrechamente relacionadas con otras variables, dicha información puede ser potencialmente útil y su uso puede llevar a descripciones más realistas del comportamiento de las variables económicas y también a pronósticos más precisos fuera de la muestra. 

De hecho, ignorar las relaciones con otras variables puede dar lugar a complicaciones en la especificación apropiada de un modelo univariado series de tiempo. Puede ser que la especificación empírica de un modelo univariado se vea obstaculizada por la presencia de valores atípicos y de cambios estructurales, que pueden atribuirse a una o más variables distintas de la variable $y_t$ en cuestión.

Para acceder a cada una de las subsecciones haga _click_ en la tabla de abajo en la subsección respectiva

| Subsecciones                                                                 | Contenido                                                                           | Dedicación,<br> 5.5 horas       | 
|--------------------------------------------------------------------------|-------------------------------------------------------------------------------------|:-------------------------:|
| [**3.1.** Estimación de Modelos VAR](Seccion03_01/Readme.md)                 | Explicación acerca de la metododología para estimar un modelo VAR.                  |              1            | 
| [**3.2.** Cointegración y Estimación de Modelos VEC](Seccion03_02/Readme.md) | Explicación acerca de lo qué es la cointegración y acerca de la metododología para estimar un modelo VEC. |              1            | 


| [:house: Inicio](../README.md)    | [Siguiente Sección: 3.1. Estimación de Modelos VAR](Seccion03_01/Readme.md) |
|-----------------------------------|-----------------------------------------------------------------------------|

# VAR GGOV INVP
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)
library(vars)
library(MTS)

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')
WDIsearch(string='GC.NFN.TOTL.GD.ZS', field='indicator')

dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS", INVP="GC.NFN.TOTL.GD.ZS"), country=c('CL'), language = "es")
# dat <- dat %>% arrange(country, year)
dat <- na.omit(dat)

dat <- mutate(dat, country=NULL, iso2c=NULL, iso3c=NULL)

series <-ts(dat,frequency=1,start = 1972)

VARorder(series, maxp = 5, output = T)

# Pruebas ARIMA GGOV
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')

dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS"), country=c('CL'), language = "es")
#dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, GGOV_lag1 = lag(GGOV, order_by = year), C1GGOV=GGOV-GGOV_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

C1GGOV_ <- dat[-c(1),]
GGOV_ = subset(dat, select = c(GGOV))
C1GGOV_ = subset(C1GGOV_, select = c(C1GGOV))

GGOV <- ts(GGOV_)
C1GGOV <- ts(C1GGOV_)
autoplot(acf(C1GGOV, plot = FALSE))
autoplot(pacf(C1GGOV, plot = FALSE))

arima1<- Arima(GGOV, order=c(0,1,2))
arima2<- Arima(GGOV, order=c(1,1,0))
arima3<- Arima(GGOV, order=c(1,1,0), include.mean = FALSE)
arima4<- Arima(GGOV, order=c(1,1,1))
arima5<- Arima(GGOV, order=c(0,1,1))

AIC(arima1,arima2,arima3,arima4,arima5)
BIC(arima1,arima2,arima3,arima4,arima5)

autoplot(acf(arima2$residuals, plot = FALSE))
autoplot(pacf(arima2$residuals, plot = FALSE))

ggtsdiag(arima2)

lb <- Box.test(arima2$residuals, type="Ljung-Box") # Test de Ljung-Box
lb$p.value

auto.arima(GGOV, stepwise = FALSE, approximation = FALSE)
auto.arima(C1GGOV, stepwise = FALSE, approximation = FALSE)

forecast1<-forecast(arima2, level = c(95), h = 3)
autoplot(forecast1)
summary(forecast1)

# Pruebas ARIMA INVP
rm(list = ls())

library(WDI)
library(dplyr)
library(ggfortify)
library(ggplot2)
library(forecast)
library(fUnitRoots)
library(urca)

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')

dat = WDI(indicator= c(INVP = "NE.CON.GOVT.ZS"), country=c('CL'), language = "es")
#dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, INVP_lag1 = lag(INVP, order_by = year), C1INVP=INVP-INVP_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

C1INVP_ <- dat[-c(1),]
INVP_ = subset(dat, select = c(INVP))
C1INVP_ = subset(C1INVP_, select = c(C1INVP))

INVP <- ts(INVP_)
C1INVP <- ts(C1INVP_)
autoplot(acf(C1INVP, plot = FALSE))
autoplot(pacf(C1INVP, plot = FALSE))

arima1<- Arima(INVP, order=c(0,1,2))
arima2<- Arima(INVP, order=c(1,1,0))
arima3<- Arima(INVP, order=c(1,1,0), include.mean = FALSE)
arima4<- Arima(INVP, order=c(1,1,1))
arima5<- Arima(INVP, order=c(0,1,1))

AIC(arima1,arima2,arima3,arima4,arima5)
BIC(arima1,arima2,arima3,arima4,arima5)

autoplot(acf(arima2$residuals, plot = FALSE))
autoplot(pacf(arima2$residuals, plot = FALSE))

ggtsdiag(arima2)

lb <- Box.test(arima2$residuals, type="Ljung-Box") # Test de Ljung-Box
lb$p.value

auto.arima(INVP, stepwise = FALSE, approximation = FALSE)
auto.arima(C1INVP, stepwise = FALSE, approximation = FALSE)

forecast1<-forecast(arima2, level = c(95), h = 3)
autoplot(forecast1)
summary(forecast1)

# Pruebas UNIT ROOT GGOV

rm(list = ls())

library(WDI)         # Esta libreria sirve para trabajar directamente con la base de datos Indicadores de Desarrollo Mundial.
library(dplyr)       # Esta libreria permite manipular las bases de datos de R de una forma sencilla, por ejemplo utilizando los comandos mutate() y arrange()
library(ggfortify)   # Esta libreria tienen comando utiles para plantear gráficos de series de tiempo, por ejemplo utilizando el comando autoplot()
library(ggplot2)     # Esta librería sirve para construir gráficos interesantes
library(fUnitRoots)  # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(urca)        # Esta libreria sirve para hacer pruebas de raíz unitaria.
library(forecast)    # Esta libreria sirve para hacer pronósticos.

WDIsearch(string='NE.CON.GOVT.ZS', field='indicator')

dat = WDI(indicator= c(GGOV = "NE.CON.GOVT.ZS"), country=c('CL'), language = "es")
dat <- dat %>% arrange(year)
dat <- na.omit(dat)
dat <- mutate(dat, GGOV_lag1 = lag(GGOV, order_by = year), C1GGOV=GGOV-GGOV_lag1, country=NULL, iso2c=NULL, iso3c=NULL)

ggplot(dat, aes(year, GGOV)) + scale_x_continuous(name="Años", limits=c(1960, 2019)) + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1960-2019", y="Porcentaje", title="Gasto real del Gobierno (%PIB)", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

ggplot(dat, aes(year, C1GGOV)) + scale_x_continuous(name="Años", limits=c(1960, 2019)) + geom_line (linewidth=0.2) + theme(plot.caption = element_text(size=7)) + labs(subtitle="1960-2019", y="Porcentaje", title="Cambio en el gasto real del Gobierno (%PIB)", caption = "Fuente: Construcción propia a partir de los Indicadores de Desarrollo Mundial del Banco Mundial")

GGOV__ <- dat[-c(1:12, 58:63),]
C1GGOV__ <- GGOV__[-c(1),]
GGOV_ = subset(GGOV__, select = c(GGOV))
C1GGOV_ = subset(C1GGOV__, select = c(C1GGOV))

GGOV <- ts(GGOV_, start=1972, end=2016)
C1GGOV <- ts(C1GGOV_, start=1972, end=2016)

autoplot(acf(GGOV, plot = FALSE))
autoplot(acf(C1GGOV, plot = FALSE))

GGOV_MCO <- lm(GGOV ~ year, data = GGOV__)
C1GGOV_MCO <- lm(C1GGOV ~ year, data = C1GGOV__)
summary(GGOV_MCO)
summary(C1GGOV_MCO)

# (1) ADF (raíz unitaria y estacionario)
GGOV_ur_trend_AIC.df <- ur.df(y=GGOV, type = c("trend"), lags = 10, selectlags = c("AIC"))
GGOV_ur_trend_BIC.df <- ur.df(y=GGOV, type = c("trend"), lags = 10, selectlags = c("BIC"))
C1GGOV_ur_none_AIC.df <- ur.df(C1GGOV, type = c("none"), lags = 10, selectlags = c("AIC"))
C1GGOV_ur_none_BIC.df <- ur.df(C1GGOV, type = c("none"), lags = 10, selectlags = c("BIC"))

summary(GGOV_ur_trend_AIC.df)
summary(GGOV_ur_trend_BIC.df)
summary(C1GGOV_ur_none_AIC.df)
summary(C1GGOV_ur_none_BIC.df)

GGOV_urdfTest<- urdfTest(GGOV, lags = 2, type = c("ct"), doplot = TRUE)
C1GGOV_urdfTest<- urdfTest(C1GGOV, lags = 1, type = c("nc"), doplot = TRUE)

ndiffs(GGOV, alpha = 0.05, test = c("adf"), type = c("trend"))

# (2) ADF-GLS (raíz unitaria y ????)
GGOV_DFGLSt.ers <- ur.ers(GGOV, type = c("P-test"), model = c("trend"),lag.max = 4)
C1GGOV_DFGLSc.ers <- ur.ers(C1GGOV, type = c("P-test"), model = c("constant"),lag.max = 4) 

summary(GGOV_DFGLSt.ers)
#summary(C1GGOV_DFGLSc.er)

# (3) KPSS (raíz unitaria y estacionario)
GGOV_1t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("nil"), use.lag = NULL)
GGOV_2t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("short"), use.lag = NULL)
GGOV_3t.kpss <- ur.kpss(GGOV, type = c("tau"), use.lag = 4)
GGOV_4t.kpss <- ur.kpss(GGOV, type = c("tau"), lags = c("long"), use.lag = NULL)

C1GGOV_1m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("nil"), use.lag = NULL)
C1GGOV_2m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("short"), use.lag = NULL)
C1GGOV_3m.kpss <- ur.kpss(C1GGOV, type = c("mu"), use.lag = 4)
C1GGOV_4m.kpss <- ur.kpss(C1GGOV, type = c("mu"), lags = c("long"), use.lag = NULL)

summary(GGOV_1t.kpss)
summary(GGOV_2t.kpss)
summary(GGOV_3t.kpss)
summary(GGOV_4t.kpss)
summary(C1GGOV_1m.kpss)
summary(C1GGOV_2m.kpss)
summary(C1GGOV_3m.kpss)
summary(C1GGOV_4m.kpss)

ndiffs(GGOV, alpha = 0.05, test = c("kpss"), type = c("trend"))

# (4) ZA (raíz unitaria y estacionario)

ZA1.za <- ur.za(GGOV, model = c("intercept"), lag=NULL)
ZA2.za <- ur.za(GGOV, model = c("trend"), lag=NULL)
ZA3.za <- ur.za(GGOV, model = c("both"), lag=NULL)

summary(ZA1.za)
summary(ZA2.za)
summary(ZA3.za)
