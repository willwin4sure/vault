Computing the [[Mixture Models#^1ed9df|least informative prior]] directly is rather challenging. The detailed structure of the prior is also not critical to good performance: what its more important is that the distribution is strictly positive (i.e. lies in the interior of the probability simplex), so that we do not have infinite divergence to points.

We will design a prior that is simpler and independent of $\mathcal{Y}$, called a ==maximally ignorant prior==, which is an attractive alternative to the least informative prior. This is the prior $p$ with the largest possible entropy $H(p)$: it is the most random distribution.

## Finite Alphabet Case

[[KL Divergence#^7f5cbf|Recall]] that for a finite alphabet $\mathcal{Y}$, we have that
$$
D(p \parallel \mathtt{U})=\sum_{y}^{}p(y)\log p(y)+\log|\mathcal{Y}|=\log|\mathcal{Y}|-H(p).
$$
Suppose we have some linear family passing through the distribution of maximum entropy $p^{*}$ in the family, i.e.
$$
\mathcal{L}_{\mathbf{t}}(p^{*})=\left\{ p \in \mathcal{P}^{\mathcal{Y}} : \mathbb{E}_{p}\left[ t_{k}(\mathsf{y}) \right] = \overline{t}_{k} \right\}. 
$$
Then, note that
$$
p^{*} = \arg\max_{p \in \mathcal{L}_{\mathbf{t}}} H(p) = \arg\min_{p \in \mathcal{L}_{\mathbf{t}}} D(p\parallel \mathtt{U}).
$$
Therefore, $p^{*}$ is the [[Information Projection and Pythagoras' Theorem#^936604|I-projection]] of $\mathtt{U}$ onto the linear family $\mathcal{L}_{\mathbf{t}}(p^{*})$. From our development of [[Orthogonal Families#^db7aaf|orthogonal families]], this means that $p^{*}$ is also a member of the exponential family $\mathcal{E}_{\mathbf{t}}(\mathtt{U})$, and can be written as a tilting of the uniform distribution:
$$
p^{*}(y)=\exp \left\{ \sum_{i=1}^{K} x_{i}t_{i}(y)-\alpha(\mathbf{x}) \right\},
$$
where $\mathbf{x}\in \mathbb{R}^{K}$ is the parameter space of $\mathcal{E}_{\mathbf{t}}(\mathtt{U})$.

## Infinite Alphabet Case

This is a lot less clear, since the uniform distribution on $\mathcal{Y}$ might not be well-defined, e.g. in the case where $\mathcal{Y}=\mathbb{R}$. However, analogous results with differential entropy $h(p)$ still hold.

> [!claim]
> Among all distributions over an alphabet $\mathcal{Y}\subseteq \mathbb{R}$ in the linear family $\mathcal{L}_{\mathbf{t}}(p^{*})$ with finite differential entropy, the distribution $p^{*}$ given by the tilted distribution, when it exists, is the unique distribution having maximal differential entropy.

> [!proof]-
> It turns out that
> $$
> h(p)=-D(p\parallel p^{*})+h(p^{*}).
> $$
> This is essentially the [[Linear Families#^747e82|Pythagorean identity]] applied to $p^{*}$ being the I-projection of $\mathtt{U}$ onto the linear family, but for formality the algebra should be verified directly.
> 
> This implies that $h(p)\leq h(p^{*})$ for $p \in \mathcal{L}_{\mathbf{t}}(p^{*})$, with equality only when $p=p^{*}$, since that is when the KL divergence is $0$.

> [!example]
> Consider the linear family
> $$
> \mathcal{L}_{\mathbf{t}}(p^{*}) = \{ p : \mathbb{E}[\mathsf{y}]=\mu, \mathbb{E}[\mathsf{y}^{2}]=\sigma^{2}+\mu^{2} \}.
> $$
> Here, $K=2$ with $t_{1}(y)=y$ and $t_{2}(y)=y^{2}$. Therefore, $p^{*}$ must be of the exponential-quadratic form
> $$
> p^{*}(y)\propto e^{x_{1}y+x_{2}y^{2}},
> $$
> i.e. $\mathsf{y}$ is Gaussian! We can then solve $p^{*}(y)=\mathtt{N}(y;\mu,\sigma^{2})$, which then has [[Information Measures for Gaussians|differential entropy]] $h(p^{*})=\frac{1}{2}\log(2\pi e\sigma^{2})$.

Once again, we see Gaussians show up! 

---

**Next:** [[Conjugate Priors]]
