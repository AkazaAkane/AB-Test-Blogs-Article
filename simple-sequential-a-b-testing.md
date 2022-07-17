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
