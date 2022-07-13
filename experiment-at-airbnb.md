---
description: By Jan Overgoor
---

# Experiment at Airbnb

## How long do you need to run an experiment? <a href="#c460" id="c460"></a>

A very common source of confusion in online controlled experiments is how much time you need to make a conclusion about the results of an experiment. The problem with the naive method of using of the p-value as a stopping criterion is that the statistical test that gives you a p-value assumes that you designed the experiment with a sample and effect size in mind. If you continuously monitor the development of a test and the resulting p-value, you are very likely to see an effect, even if there is none. Another common error is to stop an experiment too early, before an effect becomes visible.

Here is an example of an actual experiment we ran. We tested changing the maximum value of the price filter on the search page from $300 to $1000 as displayed below. The p-value curve hits the commonly used significant value of 0.05 after 7 days. But we kept the experiment running and we found that actually, the experiment ended up neutral.Why did we know to not stop when the p-value hit 0.05? It turns out that this pattern of hitting “significance” early and then converging back to a neutral result is actually quite common in our system.&#x20;

There are various reasons for this. Users often take a long time to book, so the early converters have a disproportionately large influence in the beginning of the experiment. Also, even small sample sizes in online experiments are massive in the scale of classical statistics in which these methods were developed. Since the statistical test is a function of the sample- and effect sizes, if an early effect size is large through natural variation it is likely for the p-value to be below 0.05 early. But the most important reason is that you are performing a statistical test every time you compute a p-value and the more you do it, the more likely you are to find an effect.

As a side note, people familiar with our website might notice that, at time of writing, we did in fact launch the increased max price filter, even though the result was neutral. We found that certain users like the ability to search for high-end places and decided to accommodate them, given there was no dip in the metrics.

How long should experiments run for then? To prevent a false negative (a Type II error), the best practice is to determine the minimum effect size that you care about and compute, based on the sample size (the amount of new samples that come every day) and the certainty you want, how long to run the experiment for, before you start the experiment.

One problem, though, is that we often don’t have a good idea of the size, or even the direction, of the treatment effect. It could be that a change is actually hugely successful and major profits are being lost by not launching the successful variant sooner. Or, on the other side, sometimes an experiment introduces a bug, which makes it much better to stop the experiment early before more users are alienated.

We solved the problem of how to figure out the p-value threshold at which to stop an experiment by running simulations and deriving a curve that gives us a dynamic (in time) p-value threshold to determine whether or not an early result is worth investigating.&#x20;

_Figure 6_ — An example of a dynamic p-value curve.

![](https://miro.medium.com/max/1400/0\*bXK9atg4zQpVBm1C.png)

## Understanding results in context <a href="#e884" id="e884"></a>

A second pitfall is failing to understand results in their full context. In general, it is good practice to evaluate the success of an experiment based on a single metric of interest. This is to prevent cherry-picking of ‘significant’ results in the midst of a sea of neutral ones. However, by just looking at a single metric you lose a lot of context that could inform your understanding of the effects of an experiment.

Let’s go through an example. Last year we embarked on a journey to redesign our search page. In _Figure 7_ you can see the before and after stages of the project. The new design puts more emphasis on pictures of the listings (one of our assets since we offer professional photography to our hosts) and the map that displays where listings are located.

![](https://miro.medium.com/max/1400/0\*kYQEaxwC-lLSUL4l.png)

_Figure 7_ — Before and after a full redesign of the search page.

A lot of work went into the project, and we all thought it was clearly better; our users agreed in qualitative user studies. After waiting for enough time to pass, as calculated with the methodology described in the previous section, we ended up with a neutral result. The change in the global metric was tiny and the p-value indicated that it was basically a null effect. However, we decided to look into the context and to break down the result to try to see if we could figure out why this was the case. Because we did this, we found that the new design was actually performing fine in most cases, except for Internet Explorer. We then realized that the new design broke an important click-through action for certain older versions of IE, which obviously had a big negative impact on the overall results. When we fixed this, IE displayed similar results to the other browsers, a boost of more than 2%.

![Figure 8 - Results of the new search design](https://miro.medium.com/max/1100/0\*W-Qnxk2EQR3gzSeA.png)_Figure 8_ — Results of the new search design.

Apart from teaching us to pay more attention to QA for IE, this was a good example of what lessons you can learn about the impact of your change in different contexts. You can break results down by many factors like browser, country and user type.&#x20;

But doing this in the classic A/B testing framework requires some care. It’s very common to be looking at a neutral experiment, break it down many ways and to find a single ‘significant’ effect. Declaring victory for that particular group is likely to be incorrect. The reason for this is that you are performing multiple tests with the assumption that they are all independent, which they are not.

## Assuming the system works <a href="#93ae" id="93ae"></a>

The third and final pitfall is assuming that the system works the way you think or hope it does. This should be a concern if you build your own system to evaluate experiments as well as if you use a third party tool.&#x20;

A simple example of this process is to run an experiment where the treatment is equal to the control. These are called A/A or dummy experiments. In a perfect world the system would return a neutral result (most of the time). The results that we saw for these dummy experiments are displayed in _Figure 10_.

![Figure 10 - Results of a number of dummy experiments](https://miro.medium.com/max/1400/0\*wsfrJNYzgiPl41Tq.png)_Figure 10_ — Results of a number of dummy experiments.

You can see that in the experiments where the control and treatment groups are the same size, the results look neutral as expected (it’s a dummy experiment so the treatment is actually the same as the control). But, for the case where the group sizes are different, there is a massive bias against the treatment group. We investigated why this was the case, and uncovered a serious issue with the way we assigned visitors that are not logged into treatment groups. The issue is particular to our system, but the general point is that verifying that the system works the way you think it does is worthwhile and will probably lead to useful insights.

## Conclusion <a href="#dc60" id="dc60"></a>

Controlled experiments are a great way to inform decisions around product development. Hopefully, the lessons in this post will help prevent some common A/B testing errors.

First, the best way to determine how long you should run an experiment is to compute the sample size you need to make an inference in advance. If the system gives you an early result, you can try to make a heuristic judgment on whether or not the trends have converged. It’s generally good to be conservative in this scenario. Finally, if you do need to make procedural launch and stopping decisions, it’s good to be extra careful by employing a dynamic p-value threshold to determine how certain you can be about a result. The system we use at Airbnb to evaluate experiments employs all three ideas to help us with our decision-making around product changes.

It is important to consider results in context. Break them down into meaningful cohorts and try to deeply understand the impact of the change you made. In general, experiments should be run to make good decisions about how to improve the product, rather than to aggressively optimize for a metric. Optimizing is not impossible, but often leads to opportunistic decisions for short-term gains. By focusing on learning about the product you set yourself up for better future decisions and more effective tests.

Finally, it is good to be scientific about your relationship with the reporting system. If something doesn’t seem right or if it seems too good to be true, investigate it. A simple way of doing this is to run dummy experiments, but any knowledge about how the system behaves is useful for interpreting results. At Airbnb we have found a number of bugs and counter-intuitive behaviors in our system by doing this.
