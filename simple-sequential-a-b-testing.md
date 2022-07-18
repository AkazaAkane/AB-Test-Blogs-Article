# Simple Sequential A/B Testing

### Procedure

Sequential sampling allows the experimenter to stop the trial early if the treatment appears to be a winner; it therefore addresses the “peeking” problem associated with eager experimenters who use (abuse) traditional fixed-sample methods.

1\.     At the beginning of the experiment, choose a sample size 𝑁

2\.     Assign subjects randomly to the treatment and control, with 50% probability each.

3\.     Track the number of incoming successes from the treatment group. Call this number 𝑇

4\.     Track the number of incoming successes from the control group. Call this number 𝐶

5\.   If 𝑇−𝐶 reaches 2 √N, stop the test. Declare the treatment to be the winner.

6\.     If 𝑇+𝐶 reaches 𝑁, stop the test. Declare no winner.

The two-sided test is essentially the same, but with an alternate ending:

5\.   If 𝑇−𝐶 reaches 2.25 √N, stop the test. Declare the treatment to be the winner.

6\.   If 𝐶−𝑇 reaches 2.25 √N, stop the test. Declare the control to be the winner.

7\.     If 𝑇+𝐶 reaches 𝑁, stop the test. Declare no winner.

The above procedure can, in some circumstances, reduce the number of observations required for a successful experiment by 50% or more. The procedure works extremely well with low conversion rates (that is, with small Bernoulli parameters). With high conversion rates, it works less well, but in those cases traditional fixed-sample methods should do you just fine.

In the majority of tests where _no_ effect is present, the sequential test will arrive at conclusion more slowly than its fixed-sample counterpart. But the sequential test performs much better when an effect _is_ present. It gives its strongest performance when detecting small lifts. An experiment designed to detect a smaller lift will require fewer observations using a sequential test; an experiment designed to detect a larger lift will on average require fewer observations using a fixed-sample test.

## Reference

{% embed url="https://elem.com/~btilly/ab-testing-multiple-looks/part1-rigorous.html" %}

{% embed url="https://www.evanmiller.org/sequential-ab-testing.html" %}

{% embed url="https://www.evanmiller.org/how-not-to-run-an-ab-test.html" %}
