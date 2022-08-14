---
description: Reducing variance in A/B testing with CUPED
---

# Improving the Sensitivity of Online Controlled Experiment

Assume an A/B testing setup where we're measuring a metric M, eg. $ spend per user. We have N users, randomly split into A and B. A is control, B is treatment. We have metric M for each user for the "before" time period, when treatment and control was the same, and the "after" period, when treatment had some treatment applied, which we hope increased their spend.

Let Yi be the ith user's spend in the "after" period, and Xi be their spend in the "before" period, both for A and B combined. We compute an adjusted "after" spend Y′i.

The CUPED recipe:

1. Compute the covariance cov(X,Y) of X and Y.
2. Compute the variance var(X) of X.
3. Compute the mean μX of X.
4. Compute the adjusted Y′i=Yi−(Xi−μX)\*cov(X,Y)/var(X) for each user.
5. Evaluate the A/B test using Y′ instead of Y

The point of CUPED is, when using Y′ instead of Y, the estimate of the treatment lift will have lower variance, ie. on average we will be better at estimating the lift.&#x20;

Mathematically, the decrease in variance will be var(Y′)/var(Y)=1−corr(X,Y)^2, where corr(X,Y)=cov(X,Y)/ (√var(X)var(Y)) is the correlation between X and Y.

#### missing pre-experiment data

For users who are in the experiment but not in the pre-experiment period, the corresponding covariates are not well-defined. One way to address this is to define another covariate that indicates whether or not a user appeared in the pre-experiment period.

Or if Xi missing, let Y′i = Y. If Xi not missing, Y′i=Yi−(Xi−μX)\*cov(X,Y)/var(X).

#### other covariates

Covariates based on pre-experiment data are not the only choice. All that is required is that the covariate X is not affected by the experiment’s treatment. A natural extension is the class of covariates constructed using information gathered at the first time a user appears in the experiment. For instance, the day-of-week a user is first observed in the experiment is independent of the experiment itself. Such covariates can serve as an additional powerful source for variance reduction. To further extend this idea, covariates based on any information estab- lished before a user actually triggers the experiment feature are also valid. This can be particularly helpful if the feature to be evaluated has a low triggering rate.

#### Other findings

1. adding more control covariates (B) may not increase the var reduction amount a lot because the partial correlation between after-A and pre-B given pre-A is low.
2. the variance reduction rate for the pre-experiment covariate is not mono- tonically increasing as the experiment duration increases: it reaches the maximum at some time.
3. Correlation. The higher the correlation, the better the variance reduction. When we increase the pre-experiment period length or increase the experiment duration, the correlation increases for “cumulative” metrics because the longer the period, the higher the signal-to-noise ratio.&#x20;
4. Coverage. Coverage is the percentage of users in the experiment that also appeared in the pre-experiment period. Coverage is determined by: – Experiment Duration. As the experiment duration increases, coverage decreases. The reason is that frequent visitors are seen early in the experiment and users seen later in the experiment are often new or “churned” users. The result is that as coverage decreases, the rate of variance reduction goes down. – Pre-period Duration. Increasing the pre-period length increases coverage because we have a better chance of matching an experiment user in the pre-period.

### Background

One challenge with any controlled experiment is the ability to detect the treatment effect when it indeed exists, usually referred to as “power” or “sensitivity.” Improving sensitivity is particularly important when running online experiments at large scale. A mature online experimentation platform runs thousands of experiments a year. The benefit of any increased sensitivity is therefore amplified by economies of scale.&#x20;

In reality, even with a large amount of traffic, online experiments cannot always reach enough statistical power. Google made it very clear that they are not satisfied with the amount of traffic they have even with over 10 billion searches per month (comScore 2012). There are several reasons for this.&#x20;

First, the treatment effects we would like to detect tend to be very small. The sensitivity of controlled experiments is inversely proportional to the number of users squared, so whereas a small site may need 10,000 users to detect a 5% delta, detecting a 0.5% delta requires 100 times (10 squared) more users, or one million users. Even a 0.5% change in revenue per user equates to millions of dollars for large online sites.&#x20;

Second, it is crucial to get results fast. One wants to launch good features early, and more importantly, if the treatment turns out to have a negative impact on users, we need to stop it as soon as possible.&#x20;

Third, only a small fraction of the experiment’s users actually experience the treatment feature. For example, in an experiment affecting only recipe-related queries, the majority of the users will not see the target feature because they didn’t search for recipes during the experiment. In these cases, the effective sample size can be small and statistical analysis can suffer from low statistical power.&#x20;

Finally, in a data-driven culture, there is always demand to run more experiments to keep up with the rate of innovation. A good online experimentation platform should allow many experiments to run together. This also requires that we make optimal use of large but still limited traffic.



## Reference

{% embed url="https://bytepawn.com/reducing-variance-in-ab-testing-with-cuped.html#reducing-variance-in-ab-testing-with-cuped" %}

