Gaussians are *really* nice. Let's look at their information measures.

> [!claim] (Differential entropy of a Gaussian)
> When $\mathsf{x}\sim \mathtt{N}(\mu,\sigma^{2})$, we have that
> $$
> h(\mathsf{x})=\frac{1}{2}\log(2\pi e\sigma^{2}).
> $$
> Further, if $\boldsymbol{\mathsf{x}}\sim \mathtt{N}(\boldsymbol{\mu},\boldsymbol{\Lambda})$ is multivariate, then
> $$
> h(\boldsymbol{\mathsf{x}})=\frac{1}{2}\log|2\pi e\boldsymbol{\Lambda}|.
> $$

This agrees with the idea that both entropy and variance measures uncertainty. Note the lack of dependence on the mean, which just shifts the distribution.

> [!claim] (Conditional differential entropy of joint Gaussians)
> If $\mathsf{x}$ and $\mathsf{y}$ are jointly Gaussian with means $\mu_{\mathsf{x}}$ and $\mu_{\mathsf{y}}$, variances $\lambda_{\mathsf{x}}$ and $\lambda_{\mathsf{y}}$, and covariance $\lambda_{\mathsf{xy}}$, then the conditional entropy on a particular event is just
> $$
> h(\mathsf{x}|\mathsf{y}=y)=\frac{1}{2}\log(2\pi e\lambda_{\mathsf{x}|\mathsf{y}=y})=\frac{1}{2}\log \left[ 2\pi e\left( \lambda_{\mathsf{x}}-\frac{\lambda_{\mathsf{xy}}^{2}}{\lambda_{\mathsf{y}}} \right)  \right].
> $$
> This does not depend on $y$, so $h(\mathsf{x}|\mathsf{y})$ is also equal to it. For multivariate Gaussians, it takes the form
> $$
> h(\boldsymbol{\mathsf{x}}|\boldsymbol{\mathsf{y}})=\frac{1}{2}\log|2\pi e\left( \boldsymbol{\Lambda}_{\mathsf{x}}-\boldsymbol{\Lambda}_{\mathsf{xy}}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{y}}}^{-1}\boldsymbol{\Lambda}_{\boldsymbol{\mathsf{xy}}}^{T} \right) |.
> $$

This can be shown by recalling that the conditional distribution is given by
$$
p_{\mathsf{x}|\mathsf{y}}(\bullet|y)=\mathtt{N}(\mu_{\mathsf{x}|\mathsf{y}}(y),\lambda_{\mathsf{x}|\mathsf{y}}(y)),
$$
where the mean is given by the "linear regression" formula
$$
\mu_{\mathsf{x}|\mathsf{y}}(y)=\mu_{\mathsf{x}}+\frac{\lambda_{\mathsf{xy}}}{\lambda_{\mathsf{y}}}(y-\mu_{\mathsf{y}}),
$$
and the variance is given by the Pythagorean formula
$$
\lambda_{\mathsf{x}|\mathsf{y}}(y)=\lambda_{x}-\frac{\lambda_{\mathsf{xy}}^{2}}{\lambda_{\mathsf{y}}}.
$$
You've gotten to know this stuff very well from [[Gaussian Spaces and Processes|18.676]]. Putting these two claims together allows us to readily compute the mutual information.

> [!claim] (Mutual information of joint Gaussians)
> If $\mathsf{x}$ and $\mathsf{y}$ are jointly Gaussian, then their mutual information is
> $$
> I(\mathsf{x};\mathsf{y})=\frac{1}{2}\log \left( \frac{1}{1-\rho_{\mathsf{xy}}^{2}} \right),
> $$
> where
> $$
> \rho_{\mathsf{xy}}=\frac{\lambda_{\mathsf{xy}}}{\sqrt{ \lambda_{\mathsf{x}}\lambda_{\mathsf{y}} }}
> $$
> is the ==Pearson correlation coefficient== between $\mathsf{x}$ and $\mathsf{y}$. This is the $R$ in $R^{2}$ values.

There is a more complicated formula for multivariate Gaussians that you can enjoy deriving.

Finally, we can compute the information divergence between two Gaussians.

> [!claim] (Information divergence between two Gaussians)
> For two Gaussian distributions
> $$
> p_{1}=\mathtt{N}(\mu_{1},\sigma^{2}),\quad p_{2}=\mathtt{N}(\mu_{2},\sigma^{2})
> $$
> with the same variance, we have that
> $$
> D(p_{1}\parallel p_{2})=\frac{\log e}{2}\cdot \frac{(\mu_{1}-\mu_{2})^{2}}{\sigma^{2}}.
> $$

A difference in means means a lot, but less so when the variance is large.

---

**Next:** [[Maximally Ignorant Priors]]