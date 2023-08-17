Logistic regression
================
Author: Cian Mac Liatháin, ID: 11476078
2023-08-17

1.  (35 Marks). In a staff promotion round it has been agreed that out
    of the 20 eligible staff, 8 will be promoted. The following 2 x 2
    table shows the results of the promotions by gender.

``` r
promotions<-data.frame(c(6,2,8), c(6,6,12), c(12,8,20))
colnames(promotions) <-c("Promoted", "Not Promoted", "Total")
rownames(promotions) <- c("Male", "Female", "Total")
promotions
```

    ##        Promoted Not Promoted Total
    ## Male          6            6    12
    ## Female        2            6     8
    ## Total         8           12    20

Compare the promotion rates for males and females using the following
measures and state what the relevant null and alternative hypotheses
would be. You do not need to perform the test, simply calculate the test
statistics.

- Equality of promoted proportions;

$H_0$: $\pi_m$ - $\pi_f$ = 0

$H_1$: $\pi_m$ - $\pi_f$ $\neq$ 0

$p_m$ = $\frac{6}{12}$ = 0.5 $p_f$ = $\frac{2}{8}$ = 0.25

The difference in proportions of promotions in males and females is
0.5 - 0.25 = 0.25

- Relative Risk (of promotion);

$H_0$: $\frac{\pi_m}{\pi_f}$ = 1

$H_1$: $\frac{\pi_m}{\pi_f}$ $\neq$ 0

$p_m$ = $\frac{6}{12}$ = 0.5 $p_f$ = $\frac{2}{8}$ = 0.25

The relative risk (of promotion) is $\frac{0.5}{0.25} = 2$, promotion is
seen twice as often in males compared to females in the sample.

- Odds-ratio.

$H_0$: Odds-ratio =
$\frac{\frac{\pi_m}{1 - \pi_m}}{\frac{\pi_f}{1 - \pi_f}}$ = 1

$H_1$: Odds-ratio =
$\frac{\frac{\pi_m}{1 - \pi_m}}{\frac{\pi_f}{1 - \pi_f}}$ $\neq$ 1

$Odds_m = \frac{6}{6} = 1$

$Odds_f = \frac{2}{6} = 0.33$

Odds-ratio $= \frac{1}{0.33} = 3$

The odds of promotion is three times higher in males than females in the
sample.

What tables (with the same margins) would constitute stronger evidence
of a gender bias effect in the calculation of p-value using Fisher’s
exact test.

``` r
promotions2 <-data.frame(c(8,0,8), c(4,8,12), c(12,8,20))
colnames(promotions2) <-c("Promoted", "Not Promoted", "Total")
rownames(promotions2) <- c("Male", "Female", "Total")
promotions2
```

    ##        Promoted Not Promoted Total
    ## Male          8            4    12
    ## Female        0            8     8
    ## Total         8           12    20

``` r
promotions3 <- data.frame(c(7,1,8), c(5,7,12), c(12,8,20))
colnames(promotions3) <-c("Promoted", "Not Promoted", "Total")
rownames(promotions3) <- c("Male", "Female", "Total")
promotions3
```

    ##        Promoted Not Promoted Total
    ## Male          7            5    12
    ## Female        1            7     8
    ## Total         8           12    20

Use R to perform Fisher’s exact test. Discuss whether the test should be
one-sided or two-sided.

``` r
transposedPromotions <- t(promotions[1:2,1:2])
transposedPromotions
```

    ##              Male Female
    ## Promoted        6      2
    ## Not Promoted    6      6

``` r
fisher.test(transposedPromotions, alternative = "greater")
```

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  transposedPromotions
    ## p-value = 0.2596
    ## alternative hypothesis: true odds ratio is greater than 1
    ## 95 percent confidence interval:
    ##  0.4173146       Inf
    ## sample estimates:
    ## odds ratio 
    ##   2.838407

The one-sided test has $H_1$: Odds-Ratio \> 1 which in this example is
looking at tables where males are promoted more often than the data.

``` r
fisher.test(transposedPromotions)
```

    ## 
    ##  Fisher's Exact Test for Count Data
    ## 
    ## data:  transposedPromotions
    ## p-value = 0.3729
    ## alternative hypothesis: true odds ratio is not equal to 1
    ## 95 percent confidence interval:
    ##   0.3166041 40.1965603
    ## sample estimates:
    ## odds ratio 
    ##   2.838407

The two-sided test has $H_1$: Odds-ratio $\neq$ 1. In this example, the
null hypothesis would be rejected if either females are promoted more
often than males or males are promoted more often than females.

A known bias to investigate isn’t stated in the research question so the
two-sided test might be most appropriate to include possible bias in
favour of females.

2.  (50 Marks). A study involved 51 untreated adult patients with acute
    myeloblastic leukemia who were given a course of treatment, after
    which they were assessed as to their response. The variables
    recorded in the dataset Leukemia are:

