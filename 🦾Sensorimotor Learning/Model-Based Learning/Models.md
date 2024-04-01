The most basic models are discrete, such as the rules of a turn-based game like tic-tac-toe or Go. This admits solutions such as tree search to solve.

However, we are generally more interested in models of environments with continuous state and action spaces. Our goal is to tackle problems where the model is unknown, and we want to *learn* the model. We can then use this model to train a strong policy. This is potentially more flexible than training a policy directly since we can reuse the model when learning different goals in the same environment.

> [!idea]
> As a reminder, RL algorithms don't require models, but are highly data inefficient. Models allow us to
> * Improve data efficiency.
> * Solve for multiple goals.

Further, even if analytical models already exist, they may be hard to use, so it still might help to learn your own. Finally, it turns out that models help you do better exploration.

## Maximizing Reward

If we already have a trained update rule $\hat{s}_{t+1}=\hat{f}(s_{t},a_{t};\phi)$ (perhaps a neural network with parameters $\phi$), and the reward function $r_{t}=g(s_{t},a_{t})$ is differentiable, then we can do backpropagation to directly maximize the sum of rewards $\sum_{t=1}^{T}r_{t}$.

Of course, if $T$ is large, then optimization will become hard due to vanishing/exploding gradients. One solution is ==receding horizon control==, where you just plan for a smaller horizon $H\ll T$, and then execute the first step, then re-plan and repeat.

What happens if $g$ is not differentiable, or we don't want to deal with gradients? Well, we could just try a bunch of proposed action sequences, and pick the one with the highest reward under our model.

But what should we do to generate these actions? Some techniques include random actions, ==genetic algorithms==, and the ==cross-entropy method==.

## Cross-Entropy Method

We want to sample from a distribution that shifts towards distributions that generate higher rewards. Suppose we have some parameterized sampling distribution. First, generate $N$ sequences of actions from this distribution, and then pick the top-$J$ that produce the highest sum of rewards (called ==elites==). Then, we fit a new distribution to these elites to gradually shift the sampling distribution to higher rewards.
