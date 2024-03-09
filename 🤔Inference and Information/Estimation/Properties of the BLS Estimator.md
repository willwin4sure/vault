## Bias-Covariance Decomposition

Recall that the error of any estimator can be decomposed into a bias and a variance:
$$
\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})=\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\boldsymbol{\mathsf{x}}=\mathbf{b}+(\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})-\mathbf{b}),
$$
where $\mathbf{b}=\mathbb{E}[\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})]$ is the ==bias== in the estimator, and the other term has zero mean. The mean-square estimation error is the trace of the error correlation matrix
$$
\mathbb{E}[\mathbf{e}\mathbf{e}^{T}]=\mathbf{\Lambda}_{\mathbf{e}}+\mathbf{b}\mathbf{b}^{T},
$$
where
$$
\mathbf{\Lambda}_{\mathbf{e}}=\mathbb{E}[(\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})-\mathbf{b})(\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})-\mathbf{b})^{T}]
$$
is the error covariance matrix associated with the estimator. Both the bias and variance contribute to the mean-square estimation error.

## Bias and Covariance of the BLS Estimator

> [!claim]
> The BLS estimate is unbiased.

> [!proof]- Obvious
> The bias is $\mathbb{E}[\hat{\mathbf{x}}_{BLS}(\boldsymbol{\mathsf{y}})-\boldsymbol{\mathsf{x}}]=\mathbb{E}[\mathbb{E}[\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}]]-\mathbb{E}[\boldsymbol{\mathsf{x}}]=0$.

Therefore, the MSE performance of the estimator is the trace of the error covariance matrix $\mathbf{\Lambda}_{BLS}$.

> [!claim]
> The error covariance matrix is the expected covariance of the posterior belief, i.e.
> $$
> \mathbf{\Lambda}_{BLS}=\mathbb{E}[\mathbf{\Lambda}_{\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}}(\boldsymbol{\mathsf{y}})].
> $$

> [!proof]- Obvious
> Conditional on some observation $\boldsymbol{\mathsf{y}}=\mathbf{y}$, the error covariance is just the covariance of the posterior belief. Average over this.

## Orthogonality Characterization

> [!theorem] (BLS error is orthogonal to data)
> An estimator $\hat{\mathbf{x}}(\bullet)$ is the Bayes least-squares estimator if and only if the associated estimation error $\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})=\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\boldsymbol{\mathsf{x}}$ is orthogonal to any vector-valued function $\mathbf{g}(\bullet)$ of the data, i.e.
> $$
> \mathbb{E}[g(\boldsymbol{\mathsf{y}})^{T}(\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\boldsymbol{\mathsf{x}})]=0.
> $$

> [!proof]-
> The condition is equivalent to
> $$
> \mathbb{E}[g(\boldsymbol{\mathsf{y}})^{T}(\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\mathbb{E}[\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}])]=\mathbf{0}.
> $$
> for all $g$. Now the forward direction is clear. For the backward direction, note that we can just choose $g(\boldsymbol{\mathsf{y}})=\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\mathbb{E}[\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}]$ to finish.

> [!remark]
> We've actually already seen this in [[18.675]]: if $\boldsymbol{\mathsf{x}}$ and $\boldsymbol{\mathsf{y}}$ are both square-integrable random variables, then the expected value of $\boldsymbol{\mathsf{x}}$ conditional on $\boldsymbol{\mathsf{y}}$ is the orthogonal projection in $L^{2}$ of $\boldsymbol{\mathsf{x}}$ onto the space of $\sigma(\boldsymbol{\mathsf{y}})$-measurable random variables.

> [!idea]
> Since the error $\mathbf{e}(\boldsymbol{\mathsf{x}},\boldsymbol{\mathsf{y}})$ is uncorrelated with *any* function of the data we might construct, there is no further processing that can be done on the data to further reduce the error covariance in the estimate.

Some bonus reading: ^45fa60

1. [[Implementation of Bayesian Estimation]]
2. [[BLS Covariance Inequality]]

---

**Next:** [[Non-Bayesian Parameter Estimation]]