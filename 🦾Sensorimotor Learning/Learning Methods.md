The following methods range from hard-coded behavior to self-learnt behavior.
## Unimate

Works in factories, not in the real world. Does not generalize.

## Machine Learning

In image classification, we take in an image, pass it through a neural network, and produce a classification. Reinforcement learning is essentially the same: we could take a game state, pass it through a neural network, and produce an action. 

> [!idea]
> The issue is that we don't know what the *correct* output is from any state. In the image classification task, we can use the ground truth of human labels. What should we do for RL?

## Behavior Cloning

==Behavior cloning== uses expert demonstrations by training off of human-generated trajectories. This just uses supervised learning to create a policy.

This is a bit suboptimal... ideally we train off of the rewards directly. In particular, the agent might end up in situations that never occurred in the training distribution (the expert maybe never makes certain mistakes). Even if you emulate the expert perfectly, there could be noise in the environment that pushes you away (==co-variate shift==).

One way to solve this is to just collect more data. For problems like object manipulation, people have been working on efficient ways of collecting data, e.g. by using VR/AR to allow people to directly control the robot, which will track the actions.

## Imitation Learning

==Imitation learning== is closely related, but instead of learning from (observation, action) pairs, we only learn from observations themselves, e.g. videos of people doing tasks.

Learning from videos available online is difficult since it is unclear what the agent in the video is trying to do, but people have tried solving this with computer vision.

## Limitations

Behavior cloning and imitation learning both suffer another major issue: the demonstrations could be sub-optimal! For example, a games-playing bot trained from expert trajectories will be unable to surpass human limits.

Instead, we want a way of letting the machine automatically figure out decision-making rules, based on a more fundamental goal!

Let's move on to more and more self-learnt behaviors.

## Physics Models

==Physics== is a general term referring to the "rules of the game". This would apply for real physical optimization problems, but also for other environments. 

Suppose the current state is $s_{t}$, and we are trying to achieve the goal state $s_{G}$ by time $T$. However, physics constrains us to the state updates $s_{t+1}=f(s_{t},a_{t})$. Then, we want to determine
$$
a^{*}_{t:T}=\text{argmin}_{a_{t:T}}\| s_{T}-s_{G} \|^{2}. 
$$
This is what some people refer to as ==model-based control==. The "model" in the name refers to the fact that we have a model of our physical environment.

Traditional model-based control has the following issues:

1. **Optimization is hard**

For optimization, one approach is to use gradient descent. However, the problems may often be non-convex and dis-continuous (e.g. a robot leg faces different physics if its traveling in the air versus in contact with the ground).

The ==model complexity== will be high, due to issues such as differences in physical properties of the ground or limits of control systems. Human designers may try to design simpler approximate models that are easier to optimize.

2. **Errors from perception accumulate**

Given sensory data, we must *infer* the actual state. We need to use a computer vision model to take in images and produce features such as position or pose. This isn't easy!

3. **Simulation/model may be inaccurate or unavailable**

For example, predicting object motion is not easy! This occurs because of all the assumptions we make when using physical models. Further, the real-world is not repeatable: even if we use the same objects and forces, the trajectories could differ.

Model-based methods can only be as good as the specified models!

==Intuitive models== sidestep the problem of human-specified models by *learning* the model!

## Reinforcement Learning

Is it really necessary to predict the future? Do we really need to learn a model of physics?

==Reinforcement learning== just tries to learn a (parameterized) policy $\pi(s_{t};\theta)$ that produces the next action $a_{t}$. Then, our goal is to find
$$
\theta^{*} = \text{argmax}_{\theta}\ \mathbb{E}\left( \sum_{t=1}^{T}r_{t} \right) .
$$
Some people call this ==model-free learning==, since we don't need a model of the dynamics of the system.

At initialization, $\theta$ is random, which means the produced actions are also random! Now it is really unlikely for us to progress to the goal state, which means there won't be much of a reward signal for us to learn on, either. How would we make $\theta$ better?

This makes them **rather data inefficient**, since the systems just need to perform millions of interactions until they get lucky!

Another issue is **designing a reward function**. We could use some basic distance metric (e.g. between images) from the goal state, but it might be better to train a visual classifier.

Finally, we there are concerns with **safety during training and deployment**. For example, if RL is trying to learn how to drive a car, we'd prefer to do it in a toy environment first rather than the real world (where it will crash a bunch of times at the start).

