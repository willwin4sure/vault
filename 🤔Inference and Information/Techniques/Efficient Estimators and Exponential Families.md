There is a close connection between exponential families and the existence of [[Efficient Estimators|efficient estimators]]. In fact, exponential families entirely characterize when an MVUE exists.

> [!claim] (Efficient estimators exist iff the model is an exponential family)
> An efficient estimator exists for estimating a nonrandom parameter $x$ from observations $\boldsymbol{\mathsf{y}}$ if and only if the model $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)$ is a member of an exponential family such that
> $$
> \dot{\lambda}(x)=cJ_{\boldsymbol{\mathsf{y}}}(x)
> $$
> for some constant $c>0$. Furthermore, when it exists, the efficient estimator takes the form
> $$
> \hat{x}_{eff}(\mathbf{y})=ct(\mathbf{y})+b
> $$
> for some constant $b$. This also must be the ML estimator $\hat{x}_{ML}(\mathbf{y})$.

^aecfec

> [!proof]- Forward Direction
> If an efficient estimator exists, we know it [[Efficient Estimators#^783e21|takes the form]] of 
> $$
> \hat{x}_{eff}(\mathbf{y})=x+\frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}\frac{ \partial  }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x).
> $$
> Rearranging terms and integrating gives that
> $$
> \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\left( \int^{x} J_{\boldsymbol{\mathsf{y}}}(u) \, du  \right) \hat{x}_{eff}(\mathbf{y})-\int^x uJ_{\boldsymbol{\mathsf{y}}}(u) \, du + \beta(\mathbf{y}),
> $$
> where $\beta(\mathbf{y})$ is an arbitrary function of $\mathbf{y}$. However, this is exactly the form of an exponential family with
> $$
> \lambda(x)=\int^{x} J_{\boldsymbol{\mathsf{y}}}(u) \, du,\quad
> t(\mathbf{y})=\hat{x}_{eff}(\mathbf{y}).
> $$

> [!proof]- Backward Direction
> For the other direction, we first need to show that if $J_{\boldsymbol{\mathsf{y}}}(x)>0$ for all $x$, then $\dot{\lambda}(x)=cJ_{\boldsymbol{\mathsf{y}}}(x)$ if and only if $\mathbb{E}[t(\boldsymbol{\mathsf{y}})]=\frac{1}{c}x+b$ for some constant $b$. However, this is true because the first expression is equivalent to $\frac{d}{dx}\mathbb{E}[t(\mathbf{y})]=\frac{1}{c}$ from a [[Single-Parameter Exponential Families#^42919e|computation last time]].
> 
> Now, we well look at the supposed form of an efficient estimator
> $$
> \begin{align*}
> \hat{x}(\mathbf{y})
> &= x+\frac{1}{J_{\boldsymbol{\mathsf{y}}}(x)}\frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \\
> &= x+\frac{c}{\dot{\lambda}(x)}\left[ \dot{\lambda}(x)t(\mathbf{y})-\dot{\alpha}(x) \right] \\
> &= x + c\left( t(\mathbf{y})-\mathbb{E}[t(\mathbf{y})] \right) \\
> &= x + c\left( t(\mathbf{y})-\frac{x}{c}-b \right) \\
> &= ct(\mathbf{y})-bc,
> \end{align*}
> $$
> which does not depend on $x$, so it is efficient.

> [!proof]- Connection to MLE
> For the MLE, note that
> $$
> \frac{ \partial }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\dot{\lambda}(x)t(\mathbf{y})-\dot{\alpha}(x)
> $$
> is zero when evaluated at $\hat{x}_{ML}(\mathbf{y})$, so
> $$
> t(\mathbf{y})=\frac{\dot{\alpha}(\hat{x}_{ML}(\mathbf{y}))}{\dot{\lambda}(\hat{x}_{ML}(\mathbf{y}))}=\frac{1}{c}\hat{x}_{ML}(\mathbf{y})+b.
> $$
> This agrees with the efficient estimator we saw before, since we had
> $$
> \hat{x}(\mathbf{y})=ct(\mathbf{y})-bc=\hat{x}_{ML}(\mathbf{y}).
> $$

> [!claim] (Specialization to canonical exponential families)
> If the model is from a canonical exponential family, then an efficient estimator exists if and only if the Fisher information $J_{\boldsymbol{\mathsf{y}}}(x)$ is constant, i.e. does not depend on the parameter $x$.

---

**Next:** [[Multi-Parameter Exponential Families]]