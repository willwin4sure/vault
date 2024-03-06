## Statement

> [!theorem] (Cramér–Rao Bound, scalar version)
> Suppose $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\bullet)$ for each $\mathbf{y}\in \mathcal{Y}$ is positive and differentiable on $\mathcal{X}\subset \mathbb{R}$, and satisfies the regularity condition
> $$
> \mathbb{E}\left[ \frac{ \partial  }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\boldsymbol{\mathsf{y}};x) \right] =0,\quad \text{for all }x \in \mathcal{X}.
> $$
> Then, for any unbiased $\hat{x}(\bullet)$,
> $$
> \lambda_{\hat{x}}(x)\geq \frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}
> $$
> for all $x \in \mathcal{X}$. Here,
> $$
> J_{\boldsymbol{\mathsf{y}}}(x)=\mathbb{E}\left[ S(\boldsymbol{\mathsf{y}};x)^{2} \right]
> $$
> is referred to as the ==Fisher information== in $\boldsymbol{\mathsf{y}}$ about $x$, where
> $$
> S(\boldsymbol{\mathsf{y}};x)=\frac{ \partial  }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)
> $$
> is referred to as the ==score function== for $x$ based on $\mathbf{y}$.

> [!idea]
> The Fisher information is a measure of how much the distribution of the observation changes when the parameter does. 

