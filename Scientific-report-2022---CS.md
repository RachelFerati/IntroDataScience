Causes of deaths by age in Switzerland
================
Rachel Ferati
2022-12-14

-   <a href="#introduction" id="toc-introduction">Introduction</a>
    -   <a href="#research-question" id="toc-research-question">Research
        question</a>
    -   <a href="#hypothesis" id="toc-hypothesis">Hypothesis</a>
    -   <a href="#prediction" id="toc-prediction">Prediction</a>
    -   <a href="#dataset" id="toc-dataset">Dataset</a>
-   <a href="#the-three-main-causes-of-deaths-in-switzerland-in-2019"
    id="toc-the-three-main-causes-of-deaths-in-switzerland-in-2019">The
    three main causes of deaths in Switzerland in 2019</a>
    -   <a href="#statistical-tests" id="toc-statistical-tests">Statistical
        tests</a>
-   <a href="#causes-of-deaths-among-infants-0-19-years-old"
    id="toc-causes-of-deaths-among-infants-0-19-years-old">Causes of deaths
    among infants (0-19 years-old)</a>
    -   <a href="#causes-of-deaths-between-ages-in-children"
        id="toc-causes-of-deaths-between-ages-in-children">Causes of deaths
        between ages in children</a>
-   <a href="#causes-of-deaths-among-adults"
    id="toc-causes-of-deaths-among-adults">Causes of deaths among adults</a>
-   <a href="#conclusion" id="toc-conclusion">Conclusion</a>
-   <a href="#references" id="toc-references">References</a>

This report uses the R programming language and the following R
libraries

``` r
library(tidyverse)
library(knitr)
library(ggplot2)
library(dplyr)
library(treemap)
library(hrbrthemes)
```

![](Deathfamily.jpeg)

# Introduction

*How Am I going to die* ? We all one day ask ourselves this question.
And, of course, this question is often followed by *When Am I going to
die ?*. The way we are going to die. Although these questions cannot be
answered, it is important to identify the number of deaths and their
causes to have a better understanding of the situation in a country.

> > > An important input to national and international health
> > > decision-making and planning processes is a consistent and
> > > comparative analysis of the causes of death across different
> > > population groups (Mathers, C.B., Boerma T., Ma Fat D. (2009))

