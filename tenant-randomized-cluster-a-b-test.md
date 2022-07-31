# Tenant-randomized/cluster A/B test

### Intro

A tenant-randomized A/B test, as the name suggests, is an A/B test that randomly splits software product instances used by tenants into Treatment and Control. A “tenant” is  a group of users that require a consistent experience with the product, such as users belong to the same organization, i.e. company, government agency, or school, etc., or users on the same server or in the same data center. Sometimes, tenant-randomized A/B tests are also called “enterprise-level controlled experiments”, or “cluster-based randomized experiments”. All users in the same tenant are assigned to the same variant and users from different tenants may see different variants. While this seems a simple change of randomization unit, tenant-randomized A/B test has its own challenge in analysis and execution.

![](<.gitbook/assets/image (5).png>)

### Challenge

**Variance Computation.** Within a tenant, the user activities with a product are not independent and identically distributed. Users in a financial organization are likely to have similar behaviors, and users within a tech company tend to have similar patterns of product usage. For user-level metrics (i.e. average number of certain activities _per user_, etc.) in tenant-randomized A/B tests, the i.i.d assumption no longer holds and the naïve variance calculation will likely underestimate variance, leading to false detection of changes that are actually within normal variation.

**Metric Power.** Randomizing by tenant drastically reduces the sample size. Suppose you have 500k users of your product and on average each tenant has about 100 users. You will end up with only 5k tenants in your experiment. This can affect your metric power. In our recent research on Microsoft products, we observed significant power loss on metrics in tenant-randomized A/B tests – they are only able to detect treatment effect that are 10x or larger than user-randomized A/B tests.

**Experiment Balance.** As the sample size shrinks, the experiment becomes more susceptible to the effect of outliers. A tenant is an outlier if its users interact with the product very differently from other tenants’ users. It can also be that the number of users within the tenant is drastically different from others. In many cases, we observed users are distributed disproportionately across small to large tenants and there is a long-tailed distribution of tenant size. If such tenants are not split in balance between Treatment and Control, they can lead to a biased estimate of the treatment effect.

### Sol

**Delta Method**

The Delta Method is a simple technique for correcting variance estimation when the randomization and analysis units don’t match. It takes advantage of the fact that a user level metric from tenant level sums**,** which only relies on (i.i.d.) tenant-aggregated data, it is possible to accurately estimate the variance.

![](<.gitbook/assets/image (2).png>)

**CUPED Variance Reduction**

CUPED stands for Controlled-experiment Using Pre-Experiment Data. It is a way to define variance-reduced (a.k.a. VR) metrics and has been found effective and broadly applicable in A/B tests. The central idea of variance reduction is to replace a metric Y with an alternate metric Y’ such that E(Y’)=E(Y) but Var(Y’) < Var(Y). Using Y’ instead of Y can have improved metric power.

CUPED VR leverages the variance reduction technique from Monte Carlo simulations and uses pre-experiment data to remove explainable variance through a linear-based model. The variance-reduced metric is also referred to as “regression adjusted metric”. It accounts for the relationship between pre-experiment and in-experiment values and detects treatment effects that alter this relationship. Using VR metric can detect a difference that is not detected by the original metric as well as prevent a false positive when a seeming difference is explained from the pre-experiment period.

As the name suggests, CUPED VR requires data collection and retention of data prior to treatment exposure. Also, it is effective when there is a significant overlap in the randomization unit between pre-experiment and in-experiment periods. Typically, we suggest having a pre-experiment period of 1-2 weeks for variance reduction. As the pre- and in-experiment period matching is dependent on the stability of the unit of randomization over time, too short a period will lead to poor matching, whereas too long a period will reduce correlation with the outcome metric during the experiment period.

**SeedFinder**

SeedFinder addresses the problem of possible imbalance between Treatment and Control. It tries hundreds of seeds for randomization, evaluates metrics based on pre-experiment data, and finds the seed that results in minimum bias. The optimal seed will be used for randomization during the experiment. Choosing appropriate metrics for SeedFinder, such as metrics indicating tenant characteristics, can help establish a balanced split between treatment and control and help us draw reliable conclusions from experiment results.

### Before doing tenant randomization

**Reconsider whether a tenant-randomized A/B test is necessary**

* What is the feature to be tested? Does it require a consistent experience for all users within a tenant? (If not, it might be better to run a user-randomized A/B test instead.)

**Understand Telemetry and Flight Assignment**

