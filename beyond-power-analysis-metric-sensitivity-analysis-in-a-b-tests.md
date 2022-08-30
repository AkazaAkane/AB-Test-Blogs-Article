# Beyond Power Analysis: Metric Sensitivity Analysis in A/B Tests

## Decomposing Metric Sensitivity

In metric actionability analysis, sensitivity is divided into two components: statistical power and movement probability.&#x20;

Assume a feature has an impact on a metric, i.e., the alternative hypothesis is true. Under the alternative hypothesis $$𝐻1$$:

Prob\[Detecting the treatment effect on the metric] = $$𝑃𝑟𝑜𝑏(𝐻1)𝑃𝑟𝑜𝑏(𝑝<0.05|𝐻1)$$.

$$𝑃𝑟𝑜𝑏(𝑝<0.05|𝐻1)$$ is the term for statistical power, the probability of accepting the alternative hypothesis if it is true. It is related to effect size, sample size and significance level. Power analysis allows you to estimate any of these variables, given specific values of the remaining ones. Experimenters usually evaluate the statistical power by estimating the minimum DTE, which is effect size, given a specific setting of the remaining parameters (i.e., 80% power, 0.05 significance level and a sample size reflecting the typical number of users in an A/B test for that product). $$𝑃𝑟𝑜𝑏(𝐻1)$$ is the term for movement probability, representing how often the feature causes the treatment effect under $$𝐻1$$.

Next, we talk about methods for assessing metric sensitivity considering both components.

* **Power analysis** to verify that the minimum DTE is attainable.
* Movement analysis performed on historical A/B tests:
  1. **Movement confusion matrix** to study the alignment between the observed metric movement and the expected effect from each historical A/B test.
  2. **Observed movement probability** to compare the sensitivity of different metrics based on their behavior within the historical A/B tests.

### Power Analysis

With the results of power analysis, the metric authors can discuss with the feature team whether the minimum DTE for the metric is attainable in typical A/B tests. If the value is in percentage, it is good to convert it to absolute value to get a sense of the magnitude of the required change. If the minimum DTE is far beyond what is expected in a typical A/B test, the metric may rarely move.&#x20;

In the case of Microsoft Teams, _Time in App_ has 0.3% as minimum DTE with full traffic in 1 week. The team mapped the percentage to an absolute increase in the metric, and they were satisfied with the statistical power: they expect that feature changes would reasonably lead to such an increase in _Time in App_ and hence the minimum DTE was achievable.

### Movement analysis

To evaluate a metric’s sensitivity, the metric authors should study the metric behavior on historical A/B tests results. Historical A/B tests are a great tool for evaluating metrics – they are referred to as “Experiment Corpus” in and are heavily used to assess the quality of a proposed metric. The greater the quantity of historical A/B tests available, the more actionable the study results would be.

The study uses two common types of corpus. **Labeled corpus** consists of tests where there is high confidence that a treatment effect exists. That confidence is built by examining many metric movements to check if they align with the hypothesis of the test, deep dives and offline analyses that add more evidence to the correctness of the hypotheses of the A/B tests. **Unlabeled corpus** consists of randomly selected tests and helps evaluate the overall movement probability.

#### 1. Movement Confusion Matrix

The confusion matrix below helps understand if the metric moves as expected. The expectation is indicated by the label in the matrix below and is supported by analyzing metrics and deep diving on the A/B test. In the table, the left column covers A/B tests where the alternative hypothesis $$𝐻1$$ is true and the right one covers those where $$𝐻0$$ is true. Each test from the labeled corpus can fit into one of the two columns.

