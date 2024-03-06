Oftentimes, it is unnatural to assign a prior distribution to the latent variable of interest, as in the Bayesian framework. Another approach is to treat the variable as deterministic but unknown. 

Let $\mathbf{x}$ denote the vector of parameters that we seek to estimate.

> [!notation]
> To make the parameterization explicit, we write the density for the vector of observations $\boldsymbol{\mathsf{y}}$ as $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})$. The mean vector of this distribution is $\boldsymbol{\mu}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})$ and the covariance matrix is $\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})$.
> 

For now, we focus on mean-square estimation error as the performance measure of interest.

## Valid Estimators

> [!warning]
> We cannot directly adapt the [[Bayesian Parameter Estimation#^4ebcd6|BLS framework]] to handle nonrandom parameter estimation. For example, consider the case of a scalar parameter $x$. If we attempt to construct a minimum MSE estimate, this is
> $$
> \hat{x}(\bullet) = \mathop{\arg\min}_{f(\bullet)}\ \mathbb{E}\left[ (x-f(\boldsymbol{\mathsf{y}}))^{2} \right], 
> $$
> yet $x$ is deterministic now, so we can minimize it by just setting $\hat{x}(\boldsymbol{\mathsf{y}})=x$.

Before, such an estimator would not work exactly: we could take the *mean* of our posterior on $x$ (for example), but we should not be able to view $x$ at all anymore.

> [!definition] (Valid estimator)
> An estimator $f(\bullet)$ such that $\hat{x}=f(\mathbf{y})$ is an estimate of $x$ based on $\mathbf{y}$ is ==valid== if $f(\bullet)$ does not depend on $x$.

## Bias and Error Covariance

## Minimum-Variance Unbiased Estimators

---

**Next:** [[The Cramér–Rao Bound]]