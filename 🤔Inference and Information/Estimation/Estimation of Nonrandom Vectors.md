This section is dedicated to extending all the recent work on non-Bayesian parameter estimation to vectors. 

> [!theorem] (Cramér-Rao Bound, vector version)
> Suppose $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\bullet)$ for each $\mathbf{y}\in \mathcal{Y}$ is positive and differentiable on $\mathcal{X}\subset \mathbb{R}^{K}$, and satisfies the regularity condition
> $$
> \mathbb{E}\left[ \frac{ \partial }{ \partial \mathbf{x} } \ln p_{\boldsymbol{\mathsf{y}}}(\boldsymbol{\mathsf{y}};\mathbf{x}) \right] = 0,\quad\text{for all }\mathbf{x}\in \mathcal{X}.
> $$
> Then, the covariance matrix $\boldsymbol{\Lambda}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})$ of any unbiased estimator satisfies the matrix inequality
> $$
> \boldsymbol{\Lambda}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})\geq\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x}),\quad\text{for all }\mathbf{x}\in \mathcal{X},
> $$
> i.e. $\boldsymbol{\Lambda}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})-\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x})$ is positive semidefinite, where $\mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})$ is now the ==Fisher information matrix== 
> $$
> \mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})=\mathbb{E}\left[ \mathbf{S}(\boldsymbol{\mathsf{y}};\mathbf{x})^{T}\mathbf{S}(\boldsymbol{\mathsf{y}};\mathbf{x}) \right] 
> $$
> for each $\mathbf{x}\in \mathcal{X}$, where
> $$
> \mathbf{S}(\mathbf{y};\mathbf{x})=\frac{ \partial }{ \partial \mathbf{x} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})
> $$
> is the ==score vector== (it is a row vector).

