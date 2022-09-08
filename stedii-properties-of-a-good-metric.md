# STEDII Properties of a Good Metric

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

## Sensitivity

**A sensitive metric has a high chance of detecting an effect when there is one. Conversely, when a sensitive metric that is well powered has no stat-sig movement, we have high confidence that there is no treatment effect.**&#x20;

Let $$𝐻1$$ be the alternative hypothesis – i.e., there is real treatment effect of a certain size. Then,

$$𝑃𝑟𝑜𝑏$$\[Detecting the treatment effect on the metric] = $$𝑃𝑟𝑜𝑏(𝐻1)𝑃𝑟𝑜𝑏(p-value<0.05|𝐻1)$$.

To measure the sensitivity of a metric, we can use a labeled corpus, consisting of tests in which there is high confidence that a treatment effect exists. That confidence is built by examining many metric movements to check if they align with the hypothesis of the test, deep dives, and offline analyses that add more evidence to the correctness of the hypotheses of the A/B tests. A sensitive metric will have a high proportion of stat-sig changes when there is an empirically known impact from the corpus. In cases where there is no labeled corpus, a more sensitive metric would have a higher proportion of overall tests where it is stat-sig. This will include no more than 5% false positive stat-sig tests, in the event that all tests in the corpus have no impact on the metric.

There are two components of metric sensitivity:

1. Movement Probability $$[𝑃𝑟𝑜𝑏(𝐻1)]$$: How often is the alternative hypothesis true?
2. Statistical Power $$[𝑃𝑟𝑜𝑏(p-value<0.05|𝐻1)]$$: Given that an alternative hypothesis is true, how likely is it that we are able to detect the effect?

### Movement Probability

**If the movement probability of a metric is low, the metric will rarely have a statistically significant movement, even though it may have very high statistical power**. In cases of well-optimized products, high-level metrics (such as “days active” and “sessions per user”) can be very difficult to improve in a short period of time, but they can regress very easily. So those metrics are poor indicators of success, but they are good guardrails. Proxy metrics with higher positive metric movement probability are better success metrics that we can aim to improve in an experiment. In other cases, we can improve metric design to measure more sensitive transformations of a metric, such as the “capped average of log of time spent in Teams,” instead of a simple average.

### **Statistical Power**

**Statistical power is the probability of detecting a stat-sig change if the alternative hypothesis is true**. We refer to the smallest magnitude of metric change detectable with high probability (usually 80%) as the Minimum Detectable Effect (MDE). The smaller the MDE, the higher the statistical power. If we assume that Treatment and Control have the same size and the same population variance, the relative change (expressed as a fraction of the control mean) that we can detect (using the t-test) with 80% power is approximately 4×_𝐶𝑉 /_ sample size. Here, _𝐶𝑉_ is the coefficient of variation, defined as the ratio of the standard deviation (_𝜎_) of a metric to its mean (_𝜇_), _𝐶𝑉_=_𝜎𝜇_. When designing a metric, we want to aim for a low _𝐶𝑉_ in order to get more statistical power.

## Reference

{% embed url="https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/stedii-properties-of-a-good-metric/" %}
