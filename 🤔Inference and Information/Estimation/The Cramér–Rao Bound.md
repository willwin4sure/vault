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
> The Fisher information is a measure of how much the distribution of the observation changes when the parameter does. Therefore, the higher it is, the better we should be able to resolve $x$ from $\mathbf{y}$.

## Proof

For unbiased estimators, the error $e(\mathbf{y})=\hat{x}(\mathbf{y})-x$ has zero mean and variance $\lambda_{\hat{x}}(x)$. Define $f(\mathbf{y})=S(\mathbf{y};x)$, which has zero mean by the regularity condition. Meanwhile, its variance is the Fisher information $J_{\boldsymbol{\mathsf{y}}}(x)$. 

Therefore, by the Cauchy-Schwarz inequality,
$$
\begin{align*}
\lambda_{\hat{x}}(x)\cdot J_{\boldsymbol{\mathsf{y}}}(x)
&= \mathbb{E}[e(\boldsymbol{\mathsf{y}})^{2}]\cdot \mathbb{E}[S(\boldsymbol{\mathsf{y}};x)^{2}] \\
&\geq \mathbb{E}[e(\boldsymbol{\mathsf{y}})S(\boldsymbol{\mathsf{y}};x)]^{2} \\
&=\int_{-\infty}^{\infty} (\hat{x}(\mathbf{y})-x) \frac{ \partial  }{ \partial x } p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y} \\
&=\left[ \frac{ \partial  }{ \partial x } \int_{-\infty}^{\infty} \hat{x}(\mathbf{y})p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y}  \right] - \left[ x\frac{ \partial  }{ \partial x } \int_{-\infty}^{\infty} p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y}  \right] = 1, 
\end{align*}
$$
since the expected value of the estimator is $x$ and the integral of the density is $1$.

> [!claim]
> Under sufficient regularity conditions, the Fisher information can be equivalently expressed in the form
> $$
> J_{\boldsymbol{\mathsf{y}}}(x)=-\mathbb{E}\left[ \frac{ \partial^{2} }{ \partial x^{2} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \right].
> $$

> [!example] (Basic example of the bound)
> Consider the scalar Gaussian problem
> $$
> \mathsf{y}=x+\mathsf{w},
> $$
> where $\mathsf{w}\sim \mathtt{N}(0,\sigma)^{2}$ and $\mathcal{X}=\mathbb{R}$. To determine the Cramér–Rao bound, compute
> $$
> \ln p_{\mathsf{y}}(y;x)=-\frac{1}{2\sigma^{2}}(x-\mathsf{y})^{2}-\frac{1}{2}\ln(2\pi\sigma^{2}).
> $$
> Then, we can compute
> $$
> J_{\mathsf{y}}(x)=-\mathbb{E}\left[ \frac{ \partial^{2} }{ \partial x^{2} } \ln p_{\mathsf{y}}(y;x) \right] = \frac{1}{\sigma^{2}},
> $$
> so $\lambda_{\hat{x}}(x)\geq\sigma^{2}$ for any unbiased estimator $\hat{x}$. Note that the larger $\sigma^{2}$ is, the sharper the peak of the log probability as a function of $x$, so the greater the Fisher information is. This allows us to potentially find an estimator with lower variance.

^f6b501

## Remarks on the Bound

1. The Fisher information is one of many interrelated information measures we will encounter in the subject.

2. The Fisher information cannot be computed in all problems, i.e. the regularity conditions may not be satisfied, in which case no Cramér–Rao bound exists. For example, the distributions need to be positive and differentiable in $x$. Otherwise, the logarithm in the expression might not even exist.

3. The Fisher information can be more generally interpreted as a measure of curvature: it measures how "peaky" $\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)$ is as a function of $x$. Later development of information geometry will shed additional light on this behavior.

4. Any estimator that satisfies the Cramér–Rao bound with equality must be a MVU estimator. Sometimes, the converse is not true! The Cramér–Rao bound may not be tight: sometimes, no estimator can meet the bound for all $x \in \mathcal{X}$, or even any!

5. The Cramér–Rao bound can be generalized in myriad ways, e.g. for *biased* estimates. There is an analogous bound for random parameters, referred to as the *Bayesian* Cramér–Rao bound. This is on our problem set.

6. For problems in which the regularity condition is not satisfied, a closely related result called the ==Hammersley-Chapman-Robbin (HCR) bound== applies. It is less convenient to use but more general, and the Cramér–Rao bound can be naturally developed as a limiting case of HCR.

## Interpreting the Regularity Conditions

The regularity condition for the Cramér–Rao bound just says that
$$
0=\int_{-\infty}^{\infty} \frac{ \partial  }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \cdot p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y} = \int_{-\infty}^{\infty} \frac{ \partial  }{ \partial x } p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y}.  
$$
Therefore, all we need are the conditions on $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)$ to [[Differentiation Under the Integral Sign|differentiate under the integral sign]] to swap the order of the integral and partial derivative. 

The regularity condition for the alternate form of the Fisher information just says that
$$
\begin{align*}
0&=\int_{-\infty}^{\infty} \left[ \left( \frac{ \partial  }{ \partial x } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \right) ^{2} + \frac{ \partial^{2} }{ \partial x^{2} } \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \right] p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y} \\
&=\int_{-\infty}^{\infty} \frac{ \partial^{2} }{ \partial x^{2} } p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x) \, d\mathbf{y}, 
\end{align*}
$$
so all we need to be able to do are the conditions on $\frac{ \partial }{ \partial x }p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)$ this time.

---

**Next:** [[Efficient Estimators]]