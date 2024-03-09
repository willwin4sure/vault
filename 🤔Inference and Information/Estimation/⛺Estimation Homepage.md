#MIT #CS #stats #inference 

[[⛺Decision Theory Homepage|So far]], we've mostly concerned ourselves with binary classification problems.

Now, we turn our attention to problems of estimation, where the parameter(s) of interest are either discrete- or continuous-valued.

## Main Sequence

First, the Bayesian framework. We have a prior over what the hidden random variable should be, and then we reweight it based on the likelihood of the data. Then, we extract a point estimator from the posterior based on a cost function.

1. [[Bayesian Parameter Estimation]]
2. [[Properties of the BLS Estimator]]

Then, the Non-Bayesian framework. Now, the hidden parameter is deterministic but unknown. One common technique is to such to search for the minimum variance unbiased estimator. The famous Cramér–Rao bound establishes a lower bound on the variance of such an estimator.

3. [[Non-Bayesian Parameter Estimation]]
4. [[The Cramér–Rao Bound]]

Any estimator that meets this lower bound everywhere is called efficient, a rather strong condition. The MLE meets efficiency, *if any efficient estimator exists*.

5. [[Efficient Estimators]]
6. [[Maximum Likelihood Estimation]]

We also summarize extensions of all the previous results to the vector case.

7. [[Estimation of Nonrandom Vectors]]

Then, we move on to some bonus topics. We discuss maximum likelihood estimation with regularization, which is equivalent to enforcing a prior. We also discuss how to implement hard constraints.

8. [[Regularized and Constrained ML Estimation]]

Finally, we discuss some linear regression topics, standard from statistics or intro machine learning.

9. [[The Gauss-Markov Theorem]]
10. [[Linear Regression]]
11. [[Logistic Regression]]

---

**Next:** [[⛺Techniques Homepage]]