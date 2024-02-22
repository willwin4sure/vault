This reaches the [[Stochastic Bandits#^3bac7a|minimax lower bound]] of the stochastic bandit problem up to log factors.
## Intuition

Currently, with the ETC algorithm, we are keeping track of sample means $\hat{\mu}_{i}$, but the issue is that at commit time, we miss out on some better arm, which has $\hat{\mu}_{i}<\mu_{i}$.

We need to prevent this, and will do so by constructing an estimator $\hat{\mu}_{i}+\varepsilon_{i}$ that is greater than $\mu_{i}$ with high probability (our ==upper confidence bound==, if you will). Then, at any time step, we will pick the action with maximal $\hat{\mu}_{i}+\varepsilon_{i}$, and update estimates.

In particular, if we assume that the distributions are Gaussian, then we can Chernoff bound
$$
\mathbb{P}(\hat{\mu}_{i}+\varepsilon_{i}\leq \mu_{i})\leq e^{-k_{i}\varepsilon_{i}^{2}/2\sigma^{2}},
$$
where $k_{i}$ is the number of times that action has been sampled so far (far-off events are exponentially suppressed; you can check the inequality holds for Gaussians). This justifies the choice of
$$
\varepsilon_{i}=\sqrt{ \frac{2\log\left( \frac{1}{\delta} \right)}{k_{i}} },
$$
which sets the right hand side above to $\delta$ (assuming that rewards are $1$-subgaussian). A usual choice is $\delta=\frac{1}{t^{2}}$. 
## Algorithm Description

> [!example] Example (UCB Algorithm)
> At every point in time, track the sample means $\hat{\mu}_{i}$ of each arm, as well as the number of times $k_{i}$ it has been sampled. Then, at time $t$, pick the action corresponding to the arm with maximal
> $$
> \hat{\mu}_{i}+\sqrt{ \frac{4\log t}{k_{i}} }.
> $$

This uses a nice balance of exploitation (the first term) and exploration (the second term). 

> [!claim]
> The regret of UCB is $R_{T}(\pi)\sim(NT\log T)^{1/2}$.

---

**Next:** [[Contextual Bandits, LinUCB, and Square CB]]