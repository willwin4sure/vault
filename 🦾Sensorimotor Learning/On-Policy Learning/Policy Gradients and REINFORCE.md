We are given some parameterized policy $a_{t}=\pi_{\theta}(s_{1:t})$, which creates rollouts $\tau=(s_{i},a_{i},r_{i})$ with probability $p_{\theta}(\tau)$. Our goal is to maximize the expected reward by picking
$$
\mathbb{E}\left[ R(\tau) \right]=\int p_{\theta}(\tau)R(\tau)\, d\tau, 
$$
where $R(\tau)=\sum_{t}^{}r_{t}$.

## Computing the Gradient

To optimize this quantity, we can take the gradient and do some computations:
$$
\begin{align*}
\nabla_{\theta}\int p_{\theta}(\tau)R(\tau) \, d\tau
&=\int \nabla_{\theta}p_{\theta}(\tau)R(\tau) \, d\tau \\
&=\int p_{\theta}(\tau) \frac{\nabla_{\theta}p_{\theta}(\tau)}{p_{\theta}(\tau)} R(\tau) \, d\tau \\
&=\int p_{\theta}(\tau) \nabla_{\theta}\log p_{\theta}(\tau)R(\tau) \, d\tau \\
&= \mathbb{E}\left[ \nabla_{\theta}\log p_{\theta}(\tau)R(\tau) \right].
\end{align*}
$$
> [!idea]
> This looks just like $\mathbb{E}[\nabla_{\theta}\log p_{\theta}(y|x)]$, which is the gradient for the MLE estimator in supervised learning; in this setting, however, we are reweighted each trajectory by its reward, since we do not know the correct labels $y$! We care more about increasing probabilities associated with high reward trajectories.

In particular, in the supervised learning expression, the measure for the expected value does not depend on $\theta$, so we can directly pull out the gradient to see that we would be maximizing the sum of the log probabilities.

Now, we can expand $p_{\theta}(\tau)$ by using Bayes' rule to get
$$
\begin{align*}
\mathbb{E}[\nabla_{\theta}\log p_{\theta}(\tau)R(\tau)]
&=\mathbb{E}\left[ \sum_{t}^{}(\nabla_{\theta}\log p_{\theta}(a_{t}|s_{1:t},a_{1:t-1}))R(\tau) \right],
\end{align*}
$$
since determining the next action is all $\theta$ is involved in, not determining the next state (that falls under the dynamics of the system). Here, we can replace $p_{\theta}$ with $\pi_{\theta}$ since it only depends on the policy. Since there is no dependence on the dynamics of the system, this is called ==model-free==.

We can further use the ==Markov assumption== to make the conditioning only on $s_{t}$, and not the entire history (alternatively, make the state also encode the history). 

## Policy Gradients in Action

> [!example] (The REINFORCE algorithm)
> We are given some state $s_{t}$ and feed it through some neural network with parameters $\theta$, which gives us an action $a_{t}$. We can also backprop for the gradient term 
> $$
> g_{t}=\nabla_{\theta}\log \pi_{\theta}(a_{t}|s_{t}).
> $$
> We collect $N$ ==episodes== of doing this $T$ time steps into the future, storing the rewards along the way to compute the gradient update.
> $$
> \frac{1}{N}\sum_{i=1}^{N}\left( \sum_{t=1}^{T}g_{t}^{i}\cdot R^{\gamma}(\tau) \right),
> $$
> where $R^\gamma(\tau)=\sum_{t}^{}\gamma^{t}r_{t}$ is the sum of discounted rewards for the trajectory. We can then use this to gradient ascent update our parameters $\theta$, using an optimizer like Adam.

^f6a597

There is a bit more to specify: if our action space is discrete, we can use a multinomial policy (output a PMF); if our action space is continuous, we could output parameters of a distribution (e.g. a Gaussian, or a mixture/diffusion). 

> [!idea]
> We could collect $N$ episodes of length $T$ all starting from the same state, or $N$ episodes in sequence with different starting states.

Reinitialization is good if possible, since future states might not be useful if earlier actions are terrible. However, in the real world, reinitialization is not always possible, so there is more recent work on reset-free RL.

---

**Next:** [[Variance Reduction]]