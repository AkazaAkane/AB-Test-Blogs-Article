# Multi-armed Bandit (MAB)

### Definition

MAB is a type of A/B testing that uses machine learning to learn from data gathered during the test to dynamically increase the visitor allocation in favor of better-performing variations. What this means is that variations that aren’t good get less and less traffic allocation over time. The core concept of MAB is ‘dynamic traffic allocation’ – it’s a statistically robust method to continuously identify the degree to which a version is outperforming others and to route the majority of the traffic dynamically and in real-time to the winning variant.

**Thompson Sampling**

In a nutshell, Thompson sampling is a greedy method that always chooses the arm that maximizes expected reward. In each iteration of the bandit experiment, Thompson sampling simply draws a sample _ctr_ from each arm’s Beta distribution, and assign the user to the arm with the highest _ctr_. If one of the arms is performing well, its Beta distribution parameters are updated to remember this, and Thompson sampling will more likely draw a high _ctr_ from this arm. _Throughout the experiment, high-performing arms are rewarded with more traffic, whereas under-performing arms are punished with less traffic._

### MAB VS AB TEST

Most classic A/B tests are, by design, forever in ‘exploration’ mode – after all, determining statistically significant results is their reason for existence, hence the perpetual exploration. In an A/B test, the focus is on discovering the exact conversion rate of variations. MAB adds a twist to A/B Testing – exploitation.

![](<.gitbook/assets/image (3) (2).png>)

#### Problems with AB test

* It jumps discretely from exploration to exploitation, when you might be able to transition more smoothly.
* During the exploratory phase (the test), it wastes resources exploring inferior options in order to gather as much data as possible.

### Pros and Cons

The main advantage of bandit experiment is that it terminates earlier than A/B test because it requires much smaller sample. In a two-armed experiment with click-through rate 4% and 5%, traditional will take 223 days but MAB terminates after 31 days. A second advantage of bandit experiment is that the experiment is making fewer mistakes than A/B testing. A balanced A/B test would always send 50% of traffic to each group. The plot above shows that as the experiment progresses, fewer and fewer traffic was sent to the losing arm.

However, the convenience of smaller sample size comes at a cost of a larger false positive rate. Empirically, an α of 5% finds the winning arm about 91% of the time, instead of 95%.



### When to use MAB

#### **1. Opportunity cost of lost conversions is too high**

Imagine you’re selling diamonds (or a car) online. Each lost conversion is probably worth thousands of dollars in lost opportunity for you.

#### **2. Optimizing time-limited events** <a href="#h-2-optimizing-click-through-rates-for-news-outlets-that-cover-time-sensitive-events" id="h-2-optimizing-click-through-rates-for-news-outlets-that-cover-time-sensitive-events"></a>

Optimizing and testing headlines, photo thumbnails, video thumbnails, recommended news articles, and popular articles to drive maximum clicks inside a short window.

#### 3. **Continuous optimization**  <a href="#h-3-continuous-optimization" id="h-3-continuous-optimization"></a>

Optimizers have the ability to add or subtract multiple elements from variations and test across all simultaneously.&#x20;

#### 4. **Optimizing revenue with low traffic** <a href="#h-4-optimizing-revenue-with-low-traffic" id="h-4-optimizing-revenue-with-low-traffic"></a>

If there’s not enough traffic, A/B tests can take really long to produce statistical significance. MAB is able to detect the potentially best version much earlier and increase amount of traffic to it.

### When to use AB Test instead of MAB

#### 1. **When you are aiming for statistical significance**  <a href="#h-1-when-you-are-aiming-for-statistical-significance" id="h-1-when-you-are-aiming-for-statistical-significance"></a>

For all their strengths, MAB experiments are not the best choice when you want to get a statistically robust winner. A/B tests are still the fastest way to statistical significance even though you might lose some conversions in the process.

#### 2. **Optimizing for multiple metrics** <a href="#h-2-optimizing-for-multiple-metrics" id="h-2-optimizing-for-multiple-metrics"></a>

Mature experimentation teams track 4+ goals per experiment, as experiences are composite of primary and secondary goals. While MAB experiments work great when optimizing for one key metric, they don’t work well for multiple goals as they only factor in the Primary Goal while allocating incoming traffic.

#### 3. **Post experiment analysis** <a href="#h-3-post-experiment-analysis" id="h-3-post-experiment-analysis"></a>

Most experimenters like to slice and dice the data gathered during an experiment to check how different segments reacted to modifications on their web properties. This analysis is possible in A/B Tests but might not be possible in MAB as sufficient data might not be available for underperforming variations.

#### **4. Incorporating learnings from all variations (including the poor ones) into further business decisions** <a href="#h-4-incorporating-learnings-from-all-variations-including-the-poor-ones-into-further-business-decisi" id="h-4-incorporating-learnings-from-all-variations-including-the-poor-ones-into-further-business-decisi"></a>

During the course of the test, MAB allocates most traffic to the best performing variation. This means that poor-performing variations do not get enough traffic to reach statistical confidence. So, while you may know with confidence the conversion rate for best performing variation, similar confidence may not be available for poor-performing ones. If getting this knowledge is important for a business decision (perhaps you want to know how bad is the losing variation as compared to the best one), an A/B test is the way to go.

## Reference

{% embed url="https://cxl.com/blog/bandit-tests/" %}

{% embed url="https://vwo.com/blog/multi-armed-bandit-algorithm/" %}

{% embed url="https://multithreaded.stitchfix.com/blog/2020/08/05/bandits/" %}

{% embed url="https://web.eecs.umich.edu/~teneket/pubs/MAB-Survey.pdf" %}