![movement confusion matrix](https://www.microsoft.com/en-us/research/uploads/prod/2021/04/Capture.png)

To fill the matrix, compute the metric on all the A/B tests in the labeled corpus. A treatment effect is detected when the metric has a statistically significant movement. Otherwise, a treatment effect is not detected.

Once you have the metric movements, check tests covered by $$𝑁1$$ to see if the metric has the same direction as expected. For tests where metric movements are in the opposite direction, metric authors should investigate further. Is that because the previous labeling analysis was insufficient and the expected direction is incorrect? Or is that because the metric diverges from the real movement in some cases? In the first case, update the label of the A/B test. Otherwise, the metric can be untrustworthy and can lead to incorrect decision making. Metric authors should consider using an alternative metric that better aligns with the product.

The confusion matrix summarizes the behavior of the metric on the labeled corpus. **A metric that is sensitive will have a large** $$\frac{𝑁1}{𝑁1+𝑁2}$$**, as close to 1 as possible. A robust metric (i.e., one that is less susceptible to noisy movements) will have** $$\frac{𝑁3}{𝑁3+𝑁4}$$ **that is very close to the significance level or false positive rate.**

Microsoft Teams expected _Time in App_ to move when treatment effect existed in supporting metrics such as chat or call actions. They found several tests with promising impact, but _Time in App_ missed catching those, with $$\frac{𝑁1}{𝑁1+𝑁2}$$ close to 0.

#### 2. Observed Movement Probability

&#x20;An unlabeled corpus can be used to compare sensitivity among various candidate metrics. To do the comparison, we first compute **the** **observed movement probability for each candidate metric: this is the proportion of A/B tests where the metric’s movement was statistically significant (p-value < 0.05).** To compute this probability using the unlabeled corpus, the metric authors can generate a confusion matrix for each candidate similar to the one above, with the two columns collapsed into one. This corresponds to $$\frac{𝑁1+𝑁3}{𝑁1+𝑁2+𝑁3+𝑁4}$$

To properly compare the movement probability among metrics, we first decompose it by the correctness of the alternative hypothesis. The observed movement probability can be written as:

$$𝑃𝑟𝑜𝑏(𝐻1)𝑃𝑟𝑜𝑏(𝑝<0.05|𝐻1)+𝑃𝑟𝑜𝑏(𝐻0)𝑃𝑟𝑜𝑏(𝑝<0.05|𝐻0)$$.

The first term represents the probability that a _real_ treatment effect exists and is successfully detected. The second term is the bias introduced by the metric’s false positive movement in the A/B tests with no treatment effect. The bias term is bounded at 5%, the significance level we use in our analysis.

This makes the observed movement probability a biased estimation of the real movement probability of each metric, with the bias upper bounded by 5%. Hence, the observed movement probability can still be used to compare sensitivity between metrics as long as the difference between the observed probabilities is larger than 5% – the upper bound of bias.

Microsoft Teams collected dozens of historical A/B tests, and the observed movement probability for the _Time In App_ metric was much lower than that of several other metrics in use. This indicated that _Time in App_ has much lower sensitivity and the metric authors need to improve it before it can be properly used in A/B tests.

## Tips for designing sensitive metrics

So, you performed the analysis above and you found your metric to be insensitive. Before you trash the metric and start from scratch, below are some tips and tricks to help metric authors improve the sensitivity of some metrics: variations on metric definitions help improve the overall sensitivity, and variance reduction technique efficiently improves statistical power.

### Metric design

Sometimes a metric is determined to be insensitive yet it still accurately measures what the team wants to assess in an A/B test. Applying techniques from A/B metric design is one of the simplest ways to increase its sensitivity. Below are some techniques that can help you increase the sensitivity of your metric.

#### Apply a transformation to the metric value

One straightforward way of improving the sensitivity of a metric is reducing the impact of outliers. The transformations below change the computation of the metric to minimize the impact or remove it all together.

* **Capping** the value of the metric at a fixed maximum value can greatly reduce the impact of outliers which usually hurt sensitivity.
* **Apply the Log** function on the metric value: $$𝑥→𝑙𝑜𝑔(1+𝑥)$$. This transformation gives less weight to extreme large values when the distribution is skewed, which helps detect smaller metric movement.
* Change of metric’s **aggregation level** impacts the weight assigned to each unit. See \[6] about how single-average and double-average differ in tenant randomized tests.

#### Use alternative metric types

Typically, A/B metrics are _averaged_ across the randomization units. These are Average metrics and _Time in App per User_ is one such example. However, using alternative methods to aggregate the metric can help increase its sensitivity. Below are some other metric types we can use:

* **Proportion metrics** measure the proportion of the units satisfying a condition, e.g., _% Users with Time in Channel._ A proportion metric is easier to move, compared to the other types.
* **Conditional average metric** is similar to average metric, except that the unit base is under a condition, e.g., _Time in App per Active User_. The statistical power will be improved if the numerator and denominator are correlated.
* **Percentile metric** is usually used to monitor the extreme case, such as _99 Percentile of Time in App_.

#### Try proxy metrics

If a metric represents a long-term goal, such as overall user satisfaction, it might be hard to move in the short period of an A/B test. Consider finding proxy metrics by predictive models.

### Variance Reduction

CUPED (Controlled-experiment Using Pre-Experiment Data) is one Variance Reduction method, leveraging the data prior to the A/B test to remove explainable variance during the test. It has been shown to be effective and is widely used in A/B tests at Microsoft.

Microsoft Teams tried several types of metric design and Variance Reduction for _Time in App_. Following the steps for assessing sensitivity, they finally chose _Log of Capped Time in App_ as the alternative to _Time in App_, with Variance Reduction applied whenever possible during A/B test results analysis.

### Summary

Experimenters need sensitive metrics to detect a feature impact. But sensitivity can’t be determined by statistical power alone. Metric authors could design alternative metrics and then select the best based on the evaluation methods mentioned above. Consider how each metric fulfills the following qualities. If no metric fulfills all of them, choose the one that meets most of them.

* Feature teams agree that the minimum DTE of the metric can be achieved.
* In the confusion matrix, the movement direction is always explainable for A/B tests covered by $$𝑁1$$.
* The metric has high $$\frac{𝑁1}{𝑁1+𝑁2}$$ in the confusion matrix and 2} $$\frac{𝑁3}{𝑁3+𝑁4}$$close to the significance level.
* The metric should have large observed movement probability compared with other candidates.

## Reference

{% embed url="https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/beyond-power-analysis-metric-sensitivity-in-a-b-tests/" %}
