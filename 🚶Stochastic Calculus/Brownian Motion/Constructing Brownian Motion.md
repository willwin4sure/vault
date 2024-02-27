[[Gaussian Spaces and Processes|Last]], we defined what it means for a Gaussian process on $I=[0,\infty)$ to have covariance function $\Gamma(s,t)$. We also proposed a definition for Brownian motion to be such a Gaussian process where $\Gamma(s,t)=\min\{ s, t \}$.

## Kolmogorov Extension Theorem

The Kolmogorov extension theorem tells us that on the probability space $(\mathbb{R}^{I},\mathcal{B}_{\mathbb{R}}^{\otimes I})$, there exists a unique probability measure $\nu$ with finite dimensional marginals $\nu_{J}=\mathcal{N}(0,\Gamma_{J\times J})$ for all finite $J\subseteq I$. Now, for any $\omega \in \mathbb{R}^{I}$, we let $X_{t}(\omega)=\omega_{t}$ for $t\in I$. Then, $(X_{t})_{t\in I}$ is a Gaussian process with covariance function $\Gamma$.

However, there is a **pretty major issue**: for any $\omega$, the map $t\mapsto X_{t}(\omega)$ may not be continuous, or even measurable! This is because the product $\sigma$-algebra $(\mathbb{R}^{I},\mathcal{B}_{\mathbb{R}}^{I})$ is not rich enough!

Here are our desired properties:

> [!definition] (Standard Brownian motion)
> A ==standard Brownian motion== is a stochastic process $(B_{t})_{t\geq 0}$ such that $B_{0}=0$, $B_{t}-B_{s}\sim \mathcal{N}(0,t-s)$, we have independent increments, and there are continuous sample paths $t\mapsto B_{t}(\omega)$ for all $\omega \in\Omega$.

The Kolmogorov extension theorem gives us some process $(X_{t})_{t\geq 0}$ that has all the requirements (via the covariance function) except the last one. We will modify the Gaussian process to guarantee continuity while preserving the other three properties. To do this, we need the following lemma, which you can read about below.

1. [[Kolmogorov Continuity Lemma]]

## Application to Brownian Motion

We know that $|X_{s}-X_{t}|\sim \mathcal{N}(0,t-s)$, which means that
$$
\mathbb{E}[|X_{s}-X_{t}|^{q}]\sim(t-s)^{q/2}.
$$
Then, $\varepsilon=\frac{q}{2}-1$, so the Kolmogorov continuity lemma gives us that Brownian motion is $\alpha$-Hölder continuous for all $\alpha< \frac{1}{2}$. This is essentially optimal: even $\alpha=\frac{1}{2}$ does not work!

So far, we've worked on $[0,1]$, which can be scaled to $[0,M]$ for any $M$. How would we extend to $\mathbb{R}$?

> [!theorem]
> Let $(X_{t})_{t\geq 0}$ be a Gaussian process with covariance function $\Gamma(s,t)=\min\{ s,t \}$. Then $X$ has a modification $B$ that is locally $\alpha$-Holder continuous, i.e. $|B_{t}(\omega)-B_{s}(\omega)|\leq K_{\alpha,M}(\omega)|s-t|^{\alpha}$ for all $s,t\in[0,M]$.

This is because we cannot get a uniform moment bound on the entire real line, so our constant $K$ must depend on the size $M$.

---

**Next:** [[The Wiener Measure]]