# Sample Ratio Mismatch

The SRM test is performed on the underlying data i.e. counts of users that reported using A and B, before the ship-decision analysis is started. We like to think of it as an end-to-end method for detecting A/B tests that suffer from severe quality issues and need attention. Contrary to intuition, it is not sufficient to glance only at the ratio of users in A vs. B. The ratio lacks information about the sample size. We need to use a statistical test such as a chi-square statistic to determine whether the observed distribution of users in experiment variants statistically differs from the one that was configured.

Every analysis that is part of an ongoing A/B test using ExP first needs to pass this test before we reveal the actual results of the experiment. The threshold that we use is conservative to reduce the likelihood of false positives: p-value < 0.0005. In practice, A/B tests with an SRM will result in a p-value that is much lower than the threshold. Now let’s discuss how to triage an SRM.

### Root Cause

![](<.gitbook/assets/image (4).png>)

### Finding the SRM root cause

**Segments.** A common starting point is to analyze segments (aka cohorts). Oftentimes, an SRM root-cause is localized to a particular segment. When this is the case, the segments that are impacted will have an incredibly low p-value, prompting the A/B test owners to dive deeper.&#x20;

**Triggering.** For A/B tests that are analyzed on a subset of the assigned population – a triggered A/B analysis \[9], we recommend examining the boolean condition that was used to decide which logs to keep. Commonly, A/B testers will create a triggered analysis to increase the sensitivity of their metrics. For example, if a change on a checkout page such as a new coupon code field is introduced, it might be valuable to analyze only the users that actually visited this page. However, to zoom in, a valid condition – one that captures the experience of the users before the change being tested – needs to be used.  Oftentimes the necessary logging for the provided condition is not present in every variant. Consider if the change in the checkout website example above was a new coupon code field and the condition was zooming-in to users that were exposed to this new field. What would happen in the analysis? Unless the counterfactual logging was added to the control variant that does not have this new field, there would be a severe SRM in favor of the treatment \[10]. A simple diagnostic test for confirming such a bad condition is to examine whether the untriggered population in the A/B test does not have an SRM. Whenever the untriggered analysis for a given A/B test is trustworthy and the triggered analysis has an SRM, a misconfigured triggered condition or the lack of logging for the configured condition are the most likely root cause. The solution is often simple: updating the triggered condition so it includes all aspects that could affect a user (e.g. to a condition that is triggering to users that visited the sub-site as opposed to users that interacted with a component on that sub-site).

## Reference

{% embed url="https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/diagnosing-sample-ratio-mismatch-in-a-b-testing/" %}
