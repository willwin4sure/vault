In this section, we introduce the notion of an information projection, which tells us which distribution in $\mathcal{P}$ we are least surprised to observe given that our model of the world is $q$.

> [!definition] (I-projection)
> The ==I-projection== of a distribution $q$ onto a nonempty and closed set of distributions $\mathcal{P}$ is the distribution $p^{*}$ such that
> $$
> p^{*}=\mathop{\arg\min}_{p \in \mathcal{P}}\ D(p\parallel q).
> $$

^936604

This optimum exists since $D(p\parallel q)$ is nonnegative and continuous in $p$, and $\mathcal{P}$ is compact. The projection is not necessarily unique, but when $\mathcal{P}$ is a convex set, it is.

> [!remark]
> Suppose you are doing inference, and you observe $q$ and want to find the best model inside $\mathcal{P}$. The morally correct way to do this is with the ==M-projection== $p^{*}=\mathop{\arg\min}_{p \in \mathcal{P}}\ D(q\parallel p)$, notably different from the I-projection.

^fafb8a

I don't think Pythagoras came up with the next one. 

> [!theorem] (Pythagoras' Theorem, Information Version)
> Let $q$ be any distribution and suppose $p^{*}$ is its I-projection onto a nonempty, closed, convex set $\mathcal{P}$. Then,
> $$
> D(p\parallel q)\geq D(p\parallel p^{*})+D(p^{*}\parallel q)
> $$
> for all $p \in \mathcal{P}$.

^e9babb

> [!proof]- Sketch
> Look at the distributions $p_{\lambda}=(1-\lambda)p^{*}+\lambda p$ that interpolate between the projection $p^{*}$ and your arbitrary $p \in \mathcal{P}$. Then, note that
> $$
> \frac{d}{d\lambda}D(p_{\lambda}\parallel q)
> $$
> is positive when evaluated at $\lambda=0$, otherwise $p^{*}$ would not be the projection. Expanding this fact using the definition of KL divergence finishes.

> [!claim] (Pythagoras' Corollary)
> The I-projection $p^{*}$ of any $q$ not on the boundary of $\mathcal{P}^{\mathcal{Y}}$ onto a family $\mathcal{P}$ cannot lie on a boundary component of $\mathcal{P}^{\mathcal{Y}}$ unless all of $\mathcal{P}$ is confined to that particular boundary component.

> [!proof]- Immediate
> Otherwise $D(p \parallel q)$ will be finite yet $D(p \parallel p^{*})$ will be infinite, if we pick $p \in \mathcal{P}$ outside of that boundary component.

---

**Next:** [[Linear Families]]