* Which field identifies (pseudonymously) a tenant?
* Are there empty tenant identifiers? If yes, what is the reason for that? Should they be excluded in an A/B test?
* How many distinct tenants and users are there?
* Are there tenants with only one user? What is the proportion of tenants with only one user or a small number of users? Should they be considered for an A/B test?
* Are there users associated with multiple tenants? If so, what is the reason for that? Should product sessions associated with such users be excluded from analysis?
* Do we run an AA test to confirm balance and that each tenant is assigned to only one flight?

**Metric Design**

* Which metrics should be built to evaluate the feature?
* Which of these are tenant-level metrics, and which are user-level? (User-level/single-average metrics primarily reflect the impact on large tenants, while tenant-level/double-average metrics primarily reflect the impact on small tenants.)
* Does the experimentation platform support Delta method for metric variance estimation? If not, only tenant-level metrics can be applied.
* What is the minimum detectable treatment effect for each metric?

**Improve Metric Sensitivity**

* Is there a significant overlap of tenants between pre-experiment and in-experiment period? If the answer is yes, CUPED VR can be applied.
* What improvements are possible after applying variance reduction? Is it big enough that the feature effect can be detected?

**Validity Check**

* Are there any “outlier” tenants? Could they cause metrics to move if they are not split in balance between Treatment and Control in A/A test?
* Does the experimentation platform support SeedFinder method to look for a good seed for randomization?

### What about Tenant-Pairing?

**Method**. In tenant-pairing, we group all tenants into pairs, where the two tenants in the same pair are “similar” to each other. The “similarity” can be defined by a selective set of attributes, i.e. geography, industry etc., or based on machine learning results where encoding, clustering and approximate nearest neighbor methods are applied.&#x20;

**Analysis**. Such paired variant assignment can be treated as “constrained randomization”, which is different from randomizing by tenant, a “pure randomization”. Therefore, we won’t be able to use a standard two-sample t-test to measure the significance of metric movements. Instead, we have to use a paired t-test. The idea is to calculate the difference on the metric value between treatment and control in each pair and conduct a one-sample t-test on this sample of differences against zero.

**Problem.** We tried this approach in our research; however, we discovered that **tenant-pairing consistently fails to provide better assignment balance or metric sensitivity** compared to regular tenant-based variant assignment. The major issue is the data loss introduced by tenant-pairing. Tenant-pairing relies on historical data and the result only contains tenants who had product usage in the historical period. If we have new tenants coming to use the product during experiment, they are not included in the pairing result and thus won’t be randomized. In addition, if one tenant in a pair does not show up in the experiment, its counterpart will have to be excluded from the analysis.&#x20;

**Other limitations.** Pairing-based randomization suffers from other limitations. A naïve paired t-test cannot be applied at any sub-randomization unit level. Also, pairing-based randomization requires extra effort when running segmented analysis, which slices data based on a specific dimension, e.g. platform, region, etc. to identify treatment effect for sub-groups of the population.&#x20;

Infrastructure-wise, tenant-pairing requires significant engineering work and maintenance. If tenant-pairing is based on a machine learning algorithm, the model parameters need to be tuned and refreshed regularly as they can become stale as time goes by. In addition, enabling tenant-pairing requires infrastructure change on existing experimentation platform, such as supporting paired t-test in addition to standard t-test, etc.

**Recommendation.** Evaluating these factors for real products, we have so far recommended randomized assignment over paired randomization in tenant-randomized A/B tests.

### Conclusion

The results showed that **using a combination of truncation and VR provides the biggest boost on metric sensitivity, for both tenant-level and user-level metrics**.&#x20;

User-randomized A/B tests have better metric sensitivity than tenant-randomized A/B tests. There are multiple approaches we can use to improve sensitivity for tenant-randomized A/B tests, and of these, common variance control techniques of metric capping and regression adjustment were observed to be most effective. However, tenant randomization can still result in limited metric power and we do not recommend widespread adoption of tenant-randomized A/B test. **It should only be used when necessary, and experimenters should follow the best metric design practice and make sure to use variance-reduced metrics** to be able to detect metric movements. It’s also wise to **test only one variant at a time** instead of testing multiple variants simultaneously to generate as large a sample size as possible for experiment analysis.

## Reference

{% embed url="https://www.microsoft.com/en-us/research/group/experimentation-platform-exp/articles/why-tenant-randomized-a-b-test-is-challenging-and-tenant-pairing-may-not-work/" %}

