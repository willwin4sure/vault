> Efficiency is a rather strong condition, and we can characterize the form of such an estimator well.

> [!definition] (Efficient estimator)
> A valid estimator is ==efficient== if it satisfies the [[The Cramér–Rao Bound|Cramér–Rao bound]] with equality for all $x \in \mathcal{X}$.

We then have the following:

> [!claim] (Form of efficient estimators)
> An estimator $\hat{x}(\bullet)$ is efficient if and only if it can be expressed in the form
> $$
> \hat{x}(\mathbf{y})=x+\frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}\frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x),
> $$
> where the right hand side must be independent of $x$ for the estimator to be valid.

^783e21

> [!proof]-
> Examine the [[The Cramér–Rao Bound#Proof|proof of the Cramér–Rao Bound]] and notice that equality in Cauchy-Schwarz can only occur if 
> $$
> e(\boldsymbol{\mathsf{y}})=k(x)f(\boldsymbol{\mathsf{y}})
> $$
> always. In particular, we can rearrange this into
> $$
> \hat{x}(\mathbf{y})=x+k(x)\frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x).
> $$
> We are already almost done. An efficient estimator exists if and only if there is some $k(x)$ that makes the above valid, i.e. the right hand side does not depend on $x$. 
> 
> Suppose an efficient estimator exists. We will show that $k(x)$ cannot be arbitrary by using the variance equality
> $$
> \frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}
> =\mathbb{E}[e(\boldsymbol{\mathsf{y}})^{2}]
> =\mathbb{E}[e(\boldsymbol{\mathsf{y}})\cdot k(x)f(\boldsymbol{\mathsf{y}})]
> =k(x)\mathbb{E}[e(\boldsymbol{\mathsf{y}})f(\boldsymbol{\mathsf{y}})]=k(x).
> $$
> Note that this proof only works under the regularity conditions.

> [!claim]
> When they exist, efficient estimators are unbiased, unique, and the [[Non-Bayesian Parameter Estimation#Minimum-Variance Unbiased Estimators|MVU estimator]].

> [!proof]-
> Unbiased follows by taking the expectation of the above, where the regularity condition tells us that the second term has mean zero. Further, we have explicitly written down the form of the estimator, so it is unique. It also meets a lower bound on the variance, so it must be an MVU.

> [!example] (Basic example of existence)
> We continue the [[The Cramér–Rao Bound#^f6b501|example from last time]]. Any efficient estimator would take the form
> $$
> \hat{x}(\mathsf{y})=x+\frac{1}{J_{\mathsf{y}}(x)}\frac{ \partial }{ \partial x } \ln p_{\mathsf{y}}(y;x)=x+\sigma^{2}\left( -\frac{1}{2\sigma^{2}}(2x-2y) \right)=y.
> $$
> Since this doesn't depend on $x$, it is a valid estimator and therefore efficient. This is unbiased and has variance $\sigma^{2}$, equal to the Cramér–Rao bound.

> [!remark]
> Later, we will see a connection to [[Efficient Estimators and Exponential Families#^aecfec|exponential families]].

---

**Next:** [[Maximum Likelihood Estimation]]


