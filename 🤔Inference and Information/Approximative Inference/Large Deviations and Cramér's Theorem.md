> Miracles occur in the least miraculous ways, if at all.
>
> — TA Samuel Tenka

^ac5cdc

You've already seen this in 18.675, but we actually go a bit more in depth here.

## Large Deviations

We know that for any function $t:\mathcal{Y}\to \mathbb{R}$ if $\mathsf{y_{n}}$ are i.i.d. samples from a distribution $q$,
$$
\lim_{ N \to \infty } \mathbb{P}\left( \left|\frac{1}{N}\sum_{n=1}^{N}t(\mathsf{y}_{n})-\mu\right|\leq \varepsilon \right)=1
$$
where $\mu=\mathbb{E}[t(\mathsf{y_{n}})]$.

> [!definition] (Large deviation probability)
> For $\gamma>\mu$, the ==large deviation probability== is
> $$
> \mathbb{P}\left( \frac{1}{N}\sum_{n=1}^{N}t(\mathsf{y}_{n})\geq \gamma \right).
> $$
> We are concerned with how this behaves with large $N$.

The key is that the same average is close to $\mu$ when they are [[Typicality#^d0b488|typical]] to $q$, so large deviations must come from sequences typical to another distribution.

We will also show that the dominant atypical event is when
$$
\frac{1}{N}\sum_{n=1}^{N}t(\mathsf{y}_{n})\approx \gamma.
$$
## Cramér's Theorem

> [!theorem] (Cramér's Theorem)
> Let $\boldsymbol{\mathsf{y}}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})\in \mathcal{Y}^{N}$ be a sequence of i.i.d. random variables generated from a distribution $q\in \mathcal{P}^{\mathcal{Y}}$, and let $t:\mathcal{Y}\to \mathbb{R}$ be any statistic such that $\mu=\mathbb{E}_{q}\left[ t(\mathsf{y}) \right]<\infty$. Then, for any $\gamma>\mu$,
> $$
> \lim_{ N \to \infty } -\frac{1}{N}\log \mathbb{P}\left( \frac{1}{N}\sum_{n=1}^{N}t(\mathsf{y}_{n})\geq\gamma \right) = E_{C}(\gamma),
> $$
> where $E_{C}(\gamma)$ is the ==Chernoff exponent== and is defined via
> $$
> E_{C}(\gamma)=D(p(\bullet;x)\parallel q)
> $$
> with the tilted distribution
> $$
> p(y;x)=q(y)e^{xt(y)-\alpha(x)},
> $$
> and with $x>0$ chosen such that
> $$
> \mathbb{E}_{p(\bullet;x)}\left[ t(y) \right] = \gamma.
> $$

^3ce5f3

This essentially states that
$$
\mathbb{P}\left( \frac{1}{N}\sum_{n=1}^{N}t(\mathsf{y}_{n})\geq\gamma \right)\approx 2^{-NE_{C}(\gamma)}. 
$$
Here, $p$ is the "actual" distribution the samples seem to have come from, and it is just $q$ but tilted to make higher values of $t(y)$ more likely. So $E_{C}(\gamma)$ is the divergence between the sampling distribution $q$ and the distribution $p$ for which these large deviation samples are typical for.

> [!claim] (Extension of Cramér)
> Suppose $\boldsymbol{\mathsf{y}}_{1},\dots,\boldsymbol{\mathsf{y}}_{N}$ are i.i.d. samples from a multivariate distribution $q$ over $\mathcal{Y}$. Let $\mathbf{t}:\mathcal{Y}\to \mathbb{R}^{k}$ be a function. Then, when $\mathcal{F}\subseteq \mathbb{R}^{K}$ is regular closed,
> $$
> \lim_{ N \to \infty } -\frac{1}{N}\log \mathbb{P}\left( \frac{1}{N}\sum_{n=1}^{N}\mathbf{t}(\boldsymbol{\mathsf{y}}_{n})\in \mathcal{F} \right) =E_{C}(\mathcal{F}),
> $$
> where
> $$
> E_{C}(\mathcal{F})=\min_{\{ \mathbf{x} : \mathbb{E}_{p(\bullet;\mathbf{x})}[t(\boldsymbol{\mathsf{y}})]\in \mathcal{F} \}}D(p(\bullet;x)\parallel q),
> $$
> with
> $$
> p(\mathbf{y};\mathbf{x})=q(\mathbf{y})e^{\mathbf{x}^{T}\mathbf{t}(\mathbf{y})-\alpha(\mathbf{x})}.
> $$

---

**Next:** [[Method of Types]]

