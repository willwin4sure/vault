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
> For the model of the previous theorem when $\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}\propto \mathbf{I}$, the ML estimator is the solution to the ==ordinary least squares (OLS)== problem
> $$
> \hat{\mathbf{x}}_{ML}(\mathbf{y})=\mathop{\arg\min}_{\mathbf{x}}\| \mathbf{y}-\mathbf{H}\mathbf{x} \|_{2}^{2}.
> $$
> The solution should also look familiar from any statistics or machine learning class.

## Proof

Since $\boldsymbol{\mathsf{w}}$ is Gaussian we know that $\boldsymbol{\mathsf{y}}\sim \mathtt{N}(\mathbf{Hx},\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}})$ is Gaussian as well. Therefore, we can write
$$
\hat{\mathbf{x}}_{ML}(\mathbf{y})=\mathop{\arg\max}_{\mathbf{x}}\ \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})=\mathop{\arg\min}_{\mathbf{x}}\ \varphi_{WLS}(\mathbf{x}),
$$
where $\varphi_{WLS}$ is the cost function of the weighted least squares problem as follows:
$$
\varphi_{WLS}(\mathbf{x})=(\mathbf{y}-\mathbf{H}\mathbf{x})^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}(\mathbf{y}-\mathbf{H}\mathbf{x})=\| \boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1/2}(\mathbf{y}-\mathbf{H}\mathbf{x}) \|_{2}^{2}.
$$
Since this cost function is both convex and nonnegative, the MLE is the unique solution to whenever the gradient is zero, which occurs when
$$
\mathbf{0}=\nabla_{\mathbf{x}}(\mathbf{y}-\mathbf{Hx})^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}(\mathbf{y}-\mathbf{Hx})=2\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{Hx}-2\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{y},
$$
which gives the desired form as claimed.

Now, we will show that it is MVU. It is unbiased because 
$$
\mathbb{E}\left[ \hat{\mathbf{x}}_{ML}(\boldsymbol{\mathsf{y}}) \right] = (\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}\mathbf{H})^{-1}\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}(\mathbf{Hx}+\mathbb{E}[\boldsymbol{\mathsf{w}}])=\mathbf{x},
$$
and its error covariance is given by
$$
\begin{align*}
\boldsymbol{\Lambda}_{ML}
&= \mathbb{E}\left[ [(\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{H})^{-1}\mathbf{H^{T}\boldsymbol{\Lambda}}_{\boldsymbol{\mathsf{w}}}^{-1}\boldsymbol{\mathsf{w}}][(\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{H})^{-1}\mathbf{H^{T}\boldsymbol{\Lambda}}_{\boldsymbol{\mathsf{w}}}^{-1}\boldsymbol{\mathsf{w}}]^{T} \right] \\
&= (\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{H})^{-1}.
\end{align*}
$$
Further, we can check that the Fisher information is
$$
\mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})=\mathbb{E}\left[ -\frac{\partial^{2}}{\partial\mathbf{x}^{2}}\left( -\frac{1}{2}(\mathbf{y}-\mathbf{Hx})\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}(\mathbf{y}-\mathbf{Hx}) \right)  \right] =\mathbf{H}^{T}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{w}}}^{-1}\mathbf{H},
$$
which means that the MLE achieves the efficiency lower bound.

---

**Next:** [[Linear Regression]]