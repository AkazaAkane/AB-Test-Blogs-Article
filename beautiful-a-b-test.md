---
description: talk given by Marton Trencseni - Sun 05 June 2016
---

# Beautiful A/B Test

## My Thoughts

This article is written by Marton Trencseni, ex facebook data engineer. He talks about some lessons he learns during his experience with ab testing.

## Summary

Marton was hired at Prezi as a data engineer to conduct ab testing. At first, he thought ab testing is just ez: "I thought, isn't it as simple as taking four numbers (the total impressions for A and B, and conversions for A and B), plugging it into some math equations, and reading off the results?" However, it turns out that A/B testing is a much more complex topic. Marton wants to mention what he learns throughout the 3 years.

The definition of ab test is: you have two variants of your SaaS product, and you want to see which one is better. He also mentions HIPPO, which stands for Highest Paid Person's Opinion, and is the antithesis of A/B testing and scientific thought: it's when the big guy decides what happens with the software based on her own personal preferences, and in general it's a bad idea.

The talk is organized as below:

* 4 are things you should think about before doing an A/B test
* 3 are relevant while the test is running
* 2 are things you should do after the A/B test is finished

### Should you even do an experiment?

Don't run an A/B test---which is an experiment to tell whether A or B is better---when you already know from past experiments which one will be better. Note that not doing an experiment doesn’t mean you should not measure the metrics associated with a new version, that’s not the same thing.

### What metrics?

"A mistake many people make is they focus on a more immediate metric like conversion. But usually your desired business outcome is 3 more steps down the line, so you should focus on that. Usually this metric you should look at is related to something important to your business, like revenue, or how many pieces of content people make, or how much time people spend viewing content, and so on; or to give an example from social networking: do people post more, comment more, do they spend more time with the product, and so on."

Marton mentions an example of metrics that only taking into account of revenue, but neglecting refund/returns. Also, he mentions data mining metrics: you can easy get a nice p value when you have a lot of metrics.

He advises that:

* before the test, you should select the target metric, and put it down in your A/B testing tool to keep yourself honest
* when you look at other metrics later, just remember that you're increasing the chance of seeing a false positive
* if you find a lift in another metric that seems like a reasonable effect that you didn't think off, repeat the A/B test with that target metric

But for large sites with a lot of traffics, they may not have this issue.

### Test Early, Fail Early

In Saas environment, test mock version earlier. Etsy invested a lot time an effort on infinite scroll but the A/B test showed it's performing much worse then the original. However, simply showing 80 items instead of showing 40 showed that users don't like that, so they could have saved a lot of development time by performing that simple A/B test first.

### Logging and Harshing

Make sure each user is corresponding to the outcome of interest. Make sure you have a dashboard that shows, based on the logs, what % of your users are seeing which variant.&#x20;

"Hashing just means that for each test, you randomize your users, and divide them into buckets after randomization. Because the randomization is different for each test, the tests will overlap (whether they're running at the same time or one after the other) in random ways, so other tests will "contaminate" your A and B to the same degree, so you can still measure the lift."

### Don't change the experiment while running

When a team releasing new features on version B, and trying to test it. one should restart test instead of using the same treatment and control.&#x20;

* Keep the old ones running "in the background" for long enough to get enough samples.
* Restart the experiment each time, and since you have lower counts, accept lower p-value thresholds, and hence accept more false positives. Remember, there's nothing magic or special about 5%.
* Do release the new version into the running test, but in that case you shouldn't calculate the p-value, just look at the time series for A and B, and if you have enough users, maybe you can see the difference between A and B, and between the versions of B. Or maybe you won't be able to.

### Population Effects

Representative sample: fails when you test free users on a freemium product (cannot measure revenue metrics, and free users are inherently different from paid users) and different countries (only test 12 hours but US awake and CN asleep).

if not enough traffic: change metrics, increase p value and acquire more traffic

### The Math

_Bayesian vs. Frequentist_

The **frequentist view** of the world is an objective view. They say that there is a true conversion rate of A and B, and you could in theory repeat your finite A/B test an infinite number of times, each time collect only a finite number N of samples, and they say you should worry about your false positive and false negative rate. The concept of statistical significance is about false positive rates, the concept of statistical power is related to false negative rates. False positive is when B isn't better than A, but you measure B to be better and keep it. False negative is when B is better than A, but you measure it to be worse or your experiment does not reach significance, so you reject it. So in the frequentist view, you, the observer is not a part of the picture, you're just controlling statistical fluctuations.

The **bayesian approach** is one where the observer is part of the picture. Essentially, it starts by asking what your belief is about A and B's conversion rate, this is called the prior, and then updates this belief after each and every observation. The end-result is something called the posterior, which is the current best-guess about A and B, based on what we've seen.

I would use a mix of bayesian and frequentist today; frequentist to gauge how long to run a test, bayesian to evaluate (but also show frequentist results in small print). Note that large organizations which can afford to hire a team of smart data scientists can essentially go either way, because that team will make sure the decisions are statistically sound.

"There are other approaches like **Multi Armed Bandit**, which is about regret minimization. This says, if one of the variants is performing better, let's show that more often, so as not lose conversions. So it dynamically re-allocated traffic between A and B based on current conversion rates."

**Maserati problem.** A Maserati problem is a "nice to have problem", like "once I'm rich, should I buy a Maserati or a Ferrari". In my experience, choosing between these statistical engines is a nice problem to have, if this is your biggest concern, it means all your logging is good, you have good metric, you have enough traffic, you're not committing any of these fallacies. The flipside is, if you have problems with more basic things like logging, you probably shouldn't obsess too much about the math: as long as you're feeding wrong numbers to the equations, your number one priority should be to fix that, as your numerical results will be wrong or off.

### Reporting and Lift

Standardized reporting is greater than customized email. Keep your raw and projected (to all users) lift separate, and report in a standard way to your stakeholders. (40% for that feature, but only 1% user use it.)

## Reference:

{% embed url="https://bytepawn.com/beautiful-ab-testing.html#beautiful-ab-testing" %}