Indeed, the causes of death differ from one country to another, and also
from one person to another. It depends on many factors, i.e., the
social-economic environment, the age, the family, etc. It is important
for a family to know which disease occurs whithin the family, and it is
important for the country to know which are the causes of deaths of the
population. To know that, there are a lot of database that exist for
many countries. The one from which we extracted the data needed for our
essay comes from the [World Health
Organization](@https://www.who.int/data/gho/data/themes/mortality-and-global-health-estimates/ghe-leading-causes-of-death).

> > > Over the last decade, the WHO has intensified efforts to support
> > > the collection of vital registration information and other
> > > mortality data in developing countries (Mathers, C.B., Boerma T.,
> > > Ma Fat D. (2009))

## Research question

In this essay, we will be interested in the differences between the
three leading causes of death across ages, and more precisely between
children and adults. We will less focused on the number of death in
itself, since it is not surprising that more people died when they get
older, but rather on the type of causes, whether it is communicable,
meaning that it is transmitted to one another (genetic illnesses),
non-communicable and injuries, meaning the causes that comes from the
external world (car accident), or from ourselves. Our data set will
concern only Switzerland’s population in 2019. The difference between
this country and the others could be the subject of another scientific
report.

Our research question is the following : **Is there a difference in the
type of deaths between children and adults ?**

## Hypothesis

-   H0 : there is no differences in the type of deaths between children
    and adults in Switzerland in 2019

-   H1 : There is a differences in the type of deaths between children
    and adults in Switzerland in 2019

## Prediction

Young adults and adults died mostly from injuries, compared to children
and seniors, who died mostly from non-communicable and communicable
diseases.

## Dataset

The dataset for this scientific report comes from

``` r
library(readxl)
Deathstat <- read_excel("~/UNINE/UniNE/Master en Sciences cognitives/Data science/Data - Scientific report/Final Scientific report/Deathstat.xlsx")
```

Here is a view of the variables used in this scientific report.

``` r
str(Deathstat)
```

    ## tibble [60 × 8] (S3: tbl_df/tbl/data.frame)
    ##  $ Continent: chr [1:60] "Europe" "Europe" "Europe" "Europe" ...
    ##  $ Country  : chr [1:60] "Switzerland" "Switzerland" "Switzerland" "Switzerland" ...
    ##  $ Year     : num [1:60] 2019 2019 2019 2019 2019 ...
    ##  $ Age      : chr [1:60] "0-85+" "0-85+" "0-85+" "0-1" ...
    ##  $ Rank     : num [1:60] 1 2 3 1 2 3 1 2 3 1 ...
    ##  $ Cause    : chr [1:60] "Ischaemic heart disease" "Alzheimer disease and other dementias" "Stroke" "Neonatal conditions" ...
    ##  $ Nb       : num [1:60] 124.7 83.7 45.6 167.6 127 ...
    ##  $ Category : chr [1:60] "Non-communicable" "Non-communicable" "Non-communicable" "Communicable" ...

# The three main causes of deaths in Switzerland in 2019

What are the three main causes of deaths in Switzerland in 2019 across
all ages (0-85+) ?

``` r
DeathAll <-Deathstat[1:3,]

ggplot(DeathAll, aes(x=fct_reorder(Cause, Nb), y=Nb, fill=Category,width = 0.5)) +
  geom_bar(stat = "identity") +
  xlab("Causes of death") + 
  ylab("Number of death per 100'000 people (0-85+)") +
  coord_flip() +
  scale_fill_manual(values = c("Lightblue"))
```

![](Scientific-report-2022---CS_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

In this graph, we can see that the three most common deaths for all
groups are ischaemic heart disease (cause n°1), Alzheimer disease and
other dementia (cause n°2), and stroke (cause n°3). All of these causes
are considered “non-communicable”.

What is this telling us ? First of all, let’s take a look at the number
of death per 100’000 people between ages :

``` r
DeathADCH <- Deathstat[4:60,]

ggplot(DeathADCH, aes(x=fct_relevel(Age, c("5-9"), after = 2), y=Nb)) +
  geom_point(color="Lightblue", size=4, alpha=0.6) + 
  geom_segment(aes(x=Age, xend=Age, y=0, yend=Nb)) +
  theme_light() +
  coord_flip() +
  xlab("Age groups") +
  ylab("Number of death per 100'000 people (0-85+)") +
  theme(
    panel.grid.major.y = element_blank(),
    panel.border = element_blank(),
    axis.ticks.y = element_blank())
```

![](Scientific-report-2022---CS_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

What we can see is that the number of death per 100’000 people is quite
high for children between 0 and 1 years-old, compared to people between
1 and 49 years-old. Then, the number increase from 50 years-old and
reach the highest for the 85 years-old and more.

That explains why the three main causes of death across all ages in
Switzerland in 2019 is composed only of non-communicable diseases.

### Statistical tests

``` r
TestLM <- lm(Nb~Age, data = Deathstat)
summary(TestLM)
```

    ## 
    ## Call:
    ## lm(formula = Nb ~ Age, data = Deathstat)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1017.70    -2.15    -0.14     4.28   754.04 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   102.22     121.81   0.839   0.4064    
    ## Age0-85+      -17.55     172.27  -0.102   0.9193    
    ## Age1-4       -100.67     172.27  -0.584   0.5623    
    ## Age10-14     -101.46     172.27  -0.589   0.5592    
    ## Age15-19      -99.36     172.27  -0.577   0.5673    
    ## Age20-24      -98.22     172.27  -0.570   0.5717    
    ## Age25-29      -97.73     172.27  -0.567   0.5737    
    ## Age30-34      -98.01     172.27  -0.569   0.5726    
    ## Age35-39      -96.73     172.27  -0.562   0.5776    
    ## Age40-44      -95.21     172.27  -0.553   0.5836    
    ## Age45-49      -90.52     172.27  -0.525   0.6022    
    ## Age5-9       -101.57     172.27  -0.590   0.5588    
    ## Age50-54      -82.03     172.27  -0.476   0.6365    
    ## Age55-59      -67.09     172.27  -0.389   0.6990    
    ## Age60-64      -45.76     172.27  -0.266   0.7919    
    ## Age65-69      -16.82     172.27  -0.098   0.9227    
    ## Age70-74       30.52     172.27   0.177   0.8603    
    ## Age75-79      109.09     172.27   0.633   0.5302    
    ## Age80-84      405.04     172.27   2.351   0.0237 *  
    ## Age85+       1903.87     172.27  11.052 9.99e-14 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 211 on 40 degrees of freedom
    ## Multiple R-squared:  0.866,  Adjusted R-squared:  0.8023 
    ## F-statistic:  13.6 on 19 and 40 DF,  p-value: 6.188e-12

``` r
anova(TestLM)
```

    ## Analysis of Variance Table
    ## 
    ## Response: Nb
    ##           Df   Sum Sq Mean Sq F value    Pr(>F)    
    ## Age       19 11504105  605479  13.602 6.188e-12 ***
    ## Residuals 40  1780590   44515                      
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

``` r
# (F-statistic = 13.602, df = 19, 40), p-value < .001)
# The effect of age is significant
```

To have a better understanding of the different causes, we will now look
at the three main causes of death among children and young-adults, to
see, later, if there is a difference compared to adults.

# Causes of deaths among infants (0-19 years-old)

Here is a graph that present the type of death that we can find among
children between 0 and 19 years-old. This graph confirm our prediction
that children die mostly from communicable and non-communicable
diseases.

``` r
DeathInfant <-Deathstat[4:18,]

ggplot(DeathInfant, aes(x=Cause, y=Nb, fill=Category, width = 0.5)) +
  geom_bar(stat = "identity") +
  xlab("Causes of death") + 
  ylab("Number of death per 100'000 people (0-19)") +
  facet_wrap(~ Category) +
  theme(axis.text.x = element_text(color="Black", 
                           size=7, angle=90)) +
  scale_fill_manual(values = c("Lightpink", "Orange", "Lightblue"))
```

![](Scientific-report-2022---CS_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

However, we saw earlier that there are more deaths among the 0-1
years-old compared to the 1-19 years-old. What happen if we compared
causes of deaths among children between 1 and 19 years-old ?

## Causes of deaths between ages in children

``` r
DeathChild <- Deathstat[7:18,]

ggplot(DeathChild, aes(x=fct_relevel(Age, c("5-9"), after = 1), y=Nb, fill=Category, width=c(0.2))) +
  geom_bar(stat = "identity") +
  xlab("Age groups") + 
  ylab("Number of death per 100'000 people (1-19)") +
  scale_fill_manual(values = c("Orange", "Lightblue"))
```

![](Scientific-report-2022---CS_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

What we see in this graph confirms our hypothesis that children from 0
to 14 die mosly from communicable and non-communicable disease, and that
young adults mostly die from injuries. Now that we confirmed our
hypothesis with children and young-adults, lets take a look at the
causes of deaths among adults to see if there is a difference between
adults and children.

# Causes of deaths among adults

To have a better view, let’s take only adults between 20 and 69, given
the fact that there are more deaths after 75 year-old and what we see in
the begining, in is only non-communicable diseases :

``` r
DeathAdult2 <- Deathstat[19:48,]

ggplot(DeathAdult2, aes(x=Age, y=Nb, fill=Category, width=c(0.1))) +
  geom_bar(stat = "identity") +
  xlab("Age groups (20-69)") + 
  ylab("Number of death per 100'000 people") +
  scale_fill_manual(values = c("Orange", "Lightblue"))
```

![](Scientific-report-2022---CS_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

What we can see is that :

# Conclusion

Children between 0-14 died mosly of communicable disease. Children and
Adults between 15 - 49 die mosly of injuries. Then, people betweem 0-1
and between 40 and 85+ die mostly from non-communicable disease.

# References
