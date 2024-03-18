Let you gather a lot more data. Exploration is safer, and we can access privileged information for easier training (e.g. figure out what the true reward is). 

However, the simulator probably does not match reality! 

## Model Misspecification

Suppose the friction coefficient of some surface in the real world does match the simulation you trained on. Then, the performance could be worse. 

In general, you won't know the exact physical parameters!

One solution is to train over a range of parameters, called ==domain randomization==, where you average the loss over samples from reasonable ranges. 

Another idea is to first train in simulation and then do more RL in the real world.




