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
4. Compute the adjusted Y′i=Yi−(Xi−μX)cov(X,Y)var(X) for each user.
5. Evaluate the A/B test using Y′ instead of Y

The point of CUPED is, when using Y′ instead of Y, the estimate of the treatment lift will have lower variance, ie. on average we will be better at estimating the lift.&#x20;

Mathematically, the decrease in variance will be var(Y′)/var(Y)=1−corr(X,Y)^2, where corr(X,Y)=cov(X,Y)/ (√var(X)var(Y)) is the correlation between X and Y.



## Reference

{% embed url="https://bytepawn.com/reducing-variance-in-ab-testing-with-cuped.html#reducing-variance-in-ab-testing-with-cuped" %}

