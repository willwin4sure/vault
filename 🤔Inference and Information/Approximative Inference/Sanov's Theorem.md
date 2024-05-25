Events involving i.i.d. sequences can be rephrased as events involving their corresponding types. In particular, we are concerned with events of the form
$$
\mathcal{R}=\left\{ \mathbf{y}\in \mathcal{Y}^{N} : \hat{p}(\bullet;\mathbf{y})\in \mathcal{S}\cap \mathcal{P}_{N}^{\mathcal{Y}} \right\}.
$$
> [!notation]
> All the following have the same meaning:
> $$
> \mathbb{P}_{q}\left( \hat{p}(\bullet;\boldsymbol{\mathsf{y}})\in \mathcal{S} \right) = Q\{ \mathcal{S} \} = Q \left\{ \mathcal{S}\cap \mathcal{P}_{N}^{\mathcal{Y}} \right\} = Q\{ \mathcal{R} \} = \mathbb{P}_{q}\left( \boldsymbol{\mathsf{y}}\in \mathcal{R} \right).
> $$

## Statement

Suppose we have samples from a distribution $q$. Then, the point is that the probability of every type class $\mathcal{T}_{N}^{\mathcal{Y}}(p)$ [[Method of Types#^0abe2c|decays exponentially]]. Yet there are only [[Method of Types#^aae286|polynomially many]] type classes, so the exponential corresponding to the distribution closest to $q$ dominates the sum.

[[Large Deviations and Cramér's Theorem#^ac5cdc|Once again]], miracles happen in the least miraculous ways, if at all.

> [!theorem] (Sanov's theorem)
> Let $\mathcal{S} \subseteq \mathcal{P}^{\mathcal{Y}}$ be an arbitrary set, and let $q\in \mathcal{P}^{\mathcal{Y}}$ be arbitrary. Then,
> $$
> Q\left\{ \mathcal{S}\cap \mathcal{P}_{N}^{\mathcal{Y}} \right\} \leq (N+1)^{|\mathcal{Y}|}2^{-ND(p_{*}\parallel q)},
> $$
> where
> $$
> p_{*}=\arg\min_{p \in \text{cl}(\mathcal{S})}D(p\parallel q)
> $$
> is the [[Information Projection and Pythagoras' Theorem#^936604|I-projection]] of $q$ onto the closure of $\mathcal{S}$. Moreover, if $\mathcal{S}$ has a closure equal to the closure of its interior, then
> $$
> Q\left\{ \mathcal{S}\cap \mathcal{P}_{N}^{\mathcal{Y}} \right\} \doteq 2^{-ND(p_{*}\parallel q)}.
> $$

## Dominant Atypical Behavior

Sanov's theorem suggests that when the event happens, the type will be $p_{*}$. It turns out that this is true:

> [!theorem] (Conditional limit theorem)
> Let $\mathcal{S}\subseteq \mathcal{P}^{\mathcal{Y}}$ be a (nonempty) closed and convex set, and let $\boldsymbol{\mathsf{y}}\in \mathcal{Y}^{N}$. If the elements of $\boldsymbol{\mathsf{y}}$ are i.i.d. according to the distribution $q \in \mathcal{P}^{\mathcal{Y}}$ where $q\not\in \mathcal{S}$, then for any $\varepsilon>0$,
> $$
> \lim_{ N \to \infty } \mathbb{P}\left( \left| \hat{p}(b;\boldsymbol{\mathsf{y}})-p_{*}(b) \right| > \varepsilon \big| \hat{p}(\bullet;\boldsymbol{\mathsf{y}})\in \mathcal{S} \right) = 0
> $$
> for all $b \in \mathcal{Y}$, where $p_{*}$ is the I-projection defined above. In other words, given that the lies in the atypical set $\mathcal{S}$, the type of the distribution satisfies $\hat{p}(b;\boldsymbol{\mathsf{y}})\xrightarrow[N\to \infty]{\mathbb{P}}p_{*}(b)$. 

---

**Next:** [[Asymptotics of Hypothesis Testing]]






