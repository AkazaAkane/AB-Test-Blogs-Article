---
description: >-
  Practical Guide to Controlled Experiments on the Web: Listen to Your Customers
  not to the HiPPO
---

# Ron's 2016 KDD

## My Thoughts

This paper is more like a talk about Ron's past experience. The tricks he mentions are more useful to people with AB test background, which reminds me of those tricks in DL that belong to arts rather than science.

### Summary

In this paper, Ron provides a practical guide to conducting online experiments. They believe that significant learning and return-on- investment (ROI) are seen when development teams listen to their customers, not to the Highest Paid Person’s Opinion (HiPPO). Finally, they provide several examples of controlled experiments with surprising results.

### Motivating Examples

For the Doctor FootCare checkout page, variant A in Figure 1 outperformed variant B by an order of magnitude. In reality, the site ― "upgraded" from the A to B and lost 90% of their revenue. Most of the changes in the upgrade were positive, but the coupon code was the critical one: people started to think twice about whether they were paying too much because there are discount coupons out there that they do not have. By removing the discount code from the new version (B), conversion-rate increased 6.5% relative to the old version (A).

For Microsoft’s Office Help articles, the number of ratings plummeted by an order of magnitude, thus significantly missing on goal #2 above. Based on additional tests, it turned out that the two-stage model actually helps in increasing the response rate. Even goal #1 was somewhat of a disappointment as most people chose the extremes (one or five stars). When faced with a problem for which you need help, the article either helps you solve the problem or it does not!

### Terminology



1.Confidence level. Commonly set to 95%, this level implies that 5% of the time we will incorrectly conclude that there is a difference when there is none (Type I error). All else being equal, increasing this level reduces our power (below).

2\. Power. Commonly desired to be around 80-95%, although not directly controlled. If the Null Hypothesis is false, i.e., there is a difference in the OECs, the power is the probability of determining that the difference is statistically significant. (A Type II error is one where we retain the Null Hypothesis when it is false.)

3\. Standard Error. The smaller the Std-Err, the more powerful the test. There are three useful ways to reduce the Std-Err:

1.  The estimated OEC is typically a mean of large samples. As

    shown in Section 3.1, the Std-Err of a mean decreases proportionally to the square root of the sample size, so increasing the sample size, which usually implies running the experiment longer, reduces the Std-Err and hence increases the power.
2. Use OEC components that have inherently lower variability, i.e., the Std-Dev, 𝜎, is smaller. For example, conversion probability (0-100%) typically has lower Std-Dev than number of purchase units (typically small integers), which in turn has a lower Std-Dev than revenue (real-valued).
3.  Lower the variability of the OEC by filtering out users who were not exposed to the variants, yet were still included in the OEC. For example, if you make a change to the checkout page, analyze only users who got to the page, as everyone

    else adds noise, increasing the variability.
4. The effect, or the difference in OECs for the variants. Larger differences are easier to detect, so great ideas will unlikely be missed. Conversely, if Type I or Type II errors are made, they are more likely when the effects are small.

The following formula approximates the desired sample size, assuming the desired confidence level is 95% and the desired power is 90%:

𝑛 = (4𝑟𝜎/Δ)^2

where 𝑛 is the sample size, 𝑟 is the number of variants (assumed to be approximately equal in size), 𝜎 is the std-dev of the OEC, and Δ is the minimum difference between the OECs. The factor of 4 may overestimate by 25% for large 𝑛 (32; 33), but the approximation suffices for the example below.

### Extention for Online

Treatment Ramp-up

An experiment can be initiated with a small percentage of users assigned to the treatment(s), and then that percentage can be gradually increased. For example, if you plan to run an A/B test at 50%/50%, you might start with a 99.9%/0.1% split, then rampup the Treatment from 0.1% to 0.5% to 2.5% to 10% to 50%. At each step, which could run for, say, a couple of hours, you can analyze the data to make sure there are no egregious problems with the Treatment before exposing it to more users.&#x20;

Automation\
Once an organization has a clear OEC, it can run experiments to optimize certain areas amenable to automated search. For example, the slots on the home page at Amazon are automatically optimized. If decisions have to be made quickly (e.g., headline optimizations for portal sites), these could be made with lower confidence levels because the cost of mistakes is lower.&#x20;

Software Migrations

Experiments can be used to help with software migration. If a feature or a system is being migrated to a new backend, new database, or a new language, but is not expected to change user- visible features, an A/B test can be executed with the goal of retaining the Null Hypothesis, which is that the variants are not different. We have seen several such migrations, where the migration was declared complete, but an A/B test showed significant differences in key metrics, helping identify bugs in the port. Because the goal here is to retain the Null Hypothesis, it is crucial to make sure the experiment has enough statistical power to actually reject the Null Hypothesis if it false.

### Limitations

1\. Quantitative Metrics, but No Explanations.&#x20;

It is possible to know which variant is better, and by how much, but not ―why.‖ In user studies, for example, behavior is often augmented with users’ comments, and hence usability labs can be used to augment and complement controlled experiments (34).

2\. Short term vs. Long Term Effects.&#x20;

