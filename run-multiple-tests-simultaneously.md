# Run multiple tests simultaneously

Before running multiple tests, you need to ask TWO things.

1. How likely are we in a situation in which there might be interactions between tests?
2. How much overlap is there?

If only 1% of the users see both tests, then who cares. If it’s more like 99%, then ask yourself: “Do I really think there are going to be extreme interactions?” (Probably not.)

So in practice – unless you think there is going to be an issue, or it is a super important issue you are testing, then probably don’t sweat it.

**1. Run multiple separate tests**

Unless you suspect extreme interactions and a huge overlap between tests, this is going to be OK. You’re probably fine to do it, especially if what you test is not paradigm-changing stuff and there’s little overlap.

**2. Mutually exclusive tests**

Most testing tools give you the option to run mutually exclusive tests, so people wouldn’t be part of more than one test. The reason you’d want to do this is to eliminate noise or bias from your results. The possible downside is that it might be more complex to set this kind of tests up, and it will slow down your testing as you’ll need an adequate sample size for each of these tests.

**3. Combine multiple tests into one, run as MVT**

If you suspect strong interaction between tests, it might be better to better to combine those tests together and run them as an MVT. This option makes sense if the tests you were going to run measure the same goal (e.g. purchase), they’re in the same flow (e.g. running tests on each of the multi-step checkout steps), and you planned to run them for the same duration.

MVT doesn’t make sense if Test A was going to be about an offer and Test B experimenting with the main navigation – low interaction.

## Reference

{% embed url="https://cxl.com/blog/can-you-run-multiple-ab-tests-at-the-same-time/" %}

{% embed url="https://blog.conductrics.com/ab-testing-when-tests-collide-2/" %}
