This section develops an important connection between least-squares and maximum likelihood estimation, which occurs for linear models with Gaussian data. 

## Statement

> [!theorem] (Gauss-Markov Theorem)
> Suppose data $\boldsymbol{\mathsf{y}}\in \mathbb{R}^{N}$ depends on parameters $\mathbf{x}\in \mathcal{X}=\mathbb{R}^{K}$ through the model
> $$
> \boldsymbol{\mathsf{y}}=\mathbf{H}\mathbf{x}+\boldsymbol{\mathsf{w}},\quad\boldsymbol{\mathsf{w}}\sim \mathtt{N}(\mathbf{0},\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}),
> $$
> where $\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}$ and $\mathbf{H}$ are full rank. Then, the maximum likelihood estimator
> $$
> \hat{\mathbf{x}}_{ML}(\mathbf{y})=\left( \mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{H} \right)^{-1}\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{y}
> $$
> is the solution to a ==weighted least squares== problem, and it is [[Non-Bayesian Parameter Estimation#Minimum-Variance Unbiased Estimators|MVU]].

> [!claim]
> For the model of the previous theorem when $\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}\propto \mathbf{I}$, the ML estimator is the solution to the ==ordinary least squares (OLS)=== problem
> $$
> \hat{\mathbf{x}}_{ML}(\mathbf{y})=\mathop{\arg\min}_{\mathbf{x}}\| \mathbf{y}-\mathbf{H}\mathbf{x} \|_{2}^{2}.
> $$
> The solution should also look familiar from any statistics or machine learning class.

## Proof

TODO

---

**Next:** [[Linear Regression]]