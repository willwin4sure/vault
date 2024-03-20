The evil twin of [[Single-Parameter Exponential Families|exponential families]]. These are hyperplanes in distribution space.

> [!definition] (Linear family)
> A set of distributions $\mathcal{P}\subseteq \mathcal{P}^{\mathcal{Y}}$ is a ==linear family== if for some $K<M$ there exist functions $\mathbf{t}=\begin{bmatrix}t_{1}(\bullet) & \cdots & t_{K}(\bullet)\end{bmatrix}^{T}$ and constants $\overline{\mathbf{t}}=\begin{bmatrix}\overline{t}_{1} & \cdots & \overline{t}_{K}\end{bmatrix}^{T}$ such that
> $$
> \mathbb{E}_{p}\left[ t_{i}(\mathsf{y}) \right] = \overline{t}_{i}
> $$
> for all $i$ and $p \in \mathcal{P}$. We can express this in matrix form via
> $$
> \underbrace{\begin{bmatrix}
> t_{1}(1)-\overline{t}_{1} & \cdots & t_{1}(M)-\overline{t}_{1} \\
> \vdots & \ddots & \vdots \\
> t_{K}(1)-\overline{t}_{K} & \cdots & t_{K}(M)-\overline{t}_{K}
> \end{bmatrix}}_{\tilde{\mathbf{T}}}
> \begin{bmatrix}
> p(1) \\
> \vdots \\
> p(M)
> \end{bmatrix}
> =\mathbf{0}.
> $$

Therefore, $\mathcal{P}$ is just the intersection of the null space of the matrix $\tilde{\mathbf{T}}$ with the simplex $\mathcal{P}^{\mathcal{Y}}$. By convention, we will assume that this is always nonempty.

> [!claim] (Linear families form convex sets)
> For any $p_{1},p_{2}\in \mathcal{L}$ and $\lambda \in \mathbb{R}$, we have that $\lambda p_{1}+(1-\lambda)p_{2}\in \mathcal{L}$ as well.

Further, the [[Information Projection and Pythagoras' Theorem#^e9babb|Pythagorean theorem]] we saw last time achieves equality for linear families.

> [!claim] (Pythagorean Identity)
> Let $\mathcal{L}$ be a linear family of distributions defined on the alphabet $\mathcal{Y}$. Let $q$ be an arbitrary distribution in the interior of the associated simplex $\mathcal{P}^{\mathcal{Y}}$. Then, the I-projection $p^{*}$ of $q$ onto $\mathcal{L}$ satisfies
> $$
> D(p\parallel q)=D(p\parallel p^{*})+D(p^{*}\parallel q)
> $$
> for all $p \in \mathcal{L}$.

This is a statement about when an I-projection is orthogonal in an information theoretic sense (recall that information divergence is analogous to squared Euclidean distance).

---

**Next:** [[Orthogonal Families]]