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

Before, such an estimator would not work exactly: we could take the *mean* of our posterior on the random $\boldsymbol{\mathsf{x}}$ (for example), but we should not be able to view $x$ at all anymore.

> [!definition] (Valid estimator)
> An estimator $f(\bullet)$ such that $\hat{\mathbf{x}}=f(\mathbf{y})$ is an estimate of $\mathbf{x}$ based on $\mathbf{y}$ is ==valid== if $f(\bullet)$ does not depend on $\mathbf{x}$.

To generalize to randomized estimators, we just require that the density over estimates does not depend on $\mathbf{x}$. 

## Bias and Error Covariance

We use the notation $\mathbf{e}(\boldsymbol{\mathsf{y}})=\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\mathbf{x}=\hat{\boldsymbol{\mathsf{x}}}-\mathbf{x}$ as the notation for the error.

> [!definition] (Bias and error covariance)
> We define the ==bias== in an estimator $\hat{\mathbf{x}}(\bullet)$ as
> $$
> \mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})=\mathbb{E}[\mathbf{e}(\boldsymbol{\mathsf{y}})]=\mathbb{E}[\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\mathbf{x}]=\left( \int_{-\infty}^{\infty} \hat{\mathbf{x}}(\mathbf{y}) p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \, d\mathbf{y}  \right) - \mathbf{x}.  
> $$
> The ==error covariance== is
> $$
> \boldsymbol{\Lambda}_{\mathbf{e}}(\mathbf{x})=\mathbb{E}\left[ \left( \mathbf{e}(\boldsymbol{\mathsf{y}}) - \mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x}) \right) \left( \mathbf{e}(\boldsymbol{\mathsf{y}}) - \mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x}) \right) ^{T}  \right]. 
> $$
> In both cases, the expectation is with respect to $\boldsymbol{\mathsf{y}}$.

The mean-square estimation error is again the trace of the error correlation matrix $\mathbb{E}\left[ \mathbf{e}(\boldsymbol{\mathsf{y}})\mathbf{e}(\boldsymbol{\mathsf{y}})^{T} \right]$, which in general depends on both the bias and the error covariance:
$$
\mathbb{E}\left[ \mathbf{e}(\boldsymbol{\mathsf{y}})\mathbf{e}(\boldsymbol{\mathsf{y}})^{T} \right] = \boldsymbol{\Lambda}_{\mathbf{e}}(\mathbf{x}) + \mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})\mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})^{T}.
$$
This is called the ==bias-variance decomposition==. Unfortunately, directly trying to minimize the MSE has proven a difficult optimization. It has been common to resort to suboptimum estimators satisfying certain constraints.

> [!definition] (Unbiased estimator)
> An estimator $\hat{\mathbf{x}}(\bullet)$ for a nonrandom parameter $\mathbf{x}\in \mathcal{X}$ is ==unbiased== if $\mathbf{b}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})=0$ for all $\mathbf{x} \in \mathcal{X}$. 

> [!claim]
> Because the underlying parameter $\mathbf{x}$ is fixed, the error covariance is the same as the covariance of the estimator itself.

From now on, we restrict our discussion to scalar parameters $x$. The vector parameter case can be constructed in a component-wise manner, and further insights may be provided later.

## Minimum-Variance Unbiased Estimators

**When it exists**, the MVU estimator for $x$ is defined as follows:

> [!definition] (MVU estimator)
> Suppose $\hat{x}_{*}\in \mathcal{A}$, where
> $$
> \mathcal{A}=\{ \hat{x}(\bullet) : \hat{x}(\bullet)\text{ is valid and }b_{\hat{\mathsf{x}}}(x)=0, \text{ for all } x \in \mathcal{X}\}
> $$
> is the set of valid, unbiased estimators, and suppose that for every $\hat{x}(\bullet)\in \mathcal{A}$ we have that
> $$
> \lambda_{\hat{x}}(x)\geq \lambda_{\hat{x}_{*}}(x),\quad \text{for all }x \in \mathcal{X}.
> $$
> Then $\hat{x}_{*}(\bullet)$ is the ==MVU estimator==, denoted $\hat{x}_{MVU}(\bullet)$.

> [!warning] (MVU might not exist!)
> Sometimes, the set $\mathcal{A}$ is empty—there are no valid unbiased estimators. Other times, $\mathcal{A}$ is not empty, but no estimator has a uniformly smaller variance than all others. Some may be better for some $x \in \mathcal{X}$ yet worse for others. 
> 

Even if $\hat{x}_{MVU}(\bullet)$ exists, it might be hard to find! There is no general procedure to do so, but it is possible in specific cases. It is often useful to exploit a bound on $\lambda_{\hat{x}}(x)$, which we will do in the next section. 

---

**Next:** [[The Cramér–Rao Bound]]