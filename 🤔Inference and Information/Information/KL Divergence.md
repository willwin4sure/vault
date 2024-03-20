In practice, it is not computationally feasible to implement true beliefs, so they need to be approximated. If $p(\bullet)$ represents the true belief, but we must approximate it with some distribution $q(\bullet)$, then the approximation loss can be expressed as
$$
\Delta \mathbb{E}[C(x,q)]=-\mathbb{E}_{p}[\log q(x)]+\mathbb{E}_{p}[\log p(x)]=\mathbb{E}_{p}\left[ \log \frac{p(x)}{q(x)} \right].
$$
We have a name for this quantity.

## Definitions

> [!definition] (KL divergence)
> The ==KL divergence== between two discrete distributions $p(\bullet)$ and $q(\bullet)$ is given by
> $$
> D(p\parallel q)=\sum_{a}^{}p(a)\log \frac{p(a)}{q(a)}.
> $$

^ee603e

This is also called the ==relative entropy== of $q(\bullet)$ with respect to $p(\bullet)$. Note that it is nonnegative and is only equal to $0$ when $p=q$.

> [!idea]
> You should think of $D(p\parallel q)$ as representing the *atypicality* of sampling $p$ if you believe that the distribution is $q$. 
> 

Sadly, the KL divergence is not a true distance function, even though it has some properties that are analogous to one. In particular, it is not even symmetric: $D(p\parallel q)\neq D(q\parallel p)$ in general.

> [!idea]
> In general, if $p_{b}$ is a distribution with big support and $p_{s}$ is a distribution with small support, we have that $D(p_{b}\parallel p_{s})\gg D(p_{s}\parallel p_{b})$. In particular, "it is less surprising to believe in unicorns and see no unicorn in the office than to disbelieve in unicorns and see a unicorn in the office."

Another interpretation of the KL divergence is that it is the number of additional bits required to represent $\mathsf{x}$ by virtue of not using its correct distribution. 

## Basic Properties

> [!claim]
> The following properties hold:
> $$
> D(p\parallel\mathtt{U}(\mathcal{X}))=\log|\mathcal{X}|-H(p),\quad
> I(\mathsf{x};\mathsf{y})=D(p_{\mathsf{x},\mathsf{y}}\parallel p_{\mathsf{x}}p_{\mathsf{y}}).
> $$

The first property is intuitively true because distributions that are closer to uniform carry more entropy.

The second property is intuitively true because $\mathsf{x}$ and $\mathsf{y}$ should carry more mutual information the further their joint law is from the product of their marginals; in particular, if $\mathsf{x}$ and $\mathsf{y}$ were just independent, they would have no mutual information at all.

## Relationship to Fisher Information

It turns out that the [[The Cramér–Rao Bound#^bc7726|Fisher information]] that we saw before characterizes the local behavior of the KL divergence in a parameterized family of distributions.

> [!claim]
> For a given $\mathcal{Y}$ and $\mathcal{X}\subseteq\mathbb{R}$ that contains an open set, suppose the family of distributions
> $$
> \{ p_{\boldsymbol{\mathsf{y}}}(\bullet;x)\in \mathcal{P}^{\mathcal{Y}}:x \in \mathcal{X} \}
> $$
> is such that $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\bullet)$ is positive, twice differentiable for each $\mathbf{y}\in \mathcal{Y}$, and satisfies some regularity conditions. Then, for all $x \in \mathcal{X}$,
> $$
> D\left( p_{\boldsymbol{\mathsf{y}}}(\bullet;x)\parallel p_{\boldsymbol{\mathsf{y}}}(\bullet;x+\delta) \right) = \frac{\log(e)}{2}J_{\boldsymbol{\mathsf{y}}}(x)\delta^{2}+o(\delta^{2})
> $$
> as $\delta\to 0$.

> [!proof]- Taylor expansion, omitted

In the vector case, we have instead that
$$
D(p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})\parallel p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x}+\boldsymbol{\delta}))=\frac{\log(e)}{2}\boldsymbol{\delta}^{T}\mathbf{J}_{\boldsymbol{\mathsf{y}}}(\mathbf{x})\boldsymbol{\delta}+o \left( \| \boldsymbol{\delta} \|^{2}  \right) 
$$
as $\boldsymbol{\delta}\to \mathbf{0}$. Here, the [[Estimation of Nonrandom Vectors#^12d1e3|Fisher information matrix]] is used as a bilinear form, hit on both sides by $\boldsymbol{\delta}$.

## Variational Form

This is an alternate representation that is useful in a variety of contemporary applications.

> [!theorem] (Donsker-Varadhan)
> For $p,q\in \mathcal{P}^{\mathcal{X}}$ and $\text{supp}(p)\subset \text{supp}(q)$ (and finite $\mathcal{X}$), the information divergence can be expressed in the equivalent form
> $$
> D(p\parallel q)=\max_{g:\mathcal{X}\to \mathbb{R}}\left\{ \log(e) \mathbb{E}_{p}\left[ g(\mathsf{x}) \right] - \log \mathbb{E}_{q}\left[ e^{g(\mathsf{x})} \right]  \right\} .
> $$

> [!proof]- Jensen + Hölder. Omitted, but the optimum is $g(x)=\log \frac{p(x)}{q(x)}$.

In general, if we restrict the class of $g$ to a computationally convenient collection, we can obtain a lower bound on the KL divergence.

---

**Next:** [[Other Divergence Families]]