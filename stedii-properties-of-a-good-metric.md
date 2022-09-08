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

## **Trustworthiness**

**Metrics are not only used to make informed product decisions, but they also help determine the incentives and future investments of feature teams.** An untrustworthy metric can send a product down a wrong path and away from its aspired goal.&#x20;

### **Data Quality**

**When creating a metric, we should check for the following aspects related to data quality: missing data, invalid values, low join rates with other data sources, duplicate data, and delayed data**.&#x20;

\
We must check to ensure that the p-value distribution of a metric under multiple simulated AA tests is uniform. Moreover, we recommend regularly monitoring these aspects of key metrics through dashboards, anomaly detection methods, and AA tests in order to detect and resolve any regressions.

### **Alignment with the Goal and User Experience**

When a metric is aligned with a key product goal, it aggregates data from all observation units into a single number that is most pertinent for that goal. For example, a common goal for end-to-end performance of a product such as Page Load Time (PLT) is that it should be satisfactory for the large majority of the page load instances. The distribution of PLT is usually very skewed with a long tail. Average PLT is a poor metric to track the goal, but a metric like the 95th percentile or the 99th percentile of the PLT is more suitable. If that is hard to compute, then another option would be a metric which estimates the proportion of page loads where PLT exceeds a threshold. Further PLT is only useful if it is measuring the latency of users’ experiences when loading a page. For instance, some webpages load most of the content after the [standard page load event](https://www.w3.org/TR/navigation-timing/). In such cases, a good metric would measure the latency to the point where the page becomes functional for the end user.

**Often, the goal will be clear (e.g., “increase user satisfaction”), but it will be hard to determine a metric that actually aligns with that goal.** In those cases it is important to test those metrics on a corpus of past A/B tests that we can confidently label as good or bad in terms of the desired goal \[2]. In an upcoming blog post, we will discuss how to develop a trustworthy OEC that reflects user satisfaction.

### **Generalization**

A trustworthy metric provides an unbiased estimate for the entire population for which a product decision is being made. **The most important factor to look for in this case is selection bias in data generation, collection, transformation, and metric definition and interpretation.**

An example of a bias in data generation is “app ratings” in the App Store. If an app only sends users with a positive outlook to the App Store, then the data generated from user reviews will be biased in a positive direction. A data collection bias can occur if we are unable to collect data from users who abandoned the product too quickly, or if the data collection is affected by the feature being tested. Data transformation can introduce a bias if we are incorrectly identifying spurious users (such as bots or test machines) and we are either removing legitimate users or including spurious ones \[5]. Metric definition and interpretation can also introduce bias if the metric is analyzing only a subset of the intended-to-treat users or if it puts more weight on certain users or actions unintentionally.

We recommend being very vigilant of selection bias from end-to-end, validating whether a metric value and its movement are consistent with other tracked measurements and our expectations of its movement in an A/B test. For metrics that are particularly important, it may be a good idea to run A/B tests where we are certain about the outcome, and then verify whether or not the metric movement aligns with it.

## Reference

{% embed url="https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/stedii-properties-of-a-good-metric/" %}
