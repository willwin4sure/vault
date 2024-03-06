#MIT #CS #stats #inference 

[[⛺Decision Theory Homepage|So far]], we've mostly concerned ourselves with binary classification problems.

Now, we turn our attention to problems of estimation, where the parameter(s) of interest are either discrete- or continuous-valued.

## Main Sequence

First, the Bayesian framework. We have a prior over what the hidden random variable should be, and then we reweight it based on the likelihood of the data. Then, we extract a point estimator from the posterior based on a cost function.

1. [[Bayesian Parameter Estimation]]
2. [[Properties of the BLS Estimator]]

Then, the Non-Bayesian framework. Now, the hidden parameter is deterministic but unknown. One common technique is to such to search for the minimum variance unbiased estimator.

3. [[Non-Bayesian Parameter Estimation]]