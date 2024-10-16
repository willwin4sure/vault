This class is on why neural networks generalize. After all, deep nets violate certain classical generalization theory.

> [!definition] (Training and test errors)
> So far, we've just discussed approximation, which aims to minimize the ==empirical risk==
> $$
> \hat{\mathcal{R}}(\theta)=\frac{1}{N}\sum_{i=1}^{N}\mathcal{L}(f_{\theta}(\mathbf{x}_{i}),\mathbf{y}_{i}).
> $$
> However, we actually want our networks to minimize ==population risk==
> $$
> \mathcal{R}(\theta)=\mathbb{E}_{(\mathbf{x},\mathbf{y})\sim \mathcal{P}}\mathcal{L}(f_{\theta}(\mathbf{x}),\mathbf{y}).
> $$

The former is the training error while the latter is the test error. Of course, instead of the full expectation you could just have a hold-out test set.

