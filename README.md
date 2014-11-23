*Assignment completed as component of the [Statistical
Inference](https://www.coursera.org/course/statinference) course offered
by the Johns Hopkins Bloomberg School of Public Health through
Coursera.*

Overview of ToothGrow Dataset
=============================

The data used in the following analysis is from the `ToothGrow` dataset
in the default `datasets` package. The title of the dataset is *The
Effect of Vitamin C on Tooth Growth in Guinea Pigs*. The description of
the dataset, as quoted from the accompanying documentation is as
follows:

> *The response is the length of odontoblasts (teeth) in each of 10
> guinea pigs at each of three dose levels of Vitamin C (0.5, 1, and 2
> mg) with each of two delivery methods (orange juice or ascorbic
> acid).*

Basic Exploratory Data Analysis
===============================

![](README_files/figure-markdown_strict/graph-1.png) By charting out
violin plots for each dose and delivery method combination, we can
already start to consider the hypothesis that the dose level is
correlated with tooth length regardless of delivery method. We can also
see that the delivery method also has an impact, though the impact
becomes less pronounced as the dose level increases.

Confidence Intervals and Hypothesis Testing
===========================================

In order to assess if our insights drawn from our graphical analysis are
statistically valid, we perform T-Tests for the tooth length as the
outcome predicted by three separate vectors.

1.  Delivery method as the predictor
2.  Dose level as predictor
3.  Delivery method and dose as predictors

The approach will be outlined in the first t-test and replicated for
each additional test (albeit with code suppressed for readability).

Comparing outcomes across delivery methods
------------------------------------------

For this test, we ignore dosage and consider only the delivery method
(either orange juice and ascorbic acid) as a predictor of tooth length.
We consider the following hypotheses:

> *H*<sub>0</sub>: Tooth length is not affected by delivery method.

    t.test(data = ToothGrowth, len ~ supp, paired = F, var.equal = F)

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  len by supp
    ## t = 1.9153, df = 55.309, p-value = 0.06063
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.1710156  7.5710156
    ## sample estimates:
    ## mean in group OJ mean in group VC 
    ##         20.66333         16.96333

Because the p-value is greater than 0.05 and because the confidence
interval crosses 0 (-7.571, 0.171), we can not reject *H*<sub>0</sub>
within a 95% confidence interval. This indicated that there is not
strong evidence pointing to a significant difference in tooth length
between delivery methods.

Comparing outcomes across delivery methods
------------------------------------------

For this test, we ignore delivery methods and consider only the dose
level as a predictor of tooth length. We consider the following
hypothesis:

> *H*<sub>0</sub>: Tooth length is not affected by dose level

We assess this hypothesis by performing three t-tests, with tooth length
predicted by dose level, between each combination of dose levels (i.e.
0.5 ~ 1.0, 0.5 ~ 2.0, 1.0 ~ 2.0). The primary results from these tests
are summarized in the chart below.

    ## \begin{table}[ht]
    ## \centering
    ## \begin{tabular}{rlrrrr}
    ##   \hline
    ##  & dose.levels & t.statistic & p.value & conf.int.lower & conf.int.upper \\ 
    ##   \hline
    ## 1 & 0.5mg-1.0mg & -6.48 & 0.00 & -11.98 & -6.28 \\ 
    ##   2 & 0.5mg-2.0mg & -11.80 & 0.00 & -18.16 & -12.83 \\ 
    ##   3 & 1.0mg-2.0mg & -4.90 & 0.00 & -9.00 & -3.73 \\ 
    ##    \hline
    ## \end{tabular}
    ## \end{table}

Because the p-value is less than 0.05 and because the confidence
interval does not cross 0 for any of the combinations, we can reject
*H*<sub>0</sub> with at least a 95% confidence interval. This indicates
that there is strong evidence pointing to a significant difference in
tooth length between dose levels.

Comparing outcomes across delivery methods and dose levels
----------------------------------------------------------

Finally, we consider the effect of delivery method while also
considering dose level. For this, we consider the following hypotheses:

> *H*<sub>0</sub>: Tooth length is not affected by delivery method at a
> 0.5mg dose

> *H*<sub>1</sub>: Tooth length is not affected by delivery method at a
> 1.0mg dose

> *H*<sub>2</sub>: Tooth length is not affected by delivery method at a
> 2.0mg dose

We assess this hypothesis by performing three t-tests, with tooth length
predicted by delivery method level combination at each dose levels. The
primary results from these tests are summarized in the chart below.

    ## \begin{table}[ht]
    ## \centering
    ## \begin{tabular}{rlrrrr}
    ##   \hline
    ##  & dose.level & t.statistic & p.value & conf.int.lower & conf.int.upper \\ 
    ##   \hline
    ## 1 & 0.5mg & 3.17 & 0.01 & 1.72 & 8.78 \\ 
    ##   2 & 1.0mg & 4.03 & 0.00 & 2.80 & 9.06 \\ 
    ##   3 & 2.0mg & -0.05 & 0.96 & -3.80 & 3.64 \\ 
    ##    \hline
    ## \end{tabular}
    ## \end{table}

We can take away the following for this table:

-   For *H*<sub>0</sub> and *H*<sub>1</sub>: Because the p-value is less
    than 0.05 and because the confidence interval does not cross 0 for
    either test, we can reject *H*<sub>0</sub> and *H*<sub>1</sub> with
    at least a 95% confidence interval.
-   \*For *H*<sub>2</sub>: Because the p-value is greater than 0.05 and
    because the confidence interval crosses 0 (-7.571, 0.171), we can
    not reject the *H*<sub>2</sub>: within a 95% confidence interval.

Conclusions and Assumptions
===========================

Conclusions
-----------

Based on the insights drawn from the analysis performed, we can conclude
the following:

-   There is not strong evidence indicating that tooth length varies
    based on delivery methods independent of dose level ; however, there
    is evidence showing that it varies when the dose level is contolled
    at either 0.5mg or 1.0 mg.
-   There is strong evidence indicating that tooth length varies based
    on dose levels.

Assumptions
-----------

In order to arrive at these conclusions, we assume the following about
our data and analysis:

-   Populations of guinea pigs were independent (i.e. there were 60
    guinea pigs with similar tooth lengths which were each randomly
    assigned to a condition)
-   Variances across groups are NOT equal (as such, we set
    `var.equal = F` in our T Tests)
-   Acurate and consistent data collection and measurement methodologies
    were used in compiling the dataset.

Appendix I: Coding Reference
============================

Loading the Data
----------------

First, we load the data and set the `supp` column in the dataset as a
two-level factor variable, with a level representing Ascorbic Acid and
Orange Juice respectively.

    data(ToothGrowth)
    ToothGrowth$supp <- factor(ToothGrowth$supp, 
                                  levels = c("VC", "OJ"), 
                                  labels = c("Ascorbic Acid", "Orange Juice"))

Graph Analysis
--------------

    library(ggplot2)
    library(gtable)
    library(grid)
    library(dplyr)
    library(knitr)

    xlab <- "Delivery Method"
    ylab <- "Tooth Length (cm)"
    flab <- "Vitamin C Dose Level (mg)"
    g <- ggplot(data = ToothGrowth, aes(x = factor(supp), y = len, fill = supp)) + 
            scale_fill_discrete(name = xlab)
    g <- g + geom_violin(col = "black", size = 1.5) + facet_wrap(~ dose) + 
            scale_x_discrete(breaks=NULL)
    g <- g + xlab("") + ylab(ylab)
    z <- ggplotGrob(g)
    z <- gtable_add_rows(z, unit(1, "lines"), pos = 0)
    z <- gtable_add_grob(z, 
                  list(rectGrob(gp = gpar(col = NA, fill = gray(0.8), size = .5)),
                       textGrob(flab, vjust = .27, 
                                gp = gpar(cex = .75, fontface = "bold", 
                                          col = "black"))), 2, 4, 2, 10, 
                  name = c("a", "b"))
    z <- gtable_add_rows(z, unit(2/10, "line"), 2)
    grid.newpage()
    grid.draw(z)

Dose Comparison
---------------

    data(ToothGrowth)
    teeth_05_10 <- filter(ToothGrowth, dose == 0.5 | dose == 1.0)
    teeth_05_20 <- filter(ToothGrowth, dose == 0.5 | dose == 2.0)
    teeth_10_20 <- filter(ToothGrowth, dose == 1.0 | dose == 2.0)

    ttest_05_10 <- t.test(len ~ dose, data = teeth_05_10, paired = F, var.equal = F)
    ttest_05_20 <- t.test(len ~ dose, data = teeth_05_20, paired = F, var.equal = F)
    ttest_10_20 <- t.test(len ~ dose, data = teeth_10_20, paired = F, var.equal = F)

    library(xtable)
    sumtable <- data.frame(dose.levels = c("0.5mg-1.0mg", "0.5mg-2.0mg", 
                                          "1.0mg-2.0mg"), 
                           t.statistic = c(ttest_05_10$statistic, 
                                           ttest_05_20$statistic, 
                                           ttest_10_20$statistic), 
                           p.value = c(ttest_05_10$p.value, 
                                       ttest_05_20$p.value, 
                                       ttest_10_20$p.value), 
                           conf.int.lower = c(ttest_05_10$conf.int[1], 
                                              ttest_05_20$conf.int[1], 
                                              ttest_10_20$conf.int[1]), 
                           conf.int.upper = c(ttest_05_10$conf.int[2], 
                                              ttest_05_20$conf.int[2], 
                                              ttest_10_20$conf.int[2]))
    print(xtable(sumtable), comment=F)

Dose and Delivery Comparison
----------------------------

    data(ToothGrowth)
    teeth_05 <- filter(ToothGrowth, dose == 0.5)
    teeth_10 <- filter(ToothGrowth, dose == 1.0)
    teeth_20 <- filter(ToothGrowth, dose == 2.0)

    ttest_05 <- t.test(len ~ supp, data = teeth_05, paired = F, var.equal = F)
    ttest_10 <- t.test(len ~ supp, data = teeth_10, paired = F, var.equal = F)
    ttest_20 <- t.test(len ~ supp, data = teeth_20, paired = F, var.equal = F)
    sumtable <- data.frame(dose.level = c("0.5mg", "1.0mg", 
                                          "2.0mg"), 
                           t.statistic = c(ttest_05$statistic, 
                                           ttest_10$statistic, 
                                           ttest_20$statistic), 
                           p.value = c(ttest_05$p.value, 
                                       ttest_10$p.value, 
                                       ttest_20$p.value), 
                           conf.int.lower = c(ttest_05$conf.int[1], 
                                              ttest_10$conf.int[1], 
                                              ttest_20$conf.int[1]), 
                           conf.int.upper = c(ttest_05$conf.int[2], 
                                              ttest_10$conf.int[2], 
                                              ttest_20$conf.int[2]))
    print(xtable(sumtable), comment=F)
