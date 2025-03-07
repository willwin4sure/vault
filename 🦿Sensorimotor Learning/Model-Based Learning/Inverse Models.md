## Motivation

One problem we've been discussing is a forward model in pixel space. Given an action $a_{t}$ and image $x_{t}$ as input, the goal is to predict the next frame of the video $x_{t+1}$. This is basically the task that Sora tries to solve, but instead of action inputs it uses a prompt.

However, is this actually the right problem to solve? For example, if I drop a glass bottle on the ground, this model will try to predict the exact locations of where all the glass shards go. This is a very hard problem, since there is chaos! Entropy increases.

Maybe we do want to know that there is glass on the floor and it is dangerous, but we don't actually care about the specifics of where every piece of glass goes. 

One open problem is to define the appropriate feature space (e.g. just a boolean for saying if the bottle broke, instead of where each shard went), but this is not scalable right now. 

## Inverting

How are we actually using our forward model? Well, at the end of the day, we're just trying to find the best set of actions $a_{t:T}$ so that from $x_{t}$, $x_{T}$ is as close to $x_{G}$ as possible. 

What if we just directly predicted the actions? Instead, our model will take $x_{t}$ and $x_{t+1}$ and try to determine $a_{t}$.

Usually, we will use a ==Siamese network architecture==, popularized by Hadsell in 2006. We'll have shared weights $\theta$ that process both $x_{t}$ and $x_{t+1}$ into $z_{t}$ and $z_{t+1}$, and then use those to predict $a_{t}$. The hope is that $x\mapsto z$ is also a good feature embedding in terms of what information in the state actually matters for actions. ^b1c6ad

This works for one time step, but how do we get all the way from $x_{t}$ to $x_{G}$ over multiple steps? Well, just plug both of them in, then execute the output action on $x_{t}$, then plug $x_{t+1}$ and $x_{G}$ in, and repeat. 