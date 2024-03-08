You've seen all of this before, but I'll quickly summarize it.

> [!definition] (Linear regression)
> You are finding a ==weight vector== $\boldsymbol{\theta}$ to fit a relationship $v=\boldsymbol{\theta}^{T}\mathbf{u}$ on some dataset
> $$
> \mathcal{D}=\{ (\mathbf{u}_{i},v_{i}) \}_{i=1}^{N}.
> $$
>

## Ordinary Least Squares

A natural approach is to use the ordinary least squares objective, i.e.
$$
\hat{\boldsymbol{\theta}}_{OLS}(\mathcal{D}) = \mathop{\arg\min}_{\boldsymbol{\theta}}\sum_{n=1}^{N}(v_{n}-\boldsymbol{\theta}^{T}\mathbf{u}_{n})^{2}.
$$
The objective can be equivalently expressed as 
$$
\varphi_{OLS}(\boldsymbol{\theta})=\| \mathbf{v}-\mathbf{U}\boldsymbol{\theta} \|_{2}^{2},
$$
where $\mathbf{v}\in \mathbb{R}^{N\times 1}$ and $\mathbf{U}\in \mathbb{R}^{N\times K}$ are our data. Applying the [[The Gauss-Markov Theorem|Gauss-Markov theorem]] tells us that the best fit is
$$
\hat{\boldsymbol{\theta}}_{OLS}(\mathcal{D})=(\mathbf{U}^{T}\mathbf{U})^{-1}\mathbf{U}^{T}\mathbf{v};
$$
you know this. This can also be seen as a projection of $\mathbf{v}$ onto the space spanned by the columns of $\mathbf{U}$.

## Ridge Regression

We use [[Regularized and Constrained ML Estimation#^bc61fe|Tikhonov regularization]] and penalize the objective by $\lambda \| \boldsymbol{\theta} \|_{2}^{2}$. This has the added benefit of solving even if $\mathbf{U}$ is not full rank, and it gives
$$
(\mathbf{U}^{T}\mathbf{U}+\lambda \mathbf{I})^{-1}\mathbf{U}^{T}\mathbf{v}.
$$
In the limit as $\lambda\to 0$, this approaches $\mathbf{U}^{\dagger}\mathbf{v}$, where $\mathbf{U}^{\dagger}$ is the pseudoinverse of $\mathbf{U}$.

## LASSO Regression

This is another shrinkage estimator, where we instead add a [[Regularized and Constrained ML Estimation#Constraints|hard constraint]] that $\| \boldsymbol{\theta} \|_{1}\leq\delta$. This lacks a closed-form solution, but it is a convex problem so can be efficiently solved numerically.

---

**Next:** [[Logistic Regression]]