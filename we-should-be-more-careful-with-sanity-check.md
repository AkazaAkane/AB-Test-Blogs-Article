---
description: written by Sam Plank, Adapted from work by Tom Xing
---

# We should be more careful with Sanity Check

## My Thoughts

The article talks about sometimes each single sanity check succeeds but overall they fail and move defensive metrics to negative direction.

### Background

Sanity tests are experiments run to ensure that product updates which are expected not to change user behavior don’t in fact change any metrics. Defensive metrics are metrics that shouldn’t be hurt when an experiment is run that is expected to change user behavior. For instance, when launching a stylistic change to the site that isn’t expected to hurt engagement metrics, a sanity test can be used to make sure that there are no bugs or unintended consequences. When running an experiment that is expected to increase metric A without hurting metric B, metric B is a defensive metric.\
\
Experiment will be launched if failling to reject the null hypothesis. If a sanity test had in reality a -2% effect, assuming that the error bars are 1% wide, one would expect the experiments to be launched 50% of the time.&#x20;

### Results

The author looked at a group of sanity tests and measured their effect on defensive metrics. When looking at experiments with a binomial test,there are more than half moved the defensive metrics in the negative direction with statistical significance (p=0.017). One should expect the effect on our defensive metrics to be negative, even if each individual experiment didn’t produce a statistically significant negative effect.

![](https://qph.fs.quoracdn.net/main-qimg-2371ae68edc3b797a710aa6649eb8f26-pjlq)

### Discussion

The above result shows that defensive metrics can be hurt even in the absence of of a statistically significant hit on any one metric. These could add up over multiple experiments. Defensive metrics are better understood as a bound on how much damage was done, rather than a guarantee no damage was done. Here are some suggestions:

* Be more stringent with the p value required for defensive metrics.
* Adopt as a null hypothesis that the effect on the defensive metric is negative, then require enough evidence to reject that hypothesis.
* If you have some evidence you can use as a prior, you may want to use a Bayesian approach to hypothesize how negative an effect is expected.
