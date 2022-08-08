# Unequal Sample Size

You can run an experiment with an unequal allocation (e.g. 10–90) as long as you don’t modify the allocation while the experiment is running. However it will be less efficient than a 50–50 allocation—either your test will have less power, or you will need to run it longer to achieve a comparable result.&#x20;

With an 80/20 control/variant allocation, one risks less exposure to an inferior variant during the test, compared to a test of the same duration, but with equal 50/50 allocation.

With a 20/80 control/variant allocation, one usually aims to get more users to experience the tested variant during the test.

Some appear to be under the impression that by running a 20/80 control/variant test they can reach a conclusion faster, while others believe that running a 80/20 control/variant test means they can reach the same conclusion, but with less risk.

#### Do unequal sample sizes bias results?

We rely on randomized assignment to “spread out” the influence of confounding factors equally across variations, so that they do not influence our relative comparison of different or uplift between the variations. Even if the proportion of users assigned to each variation is unequal, randomized assignment still works as long as we don’t change the traffic split. You should never modify the traffic allocation mid-experiment, because this can introduce temporal bias into your results.&#x20;

#### Are unequal sample sizes efficient?&#x20;

So it is possible to run an experiment with a non 50–50 split, but is it advisable? If our goal is to achieve some predetermined risk profile as quickly as possible—then probably not.

![](.gitbook/assets/image.png)

So a 10–90 allocation would require **2.8x** as many total users to reach a similar outcome as a 50–50 split. We can understand why this is the case by looking at the formula for the standard error of the difference between two binomial proportions, which defines the width of our confidence intervals.  A lower standard error equals greater certainty. The overall term will decrease whenever we collect samples in either variation, increasing either n1 or n2. But there are diminishing returns as  increases. Suppose we’ve already collected 1000 samples in variation A, but only 100 samples in variation B. Collecting an additional 100 samples in A will only half of the term under the square root by 10%, whereas an additional 100 samples in B would cut that term in half.

**I want to find the right way to distribute variants (control/test) in A/B-test with the constraint of no more 10 percent of population in test group. The test is t-test for continuous metric. I can design in 2 different ways:**

1. Split control/test in 90/10 percent proportion, and calculate the evaluation metric on test (10% of population) and full control (90%).
2. Expose experiment on 20% of population, 10% on control and 10% on test. Other 80% will not take part in metric calculation.

![](<.gitbook/assets/Screen Shot 2022-08-06 at 11.55.09 PM.png>)

#### Ron

If the variances of the metric of interest for A and B are similar (typically the case), your test sensitivity will be dominated by the smaller sample size. Running equally-sized variants (A and B) is therefore optimal for variance reduction, and hence running 50/50% is the most efficient from a statistical power perspective.

From a practical perspective, the case for equal variants is strong due to the following:

1. Many implementations of web sites use LRU (least-recently-used) caches throughout the stack, so running unequal variants in an A/B test gives an inherent speed advantage to the larger variant, which is cached more often. Since speed has significant impact on user behavior, not taking this into account can lead to the wrong conclusions.
2. Many metrics have skewed distributions. In [Seven Rules of Thumb for Web Site Experimenters](http://bit.ly/expRulesOfThumb)

Rule #7, we noted that while Statistical books often claim that with n>30 distributions become “normal” because of the Central Limit Theorem, when you’re looking for the 2.5% on the two tails of the distribution, you need a lot more samples. The formula we suggested in the paper depends on the skewness of the distribution, and we showed that in real examples, we need up to 100K samples for metrics like revenue/user. If you’re running experiments with unequal sample sizes, the smaller of the two variants needs to have the minimum sample size to be normal enough for the classical t-test.\
An interesting observation is that if the two distributions are not normal, then there’s an advantage to having them be “similar” so that the delta (used in the t-test) is symmetric and might be close to normally distributed. If you have unequal sample sizes, then the distributions of the means will have different distributions and the delta may be skewed and the normality assumption violated.
