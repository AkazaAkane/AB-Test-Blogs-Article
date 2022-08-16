# Pre-Experiment Stage

![](https://www.microsoft.com/en-us/research/uploads/prod/2020/07/TrustworthyABTestingPre-1024x509.png)

### Forming a Hypothesis and Selecting Users

#### Formulate your Hypothesis and Success Metrics

The first step in designing an A/B experiment is to have a clear hypothesis. The hypothesis should capture the expected effect of the change introduced by the treatment. It should also be simple: If you are making a complex change, break it down into a series of simple changes – each with its own hypothesis.&#x20;

The hypothesis should also include a set of metrics to measure the impact of the change introduced on the product. Note that **the hypothesis should be falsifiable or provable by the set of metrics considered**. For example, a hypothesis claiming that a certain treatment will improve branding is hard to prove or falsify if there is no metric that can quantify the impact of the brand.

Choosing the right set of metrics for the experiment is essential to get a correct measurement of the treatment effect. **A core set of metrics should contain user satisfaction metrics, guardrail metrics, feature and engagement metrics and data quality metrics**. These metrics should be computed for all experiments in a product, regardless of which features they target. This guarantees that we can always detect regressions of important metrics, regardless of which feature we are testing on.&#x20;

Another important consideration when forming the hypothesis is the power of the A/B test to detect the anticipated change in the metrics. If we anticipate that only a small proportion of the traffic will be affected, we need to assign an amount of traffic to the A/B test that ensures the affected portion satisfies the power calculation lower bound.

#### Choose the Appropriate Unit of Randomization

Below are three considerations when choosing the randomization unit.

**Stability of the Id**: On the web, users are tracked using cookie-based Ids. Such ids are easy to implement and track, however they are not stable. Users can clear their cookies and land in the other variant in the next visit, making accurate estimation of long term effects of a treatment hard . Only **use such ids for short-term experiments** to avoid having your experiment population significantly churned. Moreover, create metrics to capture the various categories of cookies used for tracking to ensure that the treatment is not affecting them disproportionately compared to control.

**Network Effects**: When product changes impact both the user and their collaborators/connections, the stable unit treatment value assumption (SUTVA) – that each user’s response in the experiment is independent of the treatment assignment to other users – is violated.&#x20;

**Enterprise Constraints**: Product and business limitations can constrain the selection of the randomization unit. They might require that a feature is deployed to the entire enterprise for full effectiveness, or the admins in an enterprise may have a policy to deliver a uniform experience to all users. In such cases it is not possible to randomize by individual users. Randomizing at the enterprise-level brings up special considerations to be aware of.

#### Check and Account for Pre-Experiment Bias

Historical data can be very useful for detecting and mitigating pre-experiment bias. Below are three techniques:

**Retrospective-AA Analysis**: This analysis computes the same metric for the treatment and control group in the period before the experiment start. We check if there was any bias between treatment and control group in the pre-experiment period metric. If we do observe a significant difference in a metric between the two variants in the Retro-AA analysis, we are more confident that a statistically significant movement (with p-value close to 0.05) in the metric in the experiment period is likely due to the bias introduced by randomization. Retro-AA analysis is not a conclusive test of bias in a metric due to randomization, but it is a very useful indicator in the absence of other evidence suggesting otherwise.

**Seedfinder**: With Seedfinder, before starting the experiment, we generate a key set of metrics for a large number of seeds. Then we pick an optimal seed, the one where we observe the least amount of statistically significant metric differences. Such re-randomization reduces the noise in metrics that is unrelated to the treatment tested and improves the precision of the estimated treatment effect.

**Variance Reduction**: Using pre-experiment period data for the same metric can help reduce both bias and variance in the final analysis. This method is commonly employed for variance reduction(VR) to get improved sensitivity for a metric. The basic idea of VR is to use the metric in the pre-experiment period to reduce the variance in the metric during the experiment.&#x20;

### Pre-Experiment Engineering Design Plan

The next set of patterns cover some engineering practices done before the experiment starts that we employ at Microsoft to increase trustworthiness of our experimentation.

#### Set Up Counterfactual Logging

When running A/B test, not every user in the two variants will be exposed to the treatment. Some changes target users with a specific intent or behavior. For example, consider the weather forecast answer in Bing. When it was first developed, the team wanted to run an A/B experiment to test its effect on user behavior. When we first analyzed the results, we weren’t able to detect any statistically significant movements in the desired metrics. It turns out only a small portion of the users in the test issue a weather-intent query and see the weather answer. The largest portion of the users in the test never issued any queries with weather-intent and those users’ logs simply add noise, overpowering the signal from the small portion of users who were in fact affected by the weather forecast answer.

What we need is a scorecard that “zooms in” to the portion of users who issued queries with weather intent. But how can we identify them? On treatment, it is relatively straightforward: find all users for whom Bing served the weather forecast answer. How can we identify such users in control? Bing didn’t show any weather answer on control since the answer is not in production yet. This is where counterfactual logging comes in: for each control variant request sent to Bing, the system determines whether it would have shown the weather answer if the user was assigned to treatment instead of control. If the answer would have shown, it will be marked in that impression’s log entry – we call that the counterfactual flag.

**Counterfactual logging enables us to identify the exact same type of traffic from treatment and control, so that we can make an apples-to-apples comparison between the two variants**. It allows us to identify the portion of users that were or would have been affected by the change on both treatment and control. Now we can perform a more sensitive analysis that remove noise from unaffected users and allow us to detect statistically-significant movements in the metrics. Note that it’s important to check the correctness of the counterfactual logging to avoid missing affected users from our zoomed-in scorecard.&#x20;

Running both treatment and control systems for each impression can be computationally inefficient and can cause delays that would negatively affect the user’s experience. In practice, there are many optimizations that can be introduced, and they depend on the implementation details of the system: not running the counterfactual logging on the critical path, setting it up only for the feature tested as opposed to the full product stack, or making the counterfactual flag too permissible and hence including some unaffected users in the analysis if it leads to a faster or cheaper implementation.

#### Have Custom Control and Standard Control

Microsoft News once ran an experiment where the treatment was making a minor tweak to the UI on their page. Due to an error in specifying the configuration, both treatment and control experiences did not have the search box on the new tab page. This caused a massive impact on user satisfaction and engagement but since the error impacted both treatment and control there was no detectable change when comparing treatment to control. The change was detected only when we compared treatment to production users, as part of a standard control. Users who are part of the standard control do not get exposed to any code from the experiment. We can now test if the code added to the custom control is causing a large movement in our metrics by comparing the standard control to custom control, indicating a problem in the custom control.&#x20;

The implementation of standard control for all experiments at scale is not trivial. It should be designed to not exhaust significant amount of traffic, keep users in certain experiments isolated , and respect the traffic settings targeted by each experiment.

#### Review Engineering Design Choices to Avoid Bias

When setting up the data pipelines and the infrastructure used by each variant in an A/B test, make sure that none of these design decisions impact how treatment and control responses are returned, unless they are intended to be part of the treatment.

This issue comes up frequently when the treatment is testing a new or improved machine learned model. If any components of the machine learned model are shared between treatment and control, then any update to the machine learned model based on such shared components will impact both treatment and control. We can expect control to start showing some of the more relevant content that initially appeared in treatment alone. This artificially improves the control experience due to the leakage of treatment effect and introduces a bias in the estimation of the treatment effect. Such components can be features of the model, the data pipeline used for training, or the model itself.

Other engineering design decisions can also lead to one of the variants being at an unintended advantage. Consider for example how Bing serves results to queries issued by the user. For a faster response, the system might have some responses available in a cache to reduce the response time. The size of the cache and whether or not it is a shared resource between treatment and control can unintentionally affect any performance comparison between the two.

In the experiment design stage, examine your architecture and engineering decisions to **avoid leakage of treatment effect through shared infrastructure or giving one of the variants an advantage by not controlling for availability of resources or other relevant factors**.

### Pre-Validation by Progressing Through Populations

While every change to a product will go through a rigorous quality control process before it is exposed to users, there is still a large amount of uncertainty about the desirability of the change. The more users we can include in the experiment the more we can reduce uncertainty. This introduces a tradeoff between risk and certainty. If a change is indeed not desirable, exposing it to a large number of users will increase the cost. However, if the change is not introduced to a large enough number of users, we may not know that the change is not desirable due to low statistical power to detect sufficiently small changes. Our solution is to perform a safe rollout \[23] where the treatment is tested gradually across various user populations or rings, and gradually increasing percentages within a user population.

#### Gradual Rollout Across Different User Populations

One way to navigate the risk v/s certainty tradeoff is to test a change in a special user population – say dogfood or beta users- before testing the change in a more general and bigger population. **Testing a change in a dogfood user population can help identify egregious functional and experience issues before exposing this change to more general users**. This early test reduces the uncertainty about the treatment effect: if a change will cause a large regression in the dogfood population, it is also likely to cause a regression in the general population as well. We do strongly advise against concluding that the results from an experiment on dogfood audience will apply exactly on the general population. Experiments in dogfood audiences should just be used as a milestone leading to experiment in general population.

#### Gradual Rollout within a User Population

Once we decide to experiment with the general user population, another way to navigate the risk v/s certainty tradeoff is to only expose a small portion of that population to the treatment and ramp-up as we observe and measure the treatment effect. An experiment can be first exposed to 1% of the traffic and gradually ramped-up to 5%, then 10% until the desired final traffic is achieved. With each ramp-up, we compute metrics for each traffic percentage to measure the treatment effect on the user: as the traffic increases, the power increases and we will be able to detect any unanticipated regressions in the metrics we care about.\
**With such progressions between user populations and within the same population, we can gradually test a treatment and detect any unintended consequences without causing unnecessary harm to the user experience**.