Controlled experiments measure the effect on the OEC during the experimentation period, typically a few weeks. While some authors have criticized that focusing on a metric implies short-term focus (22) (34), we disagree. Long-term goals should be part of the OEC. Let us take search ads as an example. If your OEC is revenue, you might plaster ads over a page, but we know that many ads hurt the user experience, so a good OEC should include a penalty term of usage of real-estate for ads that are not clicked, and/or should directly measure repeat visits and abandonment. Likewise, it is wise to look at delayed conversion metrics, where there is a lag from the time a user is exposed to something and take action. These are sometimes called latent conversions (24; 22). Coming up with good OECs is hard, but what is the alternative? The key point here is to recognize this limitation, but avoid throwing the baby out with the bathwater.

3\. Primacy and Newness Effects.&#x20;

These are opposite effects that need to be recognized. If you change the navigation on a web site, experienced users may be less efficient until they get used to the new navigation, thus giving an inherent advantage to the Control. Conversely, when a new design or feature is introduced, some users will investigate it, click everywhere, and thus introduce a ―newness‖ bias. This bias is sometimes associated with the Hawthorne Effect (35). Both primacy and newness concerns imply that some experiments need to be run for multiple weeks. One analysis that can be done is to compute the OEC only for new users on the different variants, since they are not affected by either factor.

4\. Features Must be Implemented.&#x20;

A live controlled experiment needs to expose some users to a Treatment different than the current site (Control). The feature may be a prototype that is being tested against a small portion, or may not cover all edge cases (e.g., the experiment may intentionally exclude 20% of browser types that would require significant testing). Nonetheless, the feature must be implemented and be of sufficient quality to expose users to it. Jacob Nielsen (34) correctly points out that paper prototyping can be used for qualitative feedback and quick refinements of designs in early stages. We agree and recommend that such techniques complement controlled experiments.

5\. Consistency.&#x20;

Users may notice they are getting a different variant than their friends and family. It is also possible that the same user will see multiple variants when using different computers (with different cookies). It is relatively rare that users will notice the difference.

6.Parallel Experiments.&#x20;

Our experience is that strong interactions are rare in practice (33), and we believe this concern is overrated. Raising awareness of this concern is enough for experimenters to avoid tests that can interact. Pairwise statistical tests can also be done to flag such interactions automatically.

7\. Launch Events and Media Announcements.&#x20;

If there is a big announcement made about a new feature, such that the feature is announced to the media, all users need to see it.

### IMPLEMENTATION ARCHITECTURE

#### Random Assignment

1.Users must be equally likely to see each variant of an experiment (assuming a 50-50 split). There should be no bias toward any particular variant.

2\. Repeat assignments of a single user must be consistent; the user should be assigned to the same variant on each successive visit to the site.

3\. When multiple experiments are run concurrently, there must be no correlation between experiments. A user’s assignment to a variant in one experiment must have no effect on the probability of being assigned to a variant in any other experiment.

4.The algorithm should support monotonic ramp-up (see Section 3.3.1), meaning that the percentage of users who see a Treatment can be slowly increased without changing the assignments of users who were already previously assigned to that Treatment.

We found that only the cryptographic hash function MD5 generated no correlations between experiments. SHA256 (another cryptographic hash) came close, requiring a five-way interaction to produce a correlation.

#### Assignment Method

Traffic splitting is a method that involves implementing each variant of an experiment on a different fleet of servers, be it physical or virtual. The website embeds the randomization algorithm into a load proxy server to split traffic between the variants. Traffic requires no changes to existing code to implement an experiment. However, the approach is the most expensive way to handle assignment.

An alternative method is server-side selection, whereby API calls embedded into the website’s servers invoke the randomization algorithm and enable branching logic that produces a different user experience for each variant. Server-side selection is an extremely general method that supports multiple experiments on any aspect of a website, from visual elements to backend algorithms. However, it requires additional work from a developer to implement the code changes needed to run experiments.

A final alternative is client-side selection, whereby JavaScript calls embedded into each web page contact a remote service for assignments and dynamically modify the page to produce the appropriate user experience. Client-side experiments are generally easier to implement than server-side experiments because the developer need only embed canned snippets of JavaScript into each page. However, this method severely limits the features that may be subject to experimentation; in particular, experiments on dynamic content or backend features are much harder to implement.

### Lessons

#### Imbalanced Sample

Assuming all factors are fixed, a good approximation for the multiplicative increase in running time for an A/B test relative to 50%/50% is 1/(4𝑝 (1 − 𝑝) ) where the treatment receives portion 𝑝 of the traffic. For example, if an experiment is run at 99%/1%, then it will have to run about 25 times longer than if it ran at 50%/50%.

#### Speed Matters

A Treatment might provide a worse user experience because of its performance. Greg Linden (36 p. 15) wrote that experiments at Amazon showed a 1% sales decrease for an additional 100msec, and that a specific experiments at Google, which increased the time to display search results by 500 msecs reduced revenues by 20%.

#### Weekly Effect

we strongly recommend running experiments for at least a week or two, then continuing by multiples of a week so that day-of-week effects can be analyzed.This lesson can be generalized to other time-related events, such as holidays and seasons, and to different geographies: what works in the US may not work well in France, Germany, or Japan.

#### Maintenance

An experiment may show a statistically significant difference between variants, but choosing to launch the new variant may still be unjustified because of maintenance costs. A small increase in the OEC may not outweigh the cost of maintaining the feature.

