A common problem with [[Policy Gradients and REINFORCE|policy gradients]]: if a trajectory results in a high reward, then *all* actions will be boosted by the gradient update. However, some of the actions that you took might have been quite bad! This is the problem of ==credit assignment==. 

This leads to rather high variance in the gradients throughout training: the same initial action could lead to drastically different rewards based on future actions. 

## Discount Factor

The ==discount factor== in REINFORCE actually reduces the variance, since if we get a reward at a certain step, gradients that were further in the past will weight it less. 

This leads to faster convergence from reduced variance, but also a bias. In particular, if $\gamma$ is too small, then we might get a policy that optimizes for short term gain without considering the future potential.

## Rewards to Go

By ==causality==, current actions don't affect past rewards! Therefore, we can replace $R^{\gamma}$, which is a sum from $t'=1$ to $T$ of discounted rewards, with a sum from $t'=t$ to $T$ of discounted rewards, where $t$ is the time step of the action gradient that we are considering.  We will write the rewards-to-go as $R_{t}^{\gamma}$.

## Baselines

^1b4273

We should only increase a logprob if the action gives us a better return than the *average* action from that state. We can introduce a ==baseline== $b$ that does not depend on $\theta$, and then replace the gradient with
$$
\frac{1}{N}\sum_{i=1}^{N}\left( \sum_{t=1}^{T}\nabla_{\theta}\log \pi_{\theta}(a_{t}^{i}|s_{t}^{i})\left( R^{\gamma}(\tau)-b \right) \right).
$$
Does the reduce the variance? Yes, because the baseline $b$ with be correlated with the average reward. We can solve an optimal baseline as
$$
b^{*}=\frac{\mathbb{E}[g(\tau)^{2}R^{\gamma}(\tau)]}{g(\tau)^{2}},
$$
where $g(\tau)=\nabla_{\theta}(\log p_{\theta}(\tau))$. In practice, however, people just do expected reward directly (no reweighting by gradients). The new values $R^{\gamma}(\tau)-b$ that we use are called ==advantages==. 

## Value Functions

^8c0666

We can define the ==value function== $V(s_{t})$ as the expected discounted reward that we will get if we start from state $s_{t}$ (this is a function of our particular policy $\pi$). Then, we can write the rewards-to-go as
$$
R_{t}^{\gamma}=\sum_{t'=t}^{T}\gamma^{t'-t}r_{t'}(s_{t'},a_{t'})=r_{t}(s_{t},a_{t})+\gamma V(s_{t+1}).
$$
If we can get a good estimate for $V$, this greatly reduces the variance, as the variance of $V$ is $0$. A lot of times, people will try to approximate $V$ using a neural network.

Suppose we estimate it with a neural network with parameters $\phi$. We have gathered a bunch of data on states, actions, and rewards. We expect in general that $\hat{V}_{\phi}(s_{t})=r_{t}+\gamma \hat{V}_{\phi}(s_{t+1})$, so we can construct a loss function
$$
L(\phi)=\left\| \hat{V}_{\phi}(s_{t})-\left( r_{t}+\gamma \hat{V}_{\phi}(s_{t+1}) \right)  \right\|_{2}^{2}. 
$$
This reduces the variance but incurs a bias: in RL, people are usually really happy to make this trade.

This loss is called the ==temporal differencing error==. Some people call this "bootstrapping". In the next section, we will use this idea to construct another RL algorithm. ^15eba5

---

**Next:** [[Advantage Actor-Critic Method]]