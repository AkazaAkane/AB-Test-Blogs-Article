# Chi-squared test/Fisher's exact test

## Chi-Square Test <a href="#769b" id="769b"></a>

The chi-squared test (for independence) is a statistical test to evaluate whether or not the distributions of two or more categorical variables — each variable has two or more possible values— are actually independence or homogenous (i.e. how the values are distributed on the two variables are relatively the same).

As the definition suggests, the data on which chi-square tests are used is a typical contingency table like one below.

![Figure 1: Example of a contingency table](https://miro.medium.com/max/1100/1\*hMue0pUBGOs8AyAL0iWtRA.png)

In the contingency table above, we have two variables in learning methods (visual and auditory), each has two possible values (pass the exam or fail).

A data set like this is often called an “R×C table,” where R is the number of rows and C is the number of columns. This is a 2×2 table (McDonald, 2014).

**Hypotheses to be tested**

In the standard form of chi-squared test, the null-alternative pair of hypotheses to be tested are as follows:

* **Null**: The variables are independent, meaning the value distributions on the variables are relatively the same
* **Alternative**: The variables are not independent, how the values are distributed depends on the variable

**Test statistics**

Given a contingency table, the test statistics of chi-square test is formulated as follows.

![Equation 1: Chi-square statistics](https://miro.medium.com/max/432/1\*d0-DHlU3\_7GM6yidHn-aTg.png)

where

![](https://miro.medium.com/max/844/1\*KpsskPVkLseQZ9owmjE3Pg.png)

Moreover,

![Equation 2: Formula to compute the expected value of cell i,j](https://miro.medium.com/max/938/1\*LThPnl1XzGh2H5pLXnrm4Q.png)

The test statistics in Equation 1 is known to approximate the chi-square distribution with degree of freedom (R-1)x(C-1) (Frost 2020). For example, a 2x2 contingency table like the one in Figure 1 implies (2–1)x(2–1) = 1 as its degree of freedom.

![Figure 2: Probability density function of several chi-square distributions with k: degree of freedom (Source: Wikipedia)](https://miro.medium.com/max/1400/0\*-DDvva\_8CLaOVbxQ.png)



**Compare test statistics to table value**

After we compute the test statistics, we compare it with the table value. Precisely, our table value is

![](https://miro.medium.com/max/190/1\*f\_tATkWmMQvYK8XrIYOU-Q.png)

where _k_ and alpha are the degree of freedom and our predefined significance level, respectively.

If we find our test statistics is greater than the above table value, we can confidently reject our null hypothesis. That is, we conclude that the variables are not independent, how the values are distributed depends on the variable at the given significance level.

#### Other issues

First, regarding chi-square test. The chi-squared test is only valid if the sample size is _relatively large_, i.e. > 1000 (McDonald, 2014). If this threshold is not met, the test result might be not reliable. In such cases, one can use Fisher’s exact test instead.

Second, there is another type of chi-square test, other than the independence/homogeneity test we discussed in this article. The counterpart is called chi-square test for goodness of fit. Briefly speaking, the test is used when we want to test whether or not the distribution of a categorical variable follows a specific (given/assumed) distribution.

Third, regarding the metrics of A/B testing. We can use other metrics that aren’t based on proportions (such as redemption rate in this article). Depending on the problem at hand, we might want to evaluate some numeric-continuous metrics (for instance: average transaction amount) through A/B testing. For the consequence, we need to resort to different statistical technique to analyze such experiments.

Finally, we can generalize A/B testing to include more than two non-control variants (a.k.a. **multivariate A/B testing**). Again, it results in a (slightly) different statistical method used to draw conclusions from them.
