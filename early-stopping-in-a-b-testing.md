# Early stopping in A/B testing

## Thoughts

### Procedure for Frequentist ab test

* decide what metric we will use to evaluate the test (eg. conversion, timespent, DAU)
* decide how many N samples we will collect
* decide what type of test (eg. t-test or χ2) we will use
* decide α acceptable false positive rate (FPR)
* collect N samples
* compute p value, if p<α reject the null hypothesis, else accept it

### Early Stopping

What happens if the tester is curious or impatient and follows a different protocol and peeks at the data repeatedly to see if it’s “already significant”. So instead of steps 5-6 above, they:

* collect N1 samples, run hypothesis test on N′:=N1 samples, compute p value, if p<α
* reject the null hypothesis and stop, else go on collect N more samples, run hypothesis test on N′:=N+N2 samples, compute p value, if p<α reject the null hypothesis and stop, else go on
* collect N3 more samples... stop if N′>=N

This is the problem with early stopping! **If we repeatedly perform the significance test at the same α level, the overall α level will be higher.** If we do this, we will on average have a higher false positive rate than we think. In the above simulation, with 2 extra peeks, at equal N intervals, the FPR roughly doubles.

The guarantee of frequentist hypothesis testing (as discussed in the past posts), is that, if we evaluate the data at the end (at N=10,000 on the chart, at the end), if the null hypothesis is true, then on average in 1−α fraction of cases the p value will be bigger than α, and we will make the correct decision to accept the null hypothesis (the correct decision). But there is no guarantee about the trajectory of the p value in between. The trajectory is by definition random, so if we repeatedly test against the p=0.05 line with an early stopping protocol, then we will reject the null hypothesis (the incorrect decision) more often.

### Solution

#### Not Stop

A option is to not do this, never to stop the experiment until the original sample size of N is reached. This is what I recommend to do. Note that peeking itself is not a problem, as long as we don’t stop the experiment. Some A/B testing tools go the extra mile and don’t show (blur out) the result before the agreed-upon sample size is reached.

#### Alpha spending in sequential trials

In itself, an early stopping protocol is not a problem. In the above example, we saw that taking 2 extra peeks at equal N intervals with early stopping at α=0.05 each yields an overall α of \~0.10. As long as we know that the overall α of our protocol is what it is, we’re fine. The problem is if we’re not aware of this, and we believe we’re actually operating at a lower α, and potentially report a lower α along with the results.

What if we are mindful of the increase in α that early stopping induces, but we want to keep the overall (=real) α at a certain level, let’s say α=0.05. Based on the previous simulation, intuitively, this is possible, we just have to test at lower α at each early stopping opportunity. This is called [alpha spending](https://en.wikipedia.org/wiki/Sequential\_analysis), because it’s like we have an overall budget of α, and we’re spending it in steps. **Note that alpha spending is not additive!**

The Pocock boundary gives a p-value threshold for each interim analysis which guides the data monitoring committee on whether to stop the trial. The boundary used depends on the number of interim analyses. **** The disadvantages are that the number of interim analyses must be fixed at the start and it is not possible under this scheme to add analyses after the trial has started. Another disadvantage is that investigators and readers frequently do not understand how the p-values are reported: for example, if there are five interim analyses planned, but the trial is stopped after the third interim analysis because the p-value was 0.01, then the overall p-value for the trial is still reported as <0.05 and not as 0.01.

![Pocock boundary ](<.gitbook/assets/image (3).png>)

The Haybittle–Peto boundary is much simpler, but it’s not an exact rule. It essentially says, perform the in-between tests at a very low α=0.001, and the final test at the desired α=0.05. Because the the early steps were performed at such low α, it doesn’t change the overall α by much. What this is essentially saying is: if you peek early and the null hypothesis is without a reasonable doubt wrong, ie. the treatment is without a reasonable doubt better than the control group already at lower Ns, then stop, else keep going. More mathy, peak at some low α=ϵ, and your additional overall α will be some δϵ that is also low.

![](<.gitbook/assets/image (4).png>)

### Conclusion

Increased false positive rate due to early stopping is beautiful nuance of statistical testing. It is equivalent to running at an overall higher α. Data scientists need to be aware of this phenomenon so they can control it and keep their organizations honest about their experimental results.

## Reference:

{% embed url="https://gopractice.io/data/peeking-problem/" %}

{% embed url="https://bytepawn.com/early-stopping-in-ab-testing.html#early-stopping-in-ab-testing" %}
