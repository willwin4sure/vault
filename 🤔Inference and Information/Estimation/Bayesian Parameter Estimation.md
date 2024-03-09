There is an unknown parameter $\boldsymbol{\mathsf{x}}\in \mathcal{X}$ that we are trying to estimate. To do so, we receive random observations $\boldsymbol{\mathsf{y}}\in \mathcal{Y}$. Both of these are modeled as random variables.

> [!idea]
> Take your prior over $\boldsymbol{\mathsf{x}}$ and rescale it by the likelihood of observing $\boldsymbol{\mathsf{y}}=\mathbf{y}$. Then use a cost function to extract a point estimate $\hat{\mathbf{x}}$ from the posterior.
## Bayesian Framework

We have an *a priori* distribution $p_{\mathsf{x}}(\bullet)$ for the unknown parameter, which represents our belief about $\boldsymbol{\mathsf{x}}$ prior to any measurement $\boldsymbol{\mathsf{y}}$. Further, the observation is characterized entirely by our data model $p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{x}}}(\bullet|\bullet)$. 

> [!claim] (Bayes' rule)
> We can then calculate our *a posteriori* distribution
> $$
> p_{\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}}(\mathbf{x}|\mathbf{y})
> = \frac{p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{x}}}(\mathbf{y}|\mathbf{x})p_{\boldsymbol{\mathsf{x}}}(\mathbf{x})}
> {\int p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{x}}}(\mathbf{y}|\mathbf{x}')p_{\boldsymbol{\mathsf{x}}}(\mathbf{x}') \, d\mathbf{x}' },
> $$
> which corresponds to revising our belief about $\boldsymbol{\mathsf{x}}$ given $\boldsymbol{\mathsf{y}}=\mathbf{y}$. Once again, we're just scaling our prior pointwise by the likelihood, and then renormalizing.

## Point Estimator

Sometimes, we need to collapse the posterior distribution into an actual estimate; we denote this $\hat{\mathbf{x}}(\mathbf{y})$. To do this, we need to specify a cost function $C(\mathbf{a},\hat{\mathbf{a}})$ that specifies the cost of estimating $\mathbf{a}$ as $\hat{\mathbf{a}}$. Then, we can pick
$$
\hat{\mathbf{x}}(\bullet)=\mathop{\arg\min}_{\mathbf{f}(\bullet)}\ \mathbb{E}\left[ C(\boldsymbol{\mathsf{x}},\mathbf{f}(\boldsymbol{\mathsf{y}})) \right].
$$
As before, this is equivalent to minimizing pointwise based on the observed value of $\mathbf{y}$, i.e.
$$
\hat{\mathbf{x}}(\mathbf{y})=\mathop{\arg\min}_{\mathbf{a}}\int_{-\infty}^{\infty} C(\mathbf{x},\mathbf{a})p_{\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}}(\mathbf{x}|\mathbf{y}) \, d\mathbf{x}. 
$$
### Minimum Uniform Cost $\implies$ Mode

The ==minimum uniform cost (MUC) criterion== is given by
$$
C(a,\hat{a})=
\begin{cases}
1 & |a-\hat{a}|>\varepsilon \\
0 & \text{otherwise}
\end{cases},
$$
which corresponds to slicing the distribution with radius $\varepsilon$ around $\hat{a}$ and seeing how much lies outside. To minimize this, we want to pick the place in the distribution that is the highest.

> [!example] (MUC gives mode)
> In the limit as $\varepsilon\to 0$, the MUC estimate is the mode of the posterior belief $p_{\mathsf{x}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y})$, i.e. the ==maximum a posteriori (MAP) estimate==.

> [!proof]- Obvious
> The slicing argument above; in the limit as $\varepsilon\to 0$, we want to pick the actual peak of the distribution.

### $L^{1}$ norm $\implies$ Median

The ==minimum absolute-error (MAE) criterion== is the $L^{1}$ norm $C(a,\hat{a})=|a-\hat{a}|$.

> [!example] (MAE gives median)
> The MAE estimate is the median of the posterior belief $p_{\mathsf{x}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y})$.

> [!proof]- Obvious
> Consider tiny movements; they do nothing if there are as many points to your left as to your right.

### $L^{2}$ norm $\implies$ Mean

The ==mean-square error (MSE) criterion== is the square of the $L^{2}$ norm $C(\mathbf{a},\hat{\mathbf{a}})=\| \mathbf{a}-\hat{\mathbf{a}} \|^{2}$. The resulting estimate is called the ==Bayes least-squares (BLS) estimator==.

> [!example] (MSE gives mean)
> The BLS estimate is the mean of the posterior belief $p_{\mathsf{x}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y})$.

^4ebcd6

> [!proof]- Obvious
> Differentiate the convex loss function.

For a bonus loss function, check out: ^775190

1. [[Robust Bayesian Estimation]]

---

**Next:** [[Properties of the BLS Estimator]]

