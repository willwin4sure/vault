
In image classification, we take in an image, pass it through a neural network, and produce a classification. Reinforcement learning is essentially the same: we could take a game state, pass it through a neural network, and produce an action. 

However, the issue is that we don't know what the *correct* output is from any state. In the image classification task, we can use the ground truth of human labels. What should we do for RL?

==Behavior cloning== uses expert demonstrations by training off of human-generated trajectories. This just uses supervised learning to create a policy.

This is a bit suboptimal... ideally we train off of the rewards directly. In particular, the agent might end up in situations that never occurred in the training distribution (the expert maybe never makes certain mistakes). Even if you emulate the expert perfectly, there could be noise in the environment that pushes you away (==co-variate shift==). One way to solve this is to just collect more data.