``` r
library(Stat2Data)
data(Leukemia)
```

- Age: Age at diagnosis (in years)
- Smear: Differential percentage of blasts
- Infil: Percentage of absolute marrow leukemia infiltrate
- Index: Percentage labeling index of the bone marrow leukemia cells
- Blasts: Absolute number of blasts, in thousands
- Temp: Highest temperature of the patient prior to treatment, in
  degrees Farenheit
- Resp: 1 = responded to treatment or 0 = failed to respond
- Time: Survival time from diagnosis (in months)
- Status: 0 =dead or 1 =alive

1)  Fit a logistic model using Resp as the response and Age as the
    predictor variable. Interpret the results and state whether the
    relationship is statistically significant.

``` r
model <- glm(Resp ~ Age, family = binomial(link = "logit"), data = Leukemia)
summary(model)
```

    ## 
    ## Call:
    ## glm(formula = Resp ~ Age, family = binomial(link = "logit"), 
    ##     data = Leukemia)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)  2.19678    1.00548   2.185   0.0289 *
    ## Age         -0.04676    0.01952  -2.395   0.0166 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 70.524  on 50  degrees of freedom
    ## Residual deviance: 64.004  on 49  degrees of freedom
    ## AIC: 68.004
    ## 
    ## Number of Fisher Scoring iterations: 4

We can see a statistically significant p-value of 0.016, we reject the
null hypothesis, there is evidence to suggest an association between Age
and whether patients respond to treatment. The Odds-Ratio of Age is
0.95, suggesting that for each year of age a patient’s odds of
responding to treatment decreases by 5%.

2)  Set up a new variable by categorising Age into 3 groups (\<30;
    30-60; \>60) and form a two-way table that exhibits the nature of
    the relationship found in a).

``` r
Leukemia %>% mutate(age_group = case_when(Age<30~"<30", Age>=30&Age<=60 ~"30-60", Age>=60 ~">60")) %>% group_by(Resp,age_group) %>% count(Resp) %>%pivot_wider(names_from=age_group, values_from=n) %>% relocate('30-60', .after = '<30')
```

    ## # A tibble: 2 × 4
    ## # Groups:   Resp [2]
    ##    Resp `<30` `30-60` `>60`
    ##   <int> <int>   <int> <int>
    ## 1     0     1      16    10
    ## 2     1     7      12     5

After categorising Age into 3 groups we can see more accurate Odds-Ratio
for each group.

\<30: OR = $\frac{7}{1}$ = 7

30-60: OR = $\frac{12}{16}$ = 0.75

\$\>\$60: OR = $\frac{5}{10}$ = 0.5

It is evident that younger people are more likely to respond from the
treatment.

3)  Redo parts a) and b) using the Temp variable as the single
    predictor, taking suitable cutpoints to categorise Temp.

``` r
model <- glm(Resp ~ Temp, family = binomial(link = "logit"), data = Leukemia)
summary(model)
```

    ## 
    ## Call:
    ## glm(formula = Resp ~ Temp, family = binomial(link = "logit"), 
    ##     data = Leukemia)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept) 38.55067   21.23644   1.815   0.0695 .
    ## Temp        -0.03884    0.02135  -1.819   0.0689 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 70.524  on 50  degrees of freedom
    ## Residual deviance: 66.766  on 49  degrees of freedom
    ## AIC: 70.766
    ## 
    ## Number of Fisher Scoring iterations: 4

With a p-value of 0.0689 \> 0.05, we cannot reject the null hypothesis,
there is no evidence to suggest an association between temperature. The
Odds-Ratio for Temp is 0.96, suggesting that for each degree farhenheit
of temperature the patient’s odds of responding to treatment decreases
by 4%.

``` r
Leukemia %>% mutate(temp_group = case_when(Temp<985~'<985', Temp>=985&Temp<=995 ~'985-995', Temp>=995 ~'>995')) %>% group_by(Resp,temp_group) %>% count(Resp) %>%pivot_wider(names_from=temp_group, values_from=n) %>% relocate('>995', .after='985-995')
```

    ## # A tibble: 2 × 4
    ## # Groups:   Resp [2]
    ##    Resp `985-995` `>995` `<985`
    ##   <int>     <int>  <int>  <int>
    ## 1     0        10     13      4
    ## 2     1        12      6      6

\<985: OR = $\frac{6}{4}$ = 1.5

985-995: OR = $\frac{12}{10}$ = 1.2

\$\>\$995: OR = $\frac{6}{13}$ = 0.5

We can see that the odds of a patient responding to treatment decreases
as their temperature rises.

The first six variables (Age, Smear, Infil, Index, Blasts, Temp) were
all measured pre-treatment. Fit a multiple logistic regression model
using all six of these variables to predict Resp.

