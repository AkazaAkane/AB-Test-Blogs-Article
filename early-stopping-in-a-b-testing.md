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

