This is a policy gradient method that attempts to reduce variance compared to [[Policy Gradients and REINFORCE#^f6a597|REINFORCE]] by estimating a learned [[Variance Reduction#^8c0666|value function]] at each state and combining it with the [[Variance Reduction#^1b4273|baseline]] technique.

## Algorithm Description

This is the simplest form of the actor-critic method.

> [!example] (The Advantage Actor-Critic (A2C) Method)
> Repeat for $E$ epochs:
> * initialize dataset $D$
> * initialize parameters $\theta$ of policy $\pi_{\theta}(a|s)$, our ==actor==
> * initialize parameters $\phi$ of value function $V_{\phi}(s)$, our ==critic==
> * for iteration $i$ in $[1,N]$:
> 	* reset state to $s_{0}$ drawn from initial distribution
> 	* rollout trajectory $\tau_{i}=\{ s_{1}^{i},a_{1}^{i},r_{1}^{i},\dots \}$ using current policy $\pi_{\theta}$
> 	* append $\tau_{i}$ to dataset $D$
> *  compute the gradient estimate $$g(\theta)=\frac{1}{N}\sum_{i=1}^{N}\left( \sum_{t=1}^{T} \nabla_{\theta}\log \pi_{\theta}(a_{t}^{i}|s_{t}^{i}) \left( r_{t}^{i} + \gamma V_{\phi}(s_{t+1}^{i}) - V_{\phi}(s_{t}^{i}) \right) \right)$$
> * update $\theta \leftarrow \theta+\alpha \cdot g(\theta)$
> * compute the gradient of the [[Variance Reduction#^15eba5|TD error]] and also update $\phi$ 

^3acd54

> [!notation]
> Sometimes, we write $Q(s_{t}^{i},a_{t}^{i})=r_{t}^{i}+\gamma V_{\phi}(s_{t+1}^{i})$ as the ==Q-value function==, and we call the entire term $A(s_{t}^{i},a_{t}^{i})=Q(s_{t}^{i},a_{t}^{i})-V_{\phi}(s_{t}^{i})$ the ==advantage function==. 

The value network $V_{\phi}$ needs to generalize well across states, since we are training it on some trajectories and then expecting it to work well for other trajectories. One way of ensuring this is making the network relatively small.

Also, note that above we are using the value function trained on the last policy in order to evaluate the new policy.

> [!idea]
> We could also update $\phi$ before calculating $g(\theta)$, so $V_{\phi}$ is more accurate.

## Generalized Advantage Estimation

We decided to use Monte-Carlo approximation for a single time step, and then bootstrap off of the value function. This gave the approximation $R_{t}^{\gamma}\approx r_{t}+\gamma V_{\phi}(s_{t+1})$. We could make the rollout longer
$$
R_{t}^{\gamma}\approx r_{t}+\gamma r_{t+1}+\cdots+\gamma^{k}r_{t+k}+\gamma^{k+1}V_{\phi}(s_{t+k+1})=:\hat{R}_{k}
$$
This results in higher variance but less bias. In practice, people will take a weighted mean of all these estimates:
$$
R_{t}^{\gamma}\approx \frac{1}{K}\sum_{k=1}^{K}\lambda_{k}\hat{R}_{k}.
$$
Subtracting off the baseline value $V(s_{t})$ gives the ==generalized advantage estimation==. This does not increase runtime, since all the states need to be run through the value network eventually anyway.

## Parallel Workers

Note that in reinforcement learning, the training data affects the network, which *in turn* affects the training data gathered! This makes getting trapped in local minima a particularly large issue, since you might stay in a local parameter region and then only gather more data about that local parameter region, never breaking free.

> [!idea]
> It is very important to preserve **data diversity**! One idea is to run $W$ workers in parallel, all sampling from the same policy.

Generically, you'd imagine that all the data should be collected, then a single gradient step should be computed synchronously. Yet it turns out that [updating synchronously](https://arxiv.org/pdf/1106.5730.pdf) is a viable option!

> [!example] (The Asynchronous Advantage Actor Critic (A3C) Method)
> Use the above algorithm but with $W$ workers operating in parallel, updating parameters asynchronously. We can even increase data diversity more by encouraging high entropy in actions, adding on the term
> $$
> \nabla_{\theta}\log \pi_{\theta}(a_{t}|s_{t})A(s_{t},a_{t})
> +\beta \nabla_{\theta}H(\pi_{\theta}(a_{t}|s_{t})).
> $$
>

What if we already have an existing dataset run on an unknown policy $\pi_{\psi}(a|s)$. Can we use it well? Not really! The gradient cannot be computed correctly. This is why these algorithms are ==on-policy==. 

---

**Next:** [[Actor-Critic Step Size]]