``` r
model1 <-  glm(formula = Resp ~ Age + Smear + Infil + Index + Blasts + Temp, family = binomial(link = "logit"), data = Leukemia)
summary(model1)
```

    ## 
    ## Call:
    ## glm(formula = Resp ~ Age + Smear + Infil + Index + Blasts + Temp, 
    ##     family = binomial(link = "logit"), data = Leukemia)
    ## 
    ## Coefficients:
    ##              Estimate Std. Error z value Pr(>|z|)   
    ## (Intercept) 108.33115   41.84379   2.589  0.00963 **
    ## Age          -0.06231    0.02746  -2.269  0.02327 * 
    ## Smear        -0.00469    0.04005  -0.117  0.90677   
    ## Infil         0.03104    0.03789   0.819  0.41264   
    ## Index         0.37281    0.13247   2.814  0.00489 **
    ## Blasts        0.03267    0.04605   0.710  0.47801   
    ## Temp         -0.11162    0.04263  -2.618  0.00884 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 70.524  on 50  degrees of freedom
    ## Residual deviance: 39.275  on 44  degrees of freedom
    ## AIC: 53.275
    ## 
    ## Number of Fisher Scoring iterations: 6

4)  Based on the output from the model, which of the six pretreatment
    variables appear to add to the predictive power of the model, given
    that the other variable are included? (Use Wald tests of the
    individual coefficients.)

We can see from this model that the p-values of the Wald tests of Age,
Index and Temp are statistically significant (\<0.05), there is evidence
to suggest an association between these variables and whether a patient
responds to treatment.

5)  Interpret the relationship (if any) between Age and Resp and also
    between Temp and Resp indicated in this multiple model.

There is a negative relationship between both Age and Resp and Temp and
Resp. The p-values for both variables is very small, even in this model
which controls for the other variables.

The odds-ratio for Age is 0.94, this suggests that for every year of age
the odds that a patient responds to treatment decreases by 6%.

The odds-ratio for Temp is 0.89, this suggests that for every degree
farhenheit of temperature the odds that a patient responds to treatment
decreases by 11%.

6)  If a predictor variable is insignificant in the fitted model here,
    might it still be possible that it should be included in a final
    model. Explain why or why not.

Yes, it is still possible that they should be included. The other
variables being included in the model, even if they do not have a
statistically significant p-value themselves, account for variability in
the response variable and the combination allows for more accurate
understanding of other predictor variables.

7)  Use the change in Deviance values to perform a test to see if a
    model that excludes all of the nonsignificant variables from the
    model in d) is a reasonable choice for the final model. Also comment
    on the stability of the estimated coefficients between the full
    model in d) and the reduced model without the “nonsignificant”
    terms.

``` r
model2 <- glm(formula = Resp ~ Age + Index + Temp, family = binomial(link = "logit"), data = Leukemia)
summary(model)
```

    ## 
    ## Call:
    ## glm(formula = Resp ~ Temp, family = binomial(link = "logit"), 
    ##     data = Leukemia)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept) 38.55067   21.23644   1.815   0.0695 .
    ## Temp        -0.03884    0.02135  -1.819   0.0689 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 70.524  on 50  degrees of freedom
    ## Residual deviance: 66.766  on 49  degrees of freedom
    ## AIC: 70.766
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
model2$deviance - model1$deviance
```

    ## [1] 3.990238

Comparing the two models, the model with variables Age, Smear, Infil,
Index, Blasts and Temp has 3 more predictor variables than second with
Age and Temp and these 3 extra predictors lower the deviance by 3.99.

``` r
pchisq(3.99, df = 3, lower.tail = FALSE)
```

    ## [1] 0.262546

Using a chi-squared distribution with 3 degrees of freedom, we find a
large p-value of 0.26. There is no evidence to reject the null
hypothesis, no evidence to suggest that the model with more variables is
preferred to the simpler model.

8)  Are the estimated coefficients for Age and Temp in your chosen model
    consistent with those found in a) and c)?

Yes, the coefficients of both Age and Temp are negative in the models
a), c) and d).

3.  (15 Marks):

<!-- -->

1)  What is the assumption of linearity in a logistic regression model?
    How can this be checked?

There should be a linear relationship between the predictor and logits.

This can be checked visually using an empirical logit plot.

2)  A multiple logistic regression model fit in R gives a Deviance =
    3.219 on 3 degrees of freedom for the test that all slopes are zero.

<!-- -->

1.  How many explanatory variables are there in the model?

3 explanatory variables.

2.  What is the p-value for the Deviance-statistic?

``` r
  pchisq(3.219, df = 3, lower.tail = FALSE)
```

    ## [1] 0.3590764

The p-value of a multiple logistic regression model with Deviance =
3.219 on 3 degrees of freedom is large, p = 0.359. There is no evidence
to suggest this model improves over the model it is being compared to.
