
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Executive summary

In this article we study the impact of transmission type in fuel efficiency in a set of 32 cars (early 70's models). We have 13 manual transmission and 19 automatic transmission cars in the sample. A first simple analysis considering only transmission seems to indicate that manual cars are more efficient than automatic transmission cars.
We have carried out a deeper analysis to determine whether transmission is relevant. It could be the case that the difference is not due to transmission but due to another underlying variable. We have used different linear models and we hace chosen the one that seems to better represent the data. We still find transmission to be a relevant factor, and that manula transmission cars are more efficient.

## Data analysis

We have the following information for different car models in the file:

 * mpg: Miles per US gallon
 * cyl: Number of cylinders
 * disp: Displacement (cubic inches)
 * hp: Gross horsepower
 * drat: Rear axle ratio
 * wt: Weight (lb / 1000)
 * qsec: 1 / 4 mile time
 * vs: V/S
 * am: Transmission (0 = automatic, 1 = manual)
 * gear: Number of forward gears
 * carb: Number of carburetors

A first approach could be to see what is the mpg for cars with automatic and manual transmission.
```{r}
boxplot(mtcars$mpg[mtcars$am==0],mtcars$mpg[mtcars$am==1])

```

We see that cars with automatic transmission (left boxplot) tend to have lower mpg than manual transmission cars (right boxplot). 

However, a deeper analysis is needed. There are other factors that have a more clear relation with mpg such as weight. It could be the case that cars with automatic transmission are heavier, being that the cause of the observed behavior. 

As a first step, we will perform a linear regression of mpg to the rest of car characteristics.
```{r}
lm1<-lm(mpg~.,mtcars)
summary(lm1)

```

We see that the model explains 80.66% of the total mpg variance (this is the value of the adjusted R-squared). However, none of the coefficients is significant at 5% level. 

We need a more parsimoniuos model, with less predictors. The next step is to identify the most relevant magnitudes. To do so we resort to information criteria. The idea is to minimize the distance of the points to our model but penalizing the presence of new predictors. The most common one is Akaike information criteria, which is R can is available in the step function:
```{r include=FALSE, cache=FALSE}
AIC<-step(lm1,k=log(length(mtcars$mpg)))

```
```{r include=FALSE, cache=FALSE}
AIC<-step(lm1,k=log(length(mtcars$mpg)))
summary(AIC)

```

We see that the model that the most relevant magnitudes in mpg are weight, transmission and 1/4 mille time. All parameters are significant at 5% level. With that model we describe 83.36% of the variance. We see that transmission is a relevant magnitude, and manual transmission cars seems to be more efficient than manual ones. From model coefficients we see that two cars with same weight and 1/4 mille time but different transmission will have different mpg, being that number higher for manual transmission. So we conclude that there is evidence that cars with manual tranmission are more efficient.
 
### Residual analysis

We will analyze model residuals:

```{r}
lm3<-lm(mpg~wt+qsec+am,mtcars)
par(mfrow=c(2,2))
plot(lm3)

```

We see that Chrysler Imperial, Fiat 128 and Toyota Corolla are the most outlying points and contribute to differ from normality in upper tail. The fact that the sample is not large (32 cars) does not help to check normality with Q-Q plot.


### Conclusions

We have fitted several lienar models to the data and have choosen the one that presents a better compromise between data description and low parameters. We see that the relevant magnitudes for fuel efficiency are weight, 1/4 mille time and transmission, being all of them significant. We conclude that manula transmission cars are more efficient.
 
