We [[Universal Approximation|just saw]] that two-layer neural networks are enough to approximate arbitrary functions. In general, do we want to scale the width or depth of our networks? (*Hint:* the class is called deep learning.)

## Advantages of Scaling Width

* Parallelism in computation.
* Shallow networks are UFAs.
* More interpretable.
* Easier to train (e.g. issues with exploding/vanishing gradients).

## Advantages of Scaling Depth

* ==Depth separation== results say that shallow networks would require exponentially more units to represent functions that deep networks can. An example is given below.

> [!example] (Depth separation for kinks)
> ReLU networks can only represent piecewise linear functions. One example of depth separation is counting the number of kinks in this function, say in the one-dimensional case. Indeed, adding width just adds piecewise linear functions, which only at most *adds* the number of kinks, while applying a ReLU can *double* the number of kinks.

However, there are still problems here:

* Very deep networks might be hard to train.
* Very deep networks might not generalize well.

These fall under the domains of optimization and generalization. Luckily, it turns out that people have solved such problems, and very deep neural networks can be trained well in practice.

## Scaling Laws

From a practical perspective for transformer-based LLMs, width v.s. depth seems not to matter too much, just the number of parameters.

However, there could certainly be confounders in these results, as pointed out by the chinchilla paper, which observes slightly different laws given different learning rate schedules and exposes a need to obsess over minor details in the training process. 

---

**Next:** [[Architectures for Grids]]