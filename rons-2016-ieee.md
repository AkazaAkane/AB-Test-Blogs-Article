---
description: 'Online Experiments: Lessons Learned'
---

# Ron's 2016 IEEE

## My Thoughts

The article talks about many tricks when conducting AB testing. Overlapping experiments are a consierable issues in large tech companies.

### OEC rather than many metrics

A common pitfall in Web experiments is the use of multiple metrics. A organization should choose a OEC rather than testing 25 metrics at the same time. The OEC can be a simple metric that summarizes important business goals or a weighted combination of metrics, as is often used in credit scores.

### Randomization

#### Server fleets

If controls and treatment are processed by different servers with different speed, then result is biased.

#### Representative environment

The experiment period should cover major trend and can be generalized. Try not to run only partial day/Christmas/weekdays.&#x20;

#### Hashing function

A common way to maintain user experience consistency is to employ a hashing function on a user ID stored in a cookie. Picking a good hashing function is critical—some aren’t random enough. Cryptographic hashes such as MD5 are generally the best. Bad hashing function will raise problem when multiple experiments are run: e.g. treatment in experiment a might only get into treatment group in experiment b.

#### Opt in/opt out

Letting users opt in or out of an experiment invalidates the randomness.



### Overlapping experiments

Concurrent Experiments > Sequential Experiments unless&#x20;

Running multiple univariate experiments concurrently offers several advantages:Univariate analysis is much easier for end users to under- stand, experiments can be turned on and off when bugs are found without shutting down other experiments, and any interactions among the factors can still be estimated as if using a full- factorial design.

### ANALYSIS

#### Bug

Ramping up over a day or two makes it possible to catch egregious errors while the experiment is exposed to a small population.

#### Secondary metrics

Secondary feature, like JavaScript errors, customer-service calls, and Web-page loading time, are important in User Experience.

#### Data cleansing

Bots and crawlers may affect the result.

#### Falling for features

Stop launching a feature that is statistically significantly different but has little business value and launching a feature because it doesn’t negatively impact users. Not only are there maintenance costs involved, but the experiment could be underpowered and might not detect that the feature has a lower OEC.



