# One-tailed vs Two-tailed Tests

### Misconceptions

> “A one-tailed hypothesis, or directional hypothesis, **predicts** the actual DIRECTION in which the findings will go. It is more precise, and usually used when other research has been carried out previously, giving us a good idea of which way the results will go e.g. we predict more or less, an increase or decrease, higher or lower.
>
> A two-tailed hypothesis, or non-directional hypothesis, **predicts** an OPEN outcome thus the results can go in 2 directions. It is left very general and is usually used when no other research has been done before thus we do not know what will happen e.g. we predict a difference, an effect or a change but we do not know in what direction.”

### **Why we care**

The two-tailed test can show evidence that the control and variation are _different_, but the one-tailed test can show evidence if variation is _better than_ the control.

> In frequentist tests, you have a null hypothesis. The null hypothesis is what you believe to be true absent evidence to the contrary. **** Now suppose you’ve run a test and received a p-value. The p-value represents the probability of seeing a result at least that “extreme” in the event the null hypothesis were true. The lower the p-value, the less plausible it is that the null hypothesis is true.
>
> Now suppose you are A/B testing a control and a variation, and you want to measure the difference in conversion rate between both variants. The two-tailed test takes as a null hypothesis the belief that both variations have equal conversion rates.
>
> The one-tailed test takes as a null hypothesis the belief that the variation is not better than the control, but could be worse.

### Two-Tailed Tests Explained

A two-sided hypothesis and a two-tailed test should be used only when one would act the same way, or draw the same conclusions, if a statistically significant difference in any direction is discovered.

There are maybe one or two rare exceptions where you may want to use two-sided tests.

1. **The costs or barriers to testing in the negative direction are high.** Let’s say you think the latest Brand refresh might hurt, not improve, conversions on your Homepage. You might need to produce stringent (i.e. statistically significant) proof that their newest design doesn’t work if you want to test removing their new designs elsewhere on your website, as you’ll be upsetting a lot of people by doing so. (Example purely illustrative — our Brand team is amazing).
2. **You are experiencing severe pressures from** [**HiPPOs**](https://www.forbes.com/sites/bernardmarr/2017/10/26/data-driven-decision-making-beware-of-the-hippo-effect/#2f15174380f9) **to validate a particular feature.** Demonstrating that what you’re testing not only not improving things, but is significantly harming a key success metric might help convince stakeholders to leave you alone.

### One-Tailed Tests

In 99% of cases, it is better to use one-sided tests which requires 20%-30% less data to detect a given positive MDE.&#x20;

**Thus, the universal accepted decision-making rationale for using one-sided tests is satisfied:** In nearly all cases, if the test performs either the same or worse than the control, we wouldn’t roll it out. So in terms of decision-making criteria, we are only looking to prove μ₂ > μ₁: the one-sided alternate hypothesis. If μ₂ < μ₁ or μ₂ = μ₁, we wouldn’t roll out the variant.



****

## Reference

{% embed url="https://blog.analytics-toolkit.com/2017/one-tailed-two-tailed-tests-significance-ab-testing/" %}

{% embed url="https://dominicsando.medium.com/why-two-sided-testing-is-reducing-your-a-b-testing-programs-impact-by-25-11d72276446a" %}

{% embed url="https://cxl.com/blog/one-tailed-vs-two-tailed-tests/" %}

