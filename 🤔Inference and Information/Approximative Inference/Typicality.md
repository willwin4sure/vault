> There are about $2^{NH(p)}$ typical sequences that are about equally likely. Different distributions will have disjoint typical sets.

We now turn from stochastics to asymptotics: we want our approximate inference techniques to approach exact inference in the appropriate limit. We will restrict our attention to models with i.i.d. samples.

## Weak Law of Large Numbers

The weak version of the [[Strong Law of Large Numbers|strong law of large numbers]]. WLLN just says convergence in probability, but SLLN gives almost sure convergence.

> [!theorem] (WLLN)
> Let $\mathsf{w}_{i}$ be i.i.d. [[Lebesgue Integration#^9d6fef|integrable]] random variables with mean $\mu$. Then, the sample mean $\frac{1}{N}\sum_{n=1}^{N}\mathsf{w}_{n}$ [[Convergence of Measurable Functions#^dfe486|converges in probability]] to $\mu$.

If we assume the $\mathsf{w}_{i}$ have finite variance, there is a simple proof of this using Chebyshev
$$
\mathbb{P}(|\mathsf{u}-\mathbb{E}[\mathsf{u}]| > \varepsilon)\leq \frac{\text{Var}[\mathsf{u}]}{\varepsilon^{2}}
$$
on the random variable $\mathsf{u}=\frac{1}{N}\sum_{n=1}^{N}\mathsf{w}_{n}$, noting that the variance decays with $N^{-1}$.

In this case, we see that the convergence rate is at least as fast as $\frac{1}{N}$. Yet it turns out that convergence is actually much faster—exponential in $N$, in fact.

## Typical Sequences

The weak law of large numbers can actually tell us what sequences are *typical* among i.i.d. sequences; these are the ones that appear with high probability. 

Consider the log-likelihood of a sequence $\mathbf{y}=(y_{1},\dots,y_{N})$ where $\mathsf{y}_{i}\sim p$, this is
$$
\log p_{\boldsymbol{\mathsf{y}}}(\mathbf{y})=\sum_{n=1}^{N}\log p(y_{n}).
$$
Therefore, by the weak law of large numbers, we expect
$$
L_{p}(\mathbf{y})=\frac{1}{N}\log p_{\boldsymbol{\mathsf{y}}}(\mathbf{y})\xrightarrow[n\to \infty]{\mathbb{P}}\mathbb{E}_{p}\left[ \log p(\mathsf{y}) \right] = - H(p).
$$
In particular, the distribution of average log-likelihoods concentrates around $-H(\mathsf{y})$.

> [!definition] (Typical set)
> Let $\mathbf{y}=(y_{1},\dots,y_{N})$ be a sequence of $N$ elements. It is called ==$\varepsilon$-typical== with respect to the probability distribution $p$ if
> $$
> \left| L_{p}(\mathbf{y})+H(p) \right|\leq \varepsilon.
> $$
> We denote the set of all such sequences as $\mathcal{T}_{\varepsilon}(p;N)$, called the ==$\varepsilon$-typical set== with respect to $p$.

^d0b488

The typical set has interesting properties: 

1. $\mathbb{P}(\boldsymbol{\mathsf{y}}\in \mathcal{T}_{\varepsilon}(p;N))\approx 1$ for large $N$. This is from the WLLN.
2. $p_{\boldsymbol{\mathsf{y}}}(\mathbf{y})\approx 2^{-NH(p)}$ for typical $\mathbf{y}$, due to the likelihood.

Together, these imply that
$$
\left| \mathcal{T}_{\varepsilon}(p;N) \right| \approx 2^{NH(p)}.
$$
In other words, there are around $2^{NH(p)}$ typical sequences, and they account for most of the sequences. They are also all about equiprobable.

> [!notation]
> $$
> p^{N}(\mathbf{y})=\prod_{n=1}^{N}p(y_{n}),\quad P\{ \mathcal{A} \}=\sum_{\mathbf{y} \in \mathcal{A}}^{}p^{N}(\mathbf{y}).
> $$

The first is a distribution over length-$N$ sequences, and the second is the corresponding measure.

> [!theorem] (Asymptotic Equipartition Property)
> Let $\mathcal{T}_{\varepsilon}(p;N)$ be the $\varepsilon$-typical set with respect to a distribution $p$. Then,
> 
> 1. $\lim_{ N \to \infty }P\{ \mathcal{T}_{\varepsilon}(p;N) \}=1$.
> 2. $2^{-N(H(p)+\varepsilon)}\leq p^{N}(\mathbf{y})\leq 2^{-N(H(p)-\varepsilon)}$ for all $\mathbf{y} \in \mathcal{T}_{\varepsilon}(p;N)$.
> 3. $(1-\varepsilon)2^{N(H(p)-\varepsilon)}\leq |\mathcal{T}_{\varepsilon}(p;N)|\leq 2^{N(H(p)+\varepsilon)}$ for sufficiently large $N$.

> [!proof] Obvious

Our work extends to continuous distributions by replacing the entropy with the [[Continuous Information Theory#^94ad12|differential entropy]] and the cardinality of $\mathcal{T}$ with the volume.

## Alternative Characterization of Typical Sets

Now, suppose we have $\mathbf{y}=(y_{1},\dots,y_{N})$ generated from $p$, but also a reference distribution $q$. Then, we know by WLLN that
$$
L_{p|q}(\mathbf{y})=\frac{1}{N}\log \frac{p^{N}(\mathbf{y})}{q^{N}(\mathbf{y})}\xrightarrow[N\to \infty]{\mathbb{P}} D(p\parallel q).
$$
This leads to another characterization of typical sets.

> [!definition] (Divergence typical sets)
> Let $\mathbf{y}=(y_{1},\dots,y_{N})$ be a sequence of $N$ elements. It is called ==divergence $\varepsilon$-typical== with respect to the probability distribution $p$, relative to a reference distribution $q$, if
> $$
> \| L_{p|q}(\mathbf{y})-D(p\parallel q) \|\leq \varepsilon.
> $$
> The set of all divergence $\varepsilon$-typical sequences of length $N$ is denoted $\mathcal{T}_{\varepsilon}(p|q;N)$, and is called the ==divergence $\varepsilon$-typical set== with respect to $p$ relative to $q$.

These two notions are not equivalent, since
$$
L_{p|q}(\mathbf{y})-D(p\parallel q)=\left( L_{p}(\mathbf{y})+H(p) \right)-\left( \frac{1}{N}\sum_{n=1}^{N}\log q(y_{n})-\mathbb{E}_{p}\left[ \log q(\mathsf{y}) \right]  \right).
$$
However, they both occur with high probability.

> [!idea]
> The value of this is that it tells us how likely sampling from a *different* distribution $q$ is to produce a typical sequence for $p$.

In particular, typical sequences for $p$ satisfy $L_{p|q}(\mathbf{y})\approx D(p\parallel q)$, so we have that
$$
q^{N}(\mathbf{y})\approx p^{N}(\mathbf{y})2^{-ND(p\parallel q)}.
$$
Therefore, the probability of drawing a typical $p$ sequence from $q$ is
$$
Q\{ \mathcal{T}_{\varepsilon}(p|q;N) \}\approx 2^{-ND(p\parallel q)}.
$$
This is **exponentially small** in $N$ and the divergence between $p$ and $q$! 

Among all $2^{N\log \mathcal{Y}}$ sequences, the typical set of any nonuniform distribution includes only a vanishing subset of the collection, and the typical sets of any two different nonuniform distributions are essentially disjoint.

> [!theorem]
> If $\mathcal{T}_{\varepsilon}(p|q;N)$ is the divergence $\varepsilon$-typical set with respect to $p$ relative to $q$, then
> $$
> (1-\varepsilon)2^{-N(D(p\parallel q)+\varepsilon)}\leq Q\{ \mathcal{T}_{\varepsilon}(p|q;N) \}\leq 2^{-N(D(p\parallel q)-\varepsilon)}
> $$
> for sufficiently large $N$.

---

**Next:** [[Large Deviations and Cramér's Theorem]]