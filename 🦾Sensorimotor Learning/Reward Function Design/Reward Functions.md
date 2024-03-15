## Sparse Versus Dense Rewards

Consider the reward function $r_{t}=\mathbf{1}_{\| x_{t}-x_{g} \|<\delta}$ where $x_{g}$ is the goal state.

This is not great for [[Policy Gradients and REINFORCE|policy gradients]], since it is too ==sparse==: there is no reward signal that will cause our parameters to update, and we will keep sampling the same bad trajectories.

A ==denser== reward we could consider is
$$
r_{t}=\begin{cases}
-\| x_{g}-x_{t} \|^{2}+1 & \text{if }\| x_{t}-x_{g} \|\leq\varepsilon, \\
-\| x_{g}-x_{t} \|^{2} & \text{otherwise}.
\end{cases}
$$
This might trap you, however, if there is an obstacle in the way: you could have to escape a local minimum to find the actual optimal path.

Therefore, a better metric would use $\mathcal{D}(x_{t},x_{g})$, some graph distance that takes into account obstacles. Yet obviously this is rather circular: if you already have such a good metric, why do you even need to train an algorithm?

So we have a general tradeoff:

* On one end are sparse rewards which are trivially easy to define (did you succeed?) but lead to difficult exploration for the agent.
* Typical denser rewards *require domain knowledge* but lead to easier exploration for the agent. It can sometimes lead to ==reward hacking== where the agent exploits the reward function for high reward without solving the actual problem.
* On the other end are **oracle** dense rewards, which are infeasible to find except in the simplest of problems.

## Penalties

Of course the discount factor still works, but another idea is to include a small penalty (e.g. $-0.1$) for every step that you take where you're not at the goal. 

We also want to penalize large actions, so we can add $-\| a_{t} \|$ to our reward.

Maybe we also want to penalize hitting walls: we could add a negative penalty when that happens and then reset the episode. You need to be careful not to introduce a local optimum that is hitting a wall and terminating.

## Human Reward Descent

This often goes unappreciated. Real RL practitioners work in a feedback loop of training PPO on some designed reward function, observing results, and then tweaking the reward function to try to achieve qualitatively better results.

## Reward Shaping

One way to reward shape is to set $r_{t}'=r_{t}+\gamma\phi(s_{t})-\phi(s_{t-1})$, using a ==potential function==. This makes the sum of rewards the same. 

In theory, we could choose some other $r_{t}'$ that makes the sum of rewards $R_{t}^{\gamma}$ different, but still lead to the same optimal policy $\pi$.

## Learning from Demonstrations

We want to have our sparse $r_{t}$ and add on a $\lambda r_{D}$ term where $r_{D}$ is some reward constructed from demonstrations.