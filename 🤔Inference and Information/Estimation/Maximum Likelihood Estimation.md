> The "best possible" estimator.

Perhaps the most widely used estimators in practice (including for learning the parameters of neural networks) are maximum likelihood estimators.

> [!definition] (Maximum likelihood estimate)
> The ==maximum likelihood estimate== $\hat{x}_{ML}(\mathbf{y})$ for a parameter $x$ based on observations $\mathbf{y}$ is defined via
> $$
> \hat{x}_{ML}(\mathbf{y})=\mathop{\arg\max}_{x \in \mathcal{X}}\ p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x),
> $$
> whenever the right hand side exists.

It turns out that when an efficient estimator exists, it is equivalent to the ML estimator of the problem. 

> [!claim] (MLE is efficient if possible)
> Suppose $\mathcal{X}$ is open, and that an efficient estimator $\hat{x}_{eff}(\bullet)$ and maximum likelihood (ML) estimator $\hat{x}_{ML}(\bullet)$ both exist. Then, the two are identical.

> [!proof]-
> We [[Efficient Estimators#^783e21|already know the form of efficient estimators]] if they exist, which is that
> $$
> \hat{x}_{eff}(\mathbf{y})=x+\frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}\frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x).
> $$
> Since the right hand side must not depend on $x$, we can substitute any value of $x$ we want! Let's pick $x=\hat{x}_{ML}(\mathbf{y})$.
> 
> By existence, we know that $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)$ is positive and differentiable, and the ML estimate satisfies
> $$
> \left[ \frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \right] \bigg|_{x=\hat{x}_{ML}(\mathbf{y})}=0.
> $$
> Note that we are using the fact that $\mathcal{X}$ is open to show that the maximum has derivative zero. Since $J_{\boldsymbol{\mathsf{y}}}(x)>0$ except in the trivial case, we just directly get that
> $$
> \hat{x}_{eff}(\mathbf{y})=\hat{x}_{ML}(\mathbf{y})
> $$
> for all $\mathbf{y}$, as desired.

> [!idea]
> The existence of an efficient estimator is a rather strong condition, since it must achieve the lower bound for *all* possible $x \in \mathcal{X}$. In general, however, the MLE is the best possible estimator.

---

**Next:** [[Estimation of Nonrandom Vectors]]

