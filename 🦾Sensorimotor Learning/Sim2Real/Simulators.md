Let you gather a lot more data. Exploration is safer, and we can access privileged information for easier training (e.g. figure out what the true reward is). 

However, the simulator probably does not match reality! 

## Model Misspecification

Suppose the friction coefficient of some surface in the real world does match the simulation you trained on. Then, the performance could be worse. 

In general, you won't know the exact physical parameters!

One solution is to train over a range of parameters, called ==domain randomization==, where you average the loss over samples from reasonable ranges. 

Another approach is to first train in simulation and then do more RL in the real world.

Another idea called ==system identification== is to try to figure out what the parameters of the world are, by collecting real-world data and fitting to it. However, this is costly since we need to gather real-world data!

## Student-Teacher for Online System Identification

Could we do this automatically? We could give the model access to some privileged information, e.g. the friction of the ground, as an input: this allows the model to do perfectly well. Yet we cannot deploy it since we don't know the friction at inference. However, we call this model the ==teacher model==.

Theoretically, a ==student model== without access to the hidden parameters, but *only the history of the states and actions of the teacher*, could infer the parameters, by looking at how the state evolves given actions. We use this to train a student policy, either by training a model to predict the hidden parameter from the state/action history, or by just feeding in the state/action history directly.

## Curriculum Design

Similar to reward shaping, this is a way of aiding training by providing a sequence of tasks from simple to complex that may make training more stable and effective. 