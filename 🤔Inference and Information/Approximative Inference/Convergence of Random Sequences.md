Uh, yea you know most of these from [[Convergence of Measurable Functions|probability class]]. But there are some extra things.

> [!definition] (Convergence in divergence)
> A sequence of random variables $\mathsf{z}_{1},\mathsf{z_{2}},\dots$ is said to ==converge in divergence== (or ==strongly in law==) to a random variable $\mathsf{z}$ if
> $$
> \lim_{ N \to \infty } D(p_{\mathsf{z}_{N}}\parallel p_{\mathsf{z}}) = 0.
> $$
> We express this convergence using the notation $\mathsf{z}_{N}\xrightarrow[]{D}\mathsf{z}$.

This is stronger than convergence in distribution due to a bound on the difference in cumulative distribution functions.

## Transformations

Some extra theorems you learned in intro statistics:

> [!theorem] (Continuous mapping theorem)
> If $\mathsf{z}_{1},\mathsf{z}_{2},\dots$ are defined over $\mathbb{R}^{k}$ and if $g:\mathbb{R}^{k}\to \mathbb{R}$ is a continuous function, then $\mathsf{z}_{n}\xrightarrow[]{}\mathsf{z}$ implies $g(\mathsf{z}_{n})\xrightarrow[]{}g(\mathsf{z})$ for convergence in distribution, probability, and almost sure.

> [!theorem] (Slutsky's theorem)
> If $\mathsf{x}_{n}\xrightarrow[]{d}\mathsf{x}$ and $\mathsf{y}_{n}\xrightarrow[]{d}c$, where $c$ is a finite constant, then
> $$
> \mathsf{x}_{n}+\mathsf{y}_{n}\xrightarrow[]{d}\mathsf{x}+c,\quad \mathsf{x}_{n}\mathsf{y}_{n}\xrightarrow[]{d} c\mathsf{x}.
> $$
> Moreover, if $\mathsf{z}_{n}$ is any sequence such that $\mathsf{x}_{n}=\mathsf{z}_{n}\mathsf{y}_{n}$ and $c\neq 0$, then
> $$
> \mathsf{z}_{n}\xrightarrow[]{d} \mathsf{x}/c.
> $$

Note that convergence in distribution to a constant is equivalent to convergence in probability to that same constant.

## Uniform Law of Large Numbers

You know the [[Strong Law of Large Numbers|strong law]] and the weak law. Here is a more general version.

> [!theorem] (Strong ULLN)
> Let $\mathsf{w}_{1},\dots,\mathsf{w}_{N}\in \mathcal{W}$ be a sequence of i.i.d. random variables, and let $\Theta$ be a compact parameter set. Let $g:\mathcal{W}\times \Theta\to \mathbb{R}$ be a function such that $g(w;\theta)$ is continuous in $\theta$ for all $w$ and is uniformly dominated by some integrable $g_{+}(w)$. Then,
> $$
> \max_{\theta \in\Theta}\left| \frac{1}{N}\sum_{n=1}^{N}g(\mathsf{w}_{n};\theta)-\mu(\theta) \right| \xrightarrow[N\to \infty]{\text{a.s.}}0.
> $$
> where $\mu(\theta)=\mathbb{E}\left[ g(\mathsf{w};\theta) \right]$ is the mean of the sample average.

You also know the [[Central Limit Theorem|central limit theorem]]. The convergence in distribution for CLT is also uniform by a theorem due to Pólya. The rate of the convergence can be bounded by the Berry-Esseen theorem.

> [!theorem] (Strong CLT)
> Let $\mathsf{w}_{1},\dots,\mathsf{w}_{N}$ be a set of i.i.d. random variables with mean $\mu$ and variance $\sigma^{2}<\infty$. Then,
> $$
> \mathsf{e}_{N}:=\sqrt{ N }\left( \frac{1}{N}\sum_{n=1}^{N}\mathsf{w}_{n}-\mu \right) \xrightarrow[N\to \infty]{D} \mathtt{N}(0,\sigma^{2})
> $$
> if and only if $D(p_{\mathsf{e}_{N}}\parallel \mathtt{N}(0,\sigma^{2}))<\infty$ for some $N$.

In particular, this establishes convergence in divergence of the distribution.

---

**Next:** [[Asymptotics of Non-Bayesian Parameter Estimation]]