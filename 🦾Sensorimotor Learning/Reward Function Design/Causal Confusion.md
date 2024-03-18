Suppose you are training a self-driving car that takes in an image and outputs an action. We are training a simple brake system: we give it data of a human in front of the car and the brake indicator lighting up as the human slams the brakes.

Do we expect this to train well? Well, maybe the model decides to only hit the brakes if and only if the brake indicator light goes off! Then it never hits the brakes! This is called training off of a ==spurious feature==: it allows you to reach a good training and validation loss, despite it being completely wrong!

Even when doing behavior cloning, don't just look at the losses. You need to also look at the reward function, which will manifest the catastrophe.

The issue here is a confusion between causality and correlation. These issues actually come up all the time in supervised learning as well: maybe you want the model to learn that a dog is present by looking for its eyes and a tail, but it actually looks for a tennis ball and grass. 

One resolution method is to give a mask to prevent these issues (remove the information about the brake indicator).

## Copycat Problem

This is a similar issue.

A policy with history (of previous states and actions) has a lower validation loss, but it does way worse in terms of the reward function (on a driving task, it has more collisions and more interventions).

The reason is that expert actions are highly correlated, so you can just ==copycat== recent previous actions. 

How do we solve this issue? Impose a higher weighting on losses at "change points", the few places where you cannot just copy the previous actions. These are the places where the error is high!

So by smoothing ideas, just apply a convex function to the loss before summing! For example, you can square the loss at each data point before averaging and you will punish the large errors at change points a lot more.

