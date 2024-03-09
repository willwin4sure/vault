> [!notation]
> For a symmetric matrix $\mathbf{A}$, we write $\mathbf{A}\geq 0$ to mean that $\mathbf{u}^{T}\mathbf{A}\mathbf{u}\geq 0$ for every vector $\mathbf{u}$. In other words, $\mathbf{A}$ is positive semi-definite. For a pair of symmetric matrices $\mathbf{A}$ and $\mathbf{B}$, we write $\mathbf{A}\geq \mathbf{B}$ to mean $\mathbf{A}-\mathbf{B}\geq 0$.

> [!theorem]
> Let $\mathbf{\Lambda}_{\mathbf{e}}$ denote the error covariance of an arbitrary estimator $\hat{\mathbf{x}}(\bullet)$. Then, the error covariance of the BLS estimator $\mathbf{\Lambda}_{BLS}$ satisfies
> $$
> \mathbf{\Lambda}_{BLS}\leq \mathbf{\mathbf{\Lambda}}_{\mathbf{e}},
> $$
> with equality if and only if
> $$
> \hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\mathbb{E}[\hat{\mathbf{x}}(\boldsymbol{\mathsf{y}})-\boldsymbol{\mathsf{x}}]=\hat{\mathbf{x}}_{BLS}(\boldsymbol{\mathsf{y}})=\mathbb{E}[\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}}].
> $$

This is saying that the lowest covariance estimator has to be the BLS, plus some global bias. "Lowest" makes sense since if we use it as a bilinear form against any vector with itself, it will result in the smallest variance.

---

**Return:** [[Properties of the BLS Estimator#^45fa60]]