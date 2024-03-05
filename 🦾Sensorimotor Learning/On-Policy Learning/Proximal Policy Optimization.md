Instead of enforcing a KL-divergence constraint, we will ensure that the ratio
$$
\frac{\pi_{\theta+d}(a|s)}{\pi_{\theta}(a|s)}
$$
is close to $1$, meaning that the policy is not changing by too much. We realize this penalty by clipping the ratio within a pre-defined bound $(1-\varepsilon,1+\varepsilon)$.

> [!example] (PPO-clip)
> The clipped objective function for PPO is
> $$
> \mathbb{E}_{(s,a)\sim \pi_{\theta}}\left[ \min\left\{ \frac{\pi_{\theta +d}(a|s)}{\pi_{\theta}(a|s)} A^{\pi_{\theta}}(s,a), \text{clip}\left( \frac{\pi_{\theta+d}(a|s)}{\pi_{\theta}(a|s)}, 1-\varepsilon, 1+\varepsilon \right) A^{\pi_{\theta}}(s,a) \right\}  \right] .
> $$
> Here is some pseudocode, also courtesy of [OpenAI](https://spinningup.openai.com/en/latest/algorithms/ppo.html):
> 
> ![[ppoalg.png|center|512]]
> 
> Note here that the form of the objective is slightly different, with
> $$
> g(\varepsilon,A)=
> \begin{cases}
> (1+\varepsilon)A & A\geq 0, \\
> (1-\varepsilon)A & A<0.
> \end{cases}
> $$
> The idea is that if the advantage $A$ is positive, $\pi_{\theta}(a_{t}|s_t)$ is incentivized to increase. However, the amount it can increase is clipped by $\varepsilon$ times the old policy $\pi_{\theta_{k}}(a_{t}|s_{t})$. Similarly, if $A$ is negative, then $\pi_{\theta}(a_{t}|s_{t})$ is incentivized to decrease, but it is also constrained.

---
