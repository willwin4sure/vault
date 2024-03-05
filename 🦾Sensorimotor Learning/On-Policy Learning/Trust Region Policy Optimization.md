[[Actor-Critic Step Size|Last time]], we found that the natural gradient is the correct direction to step in to maximize increase in objective given a constraint on the KL divergence of the new and old policy distributions. However, the constraint $\varepsilon$ that we set is arbitrary, and we would like a way of setting it.

One proposal was to make the step size as large as possible under the constraint that $J(\theta+d)-J(\theta)$ is positive. Directly computing this value is computationally infeasible, so we will need an approximation of this value. 

> [!fact]
> The identity
> $$
> J(\theta+d)-J(\theta)=\mathbb{E}_{\tau \sim p_{\theta+d}} \left[ \sum_{t}^{} \gamma^{t} A^{\pi_{\theta}}(s_{t},a_{t}) \right]
> $$
> holds, where the objective function is the expected sum of discounted rewards
> $$
> J(\theta):=\mathbb{E}_{\tau \sim p_{\theta}}\left[\sum_{t}^{}\gamma^{t}r_{t}\right].
> $$
> 

> [!proof]-
> We can rewrite the right hand side as
> $$
> \mathbb{E}_{\tau \sim \pi_{\theta+d}}\left[ \sum_{t}^{}\gamma^{t}\left( r_{t}+\gamma V^{\pi_{\theta}}(s_{t+1}) - V^{\pi_{\theta}}(s_{t}) \right) \right]. 
> $$
> However, this sum telescopes to
> $$
> \mathbb{E}_{\tau \sim \pi_{\theta+d}}\left[ \sum_{t}^{} \gamma^{t} r_{t} \right] - \mathbb{E}_{\tau \sim \pi_{\theta+d}}\left[ V^{\pi_{\theta}}(s_{0}) \right].
> $$
> However, the left term is exactly $J(\theta+d)$, while the right term is $J(\theta)$, since $s_{0}$ does not depend on the fact that $\tau$ was sampled from the new policy, and our value function is tuned to the old policy.

Note that the trajectories $\tau$ are sampled from the distribution $p_{\theta+d}$ induced by the *new* policy $\pi_{\theta+d}$, while the advantages are computed with respect to the *old* policy $\pi_{\theta}$. Also, the first expression is relatively ad-hoc: since the advantage is already a sum of rewards, it is a *double sum*.

However, this expression is still not amenable to optimization, since we only have trajectories sampled from $\pi_{\theta}$.

> [!idea]
> We will try to fix it with some importance sampling. At the end of the day, we're just forced to approximate using trajectories from the old policy, however.

We can rewrite the term above as
$$
\sum_{t}^{}\mathbb{E}_{s_{t}\sim p_{\theta+d}}\mathbb{E}_{a_{t}\sim \pi_{\theta}}
\frac{\pi_{\theta+d}(a_{t}|s_{t})}{\pi_{\theta}(a_{t}|s_{t})} \left[ \gamma^{t} A^{\pi_{\theta}}(s_{t},a_{t}) \right] =: K(\theta+d)
$$
Now, just approximate the $s_{t}\sim p_{\theta+d}$ with $s_{t}\sim p_{\theta}$, which gives an estimate $\tilde{K}(\theta+d)$. 

> [!claim]
> It turns out that 
> $$
> K(\theta+d)\geq \tilde{K}(\theta+d)-C\cdot KL^{\max}(\pi_{\theta}\parallel\pi_{\theta+d}),
> $$
> where $KL^{\max}(\pi_{\theta}\parallel\pi_{\theta+d})=\max_{s}\left[KL(\pi_{\theta}(a|s)\parallel\pi_{\theta+d}(a|s))\right]$, for some constant $C$.

This tells us that our true reward won't be that much worse than estimated. Therefore, we can write
$$
J(\theta+d) - J(\theta) \approx \mathbb{E}_{(s,a)\sim \pi_{\theta}}\left[ \frac{\pi_{\theta+d}(a|s)}{\pi_{\theta}(a|s)} A^{\pi_{\theta}}(s,a) \right]
$$
This is called the ==surrogate advantage==, and is something that can be computed readily. All we need to do is push $s$ through our new policy $\pi_{\theta+d}$, but the key point is that we don't need to sample new trajectories.

> [!example] (Trust Region Policy Optimization)
> Here is some pseudocode, courtesy of [OpenAI](https://spinningup.openai.com/en/latest/algorithms/trpo.html). This does not use a discount factor $\gamma$.
> 
> ![[trpoalg.png|center|512]]
> 
> Notice the use of a backtracking line search to find the largest step size to that the surrogate advantage is positive (note $\alpha \in(0,1)$).

## Adaptive Lagrangian Method

TRPO uses a line search to guarantee that the surrogate advantage is improving at each natural gradient step. However, this line search is rather expensive!

> [!idea]
> Well, why don't we just use gradient descent to *directly* optimize what we want, instead of imposing a hard constraint? 

Suppose the old policy is $\theta$, and we are trying to construct a new policy $\theta'$. Then, we can write down the optimization problem
$$
\max_{\theta'} \mathbb{E}_{(s,a)\sim \pi_{\theta}}\left[ \frac{\pi_{\theta'}(a|s)}{\pi_{\theta}(a|s)}A^{\pi_{\theta}}(s,a) - \beta\cdot KL(\pi_{\theta}(\bullet|s) \parallel \pi_{\theta'}(\bullet|s)) \right] 
$$
for some hyperparameter $\beta$. Then, we perform adaptive updates on $\beta$: if the KL divergence is lower than some arbitrary threshold, decrease it; if it is above some other threshold, increase it.

In the next section, we'll discuss the popular PPO, which removes the KL-divergence constraints and instead uses clipping. It is faster and performs better!

---

**Next:** [[Proximal Policy Optimization]]