This is an open problem for machine learning: we need models to perform well outside of their training distributions.

In the standard machine learning paradigm, your training distribution matches your test distribution. In the real world, this won't be true, since you can never have a representative sample of the world it will be deployed in (one fundamental problem is that *the world changes*). 

## Adversarial Examples and Training

The classic case where you can take an image and add a small amount of noise to make the classification completely wrong. This noise can be constructed via backpropagation through to the input. This can be done for things like 3D objects and speech as well.

We can define the adversary as solving the optimization
$$
\max_{\delta \in\Delta}\mathcal{L}(f_{\theta}(\mathbf{x}+\delta),\mathbf{y}),
$$
where $\Delta$ is some small feasible set, e.g. constrained in $L^{\infty}$-norm. Then, robust optimization is the solution to the adversarial game
$$
\min_{\theta} \frac{1}{n}\sum_{i=1}^{n}\max_{\delta \in\Delta}\mathcal{L}(f_{\theta}(\mathbf{x}^{(i)}+\delta),\mathbf{y}^{(i)}).
$$
> [!warning]
> In the real world, there are a lot more complicated perturbations that you probably want to remain invariant to, e.g. placing a sticker on an object in an image for classification.

One way of thinking about what machine learning models pick up on is that there are three types of features in our data:

* Useless features.
* Robust features, which correlate well even if perturbed.
* Non-robust features, which correlate but can be flipped via perturbation.

These non-robust features may be predictive, but might not promote generalization. In particular, machine learning models love using shortcuts for classification.

> [!example] (Dogs with bowties)
> In turns out that people like putting bowties on cats. So if you put one on a dog, a model may be tricked into thinking it is a cat.

Note that this is an issue with **data**, not with our models. And there are tons of examples where we have non-desirable predictive patterns:

* Classifying x-rays by figuring out which hospital they came from and modeling each distribution separately.
* Calling tumors more likely to be malignant if there is a ruler in the image.

If you train the model to be robust to adversarial perturbations, you do get invariance to the non-robust features but it is more expensive. Then, the noise required to flip a classifier's prediction is much larger. 

## Distribution Shifts

This is a larger scale picture. Before, we perturb individual data points, but now we consider the problem of perturbing entire distributions. In standard training, we just work over the distribution $\mathbb{P}_{n}$, but not we can consider being robust to small perturbations:
$$
\min_{\theta}\max_{\mathbb{Q}:D(\mathbb{Q},\mathbb{P}_{n})<\varepsilon}\mathbb{E}_{(\mathbf{x},y)\sim \mathbb{Q}}\left[ \mathcal{L}(f_{\theta}(\mathbf{x}),\mathbf{y}) \right].
$$
This is called ==distributionally robust optimization==. 

---

**Next:** [[Transfer Learning]]