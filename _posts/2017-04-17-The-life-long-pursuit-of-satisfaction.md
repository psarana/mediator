Will you become more self-satisfied with age?
================

**The back story**

"Life, liberty, and the pursuit of happiness" is one of the most well-known phrases from the United States Declaration of Independence. The phrase highlights a few of the common goals Americans may have - however, should happiness really be  one of *the* most central goals? Happiness relates to an optimistic state of mind. It goes without saying that being happy is preferable to being unhappy but happiness is something that is in need of constant effort. Happiness requires us to consistently take actions that can maintain our happiness but I would argue a greater goal would be one of striving for satisfaction. Happiness can be manipulated and stimulated whereas satisfaction is a sense of holistic fulfillment with oneself and/or with one's life. Satisfaction can also be the feeling of completing goals or obtaining some set achievements. Unlike happiness, satisfaction of achievements/goals is not fickle and does not fade. The purpose of life should not be to "feel" satisfaction but to continually strive for it.

If we switch our focus to continually striving for satisfaction rather than chasing happiness, can satisfaction be measured with age as one hopefully begins to collect achievements? This question became the basis of delving into an analysis of self-satisfaction as explained by age.

**The Data**

Looking at survey data from the USA General Social Survey (GSS), I attempted to do an in-depth analysis of the variables age and self-satisfaction.  The GSS is a  large scale, representative survey of American adults living in households and has been providing demographic and sociological data on a wide variety of topics since 1972[1](https://gssdataexplorer.norc.org).

In 2004, survey respondents were asked their level of agreement (strongly agree, agree, disagree and slightly disagree) with the statement: "On the whole, I am satisfied with myself".

As with any explanatory and response variable relationship, there are many possible cofounding variables also interacting. For this analysis aside from our response variable of satisfaction and the explanatory variable of age, the variables of  respondents income, education level, race and (separately) sex are considered.

**Methods**

![](https://github.com/psarana/psarana.github.io/blob/master/assets/images/Age_versus_SelfSatisfaction.jpg)

The data was explored first with visualization. By plotting age versus satisfaction which showed some sort of relation  that with as age increases, satisfaction initially increases but caps at approximately 40-50 where it drastically decreases.

![](https://github.com/psarana/psarana.github.io/blob/master/assets/images/Age_versus_SelfSatisfaction_BySex.jpg)

This visual trend did not change when the blocked groups of females and males were considered separately. However, visualizations can be misleading so the next step was to test if there was a  significant difference between the proportion of  females versus males who responded to the self-satisfaction question in 2004 using a chi-squared test.

The next analysis was with  a logistic regression model which modelled whether or not an individual is satisfied or not  as dependent variable and age, respondent's income, education and race as independent variables. Note that several models were considered and compared with an anova test before settling on the one used for analysis.

**Results**

Chi-Squared test

The chi-squared test showed no significant association between sex and self-satisfaction level and sex (p-value = 0.4437).

```
> chisq.test(dat$sat, dat$age)

Pearson's Chi-squared test

data:  dat$sat and dat$age
X-squared = 215.26, df = 213, p-value = 0.4437
```

Logistic regression

The logistic regression analysis holds that the association between age and self-satisfaction is non-significant (p-value = 0.7685) while controlling for  respondent income, education and race.

```
Call:
glm(formula = satself ~ (age) + rincome + educ + race, family = binomial,
    data = data)

Deviance Residuals:
    Min       1Q   Median       3Q      Max  
-1.8884  -1.3679   0.8844   0.9640   1.2160  

Coefficients:
                       Estimate Std. Error z value Pr(>|z|)  
(Intercept)            0.848900   0.481979   1.761   0.0782 .
age                   -0.001271   0.004320  -0.294   0.7685  
rincome$1000 to 2999   0.466359   0.468807   0.995   0.3198  
rincome$3000 to 3999   0.233476   0.553504   0.422   0.6732  
rincome$4000 to 4999   0.425365   0.508327   0.837   0.4027  
rincome$5000 to 5999   0.446983   0.614188   0.728   0.4668  
rincome$6000 to 6999   0.704026   0.632834   1.112   0.2659  
rincome$7000 to 7999   0.853918   0.622950   1.371   0.1704  
rincome$8000 to 9999   1.134266   0.572361   1.982   0.0475 *
rincome$10000 - 14999  0.577639   0.391522   1.475   0.1401  
rincome$15000 - 19999  0.864097   0.397112   2.176   0.0296 *
rincome$20000 - 24999  0.403048   0.380578   1.059   0.2896  
rincome$25000 or more  0.409660   0.349691   1.171   0.2414  
educ                  -0.043611   0.021022  -2.075   0.0380 *
raceblack             -0.222974   0.160713  -1.387   0.1653  
raceother             -0.255764   0.213306  -1.199   0.2305  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

(Dispersion parameter for binomial family taken to be 1)

    Null deviance: 1906.1  on 1463  degrees of freedom
Residual deviance: 1886.2  on 1448  degrees of freedom
  (911 observations deleted due to missingness)
AIC: 1918.2

Number of Fisher Scoring iterations: 4

```

Revisiting the concept addressed in the chi-squared test, sex is blocked into two homogenous groups of females and males where the above logistic regression is performed on each blocked group. Again, it was found that there was non-significant association between age and self-satisfaction even when the sex groups were divided into females and males.

**Discussion**

The results conclude that with the chosen linear regression model, there is no statistically significant association between age and self-satisfaction when respondent income, education and race are taken into consideration. It should be noted that these are not the only possible variables that play a role in self-satisfaction there are thousands of possible confounding relationships and these were merely chosen as the most probable based on research of previous studies. This does not suggest that under another probable model that self-satisfaction and age would not be statistically significant but it does stand that the relationship is not significant under association with known self-satisfaction components.

So it seems like age cannot be said to (statistically) suggest that one will become more self-satisfied over the years. This is actually most fitting as self-satisfaction is meant to be something continually strived so perhaps one can find solace in the process rather than the destination in this case. Life, liberty and the pursuit of self-satisfaction may be a life long journey.

References

"GSS Data Explorer | NORC At The University Of Chicago". Gssdataexplorer.norc.org. N.p., 2017. Web. 17 Apr. 2017.
