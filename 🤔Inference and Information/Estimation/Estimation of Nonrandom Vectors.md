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

^12d1e3

> [!remark]
> This is strictly stronger than the scalar version by looking at the diagonal elements.

The proof is essentially the same as the [[The Cramér–Rao Bound#Proof|scalar case]]. 

> [!proof]-
> Define $\mathbf{f}(\mathbf{y})=S(\mathbf{y};x)^{T}$, so it has zero mean and covariance $\mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})$. Then, we can compute the covariance between $\mathbf{f}(\boldsymbol{\mathsf{y}})$ and the error $\mathbf{e}(\boldsymbol{\mathsf{y}})$ as
> $$
> \begin{align*}
> \mathbb{E}\left[ \mathbf{e}(\boldsymbol{\mathsf{y}})\mathbf{f}(\boldsymbol{\mathsf{y}})^{T} \right] &= \int_{-\infty}^{\infty} (\mathbf{\hat{x}}(\mathbf{y})-\mathbf{x})\frac{ \partial }{ \partial \mathbf{x} } p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \, d\mathbf{y} \\
> &= \left[ \frac{ \partial }{ \partial \mathbf{x} } \int_{-\infty}^{\infty} \hat{\mathbf{x}}(\mathbf{y})p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \, d\mathbf{y}  \right] - \left[ \mathbf{x}\frac{ \partial }{ \partial \mathbf{x} } \int_{-\infty}^{\infty} p_{y}(\mathbf{y};\mathbf{x}) \, d\mathbf{y}  \right] \\
> &= \mathbf{I}-\mathbf{0}= \mathbf{I}.
> \end{align*}
> $$
> Now we will consider an arbitrary projection $\mathbf{c}$ where $\tilde{e}(\boldsymbol{\mathsf{y}})=\mathbf{c}^{T}\mathbf{e}(\boldsymbol{\mathsf{y}})$ and
> $$
> \tilde{f}(\boldsymbol{\mathsf{y}})=\mathbf{c}^{T}\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x})\mathbf{f}(\boldsymbol{\mathsf{y}})=\mathbf{f}(\boldsymbol{\mathsf{y}})^{T}\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x})\mathbf{c}.
> $$
> Then, $\text{Var}(\tilde{e}(\boldsymbol{\mathsf{y}}))=\mathbf{c}^{T}\boldsymbol{\Lambda}_{\hat{\boldsymbol{\mathsf{x}}}}(\mathbf{x})\mathbf{c}$ and 
> $$
> \text{Var}(\tilde{f}(\boldsymbol{\mathsf{y}}))=\text{Cov}(\tilde{e}(\boldsymbol{\mathsf{y}}),\tilde{f}(\boldsymbol{\mathsf{y}}))=\mathbf{c}^{T}\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x})\mathbf{c}.
> $$
> Applying the Cauchy-Schwarz inequality finishes.

> [!claim]
> Suppose, in addition to the conditions of the above theorem, $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\bullet)$ is twice-differentiable and satisfies the further regularity condition
> $$
> \mathbb{E}\left[ \left( \frac{ \partial }{ \partial \mathbf{x} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \right)^{T}\left( \frac{ \partial }{ \partial \mathbf{x} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \right) +\frac{ \partial^{2} }{ \partial \mathbf{x}^{2} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \right] = 0. 
> $$
> Then, the Fisher information matrix can be equivalently expressed in the form
> $$
> \mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})=-\mathbb{E}\left[ \frac{ \partial^{2} }{ \partial \mathbf{x}^{2} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \right] 
> $$

^0c941a

We also have the vector analogue for the form of efficient estimators:

> [!claim]
> An unbiased efficient estimate $\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})$ exists if and only if
> $$
> \hat{\mathbf{x}}(\mathbf{y})=\mathbf{x}+\mathbf{J}_{\boldsymbol{\mathsf{y}}}^{-1}(\mathbf{x})\left[ \frac{ \partial }{ \partial \mathbf{x} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) \right]^{T}
> $$
> is a valid estimator, i.e. if and only if the right hand side does not depend on $\mathbf{x}$.

This can again be shown by looking at the equality case of Cauchy-Schwarz.

Finally, the MLE is still king.

> [!claim]
> Suppose $\mathcal{X}$ is open and that an efficient estimator $\hat{\mathbf{x}}_{eff}(\bullet)$ and MLE $\hat{\mathbf{x}}_{ML}(\bullet)$ both exist. Then, the two are identical.

> [!warning]
> Once again, this does *not* mean that the MLE is always efficient. When no efficient estimator exists, the ML estimate can still be computed, but it need not have any special properties. However, it still usually has good asymptotic properties.

---

**Next:** [[Regularized and Constrained ML Estimation]]


