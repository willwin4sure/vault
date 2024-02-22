Let $(E,\mathcal{E},\mu)$ be a measure space and $\theta$ be a measure-preserving transformation on it. Given a measurable function $f$, we define $S_{0}=0$ and for $n\geq 1$,
$$
S_{n}=S_{n}(f)=f+f\circ\theta+\cdots+f\circ\theta^{n-1}.
$$
This is a function that takes in some set $A$, spins it around using $\theta$ a bunch of times, then passes them through $f$ and sums them up.

This should average out all the "orbits" of $\theta$, resulting in something invariant with respect to $\theta$. In this section and the next, we show that this is true both a.e. and in $L^{p}$, under certain conditions.
## Maximal Ergodic Lemma

First, we will need a helpful lemma.

> [!claim] Claim (Maximal Ergodic Lemma)
> Let $f$ be an integrable function on $E$. Set $S^{*}=\sup_{n\geq 0}S_{n}(f)$. Then,
> $$
> \int_{\{S^{*}>0\}} f \, d\mu \geq 0. 
> $$

> [!proof]-
> Set $S_{n}^{*}=\max_{0\leq m\leq n}S_{m}$ and $A_{n}=\{ S_{n}^{*} > 0 \}$. Note that $S_{n}^{*}\geq 0$ since $S_{0}=0$. Now, we can check that for all $m>0$, we have that
> $$
> S_{m}=f+S_{m-1}\circ\theta \leq f+S_{n}^{*}\circ\theta.
> $$
> On the set $A_{n}$, we have that $S_{n}^{*}=\max_{1\leq m\leq n}S_{m}$, which allows us to write $S_{n}^{*}\leq f+S_{n}^{*}\circ\theta$. Meanwhile, on the set $A_{n}^{c}$, $S_{n}^{*}=0\leq S_{n}^{*}\circ\theta$. Therefore, summing gives
> $$
> \int_{E} S_{n}^{*} \, d\mu \leq \int_{A_{n}} f \, d\mu + \int_{E}(S_{n}^{*}\circ\theta) \, d\mu. 
> $$
> Yet $S_{n}^*$ is integrable and $\theta$ is measure-preserving, so
> $$
> \int_{E}(S_{n}^{*}\circ\theta) \, d\mu = \int_{E}S_{n}^{*} \, d\mu. 
> $$
> This forces $\int_{A_{n}}f \, d\mu \geq 0$. As $n\to \infty$, $A_{n}\uparrow \{ S^{*}>0 \}$ so, by dominated convergence with dominating function $|f|$,
> $$
> \int_{E} \mathbf{1}_{\{S^*>0\}}f \, d\mu = \lim_{ n \to \infty } \int_{E}\mathbf{1}_{A_{n}} f \, d\mu \geq 0, 
> $$
> as desired.

> [!theorem] Theorem (Birkhoff's Almost Everywhere Ergodic Theorem)
> Assume that $(E,\mathcal{E},\mu)$ is $\sigma$-finite and that $f$ is an integrable function on $E$. Then, there exists an invariant function $\overline{f}$ with $\mu(|\overline{f}|)\leq \mu(|f|)$, such that $S_{n}(f)\to \overline{f}$ a.e. as $n\to \infty$.

---

**Next:** [[Von Neumann's Lp Ergodic Theorem]]