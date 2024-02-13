In this section, we analyze the structure of the $L^{p}$-norm on $L^{p}$. It turns out that as long as we mod out by measure zero differences in functions (to form $\mathcal{L}^{p}$), we get a Banach space.
## Banach Spaces

First, we present the general definition of a normed vector space.

> [!definition] Definition (Normed vector space)
> Let $V$ be a vector space. A map $v\mapsto \| v \|:V\to[0,\infty)$ is a ==norm== if
> 
> 1. $\| u+v \|\leq \| u \|+\| v \|$ for all $u,v\in V$,
> 2. $\| \alpha v \|=|\alpha|\| v \|$ for all $v\in V$ and $\alpha \in \mathbb{R}$,
> 3. $\| v \|=0$ implies $v=0$.
> 
> For any norm, if $\| v_{n}-v \| \to 0$, then $\| v_{n} \| \to \| v \|$.

The notion of completeness of metric spaces extends here.

> [!definition] Definition (Banach spaces)
> A normed vector space $V$ is called ==complete== if every Cauchy sequence converges. In other words, for any sequence $(v_{n}:n\in \mathbb{N})$ in $V$ such that $\| v_{n}-v_{m} \| \to 0$ as $n,m\to \infty$, there exists some $v\in V$ such that $\| v_{n}-v \| \to 0$ as $n\to \infty$.
> 
> A complete normed vector space is called a ==Banach space==.

^627497

## $L^{p}$ is (almost) a Banach Space

Let's try to check 

1. Follows from [[Minkowski's Inequality#^38e149|Minkowski's inequality]].
2. Follows from linearity of integration.
3. Is not true! $\| f \|_{p}=0$ does not imply $f=0$, [[Lebesgue Integrals Behave#^38742c|only that $f=0$ a.e.]]

However, this is not hard to patch. 

> [!definition]
> Define $\sim$ as the equivalence relation between functions that agree almost everywhere. Let $[f]$ denote the equivalence class of $f$, and write
> $$
> \mathcal{L}^{p}=\{ [f] : f\in L^{p} \},
> $$
> which is just $L^{p}$ modulo $\sim$.

Now, $\mathcal{L}^{p}$ is a valid normed vector space. Now, we will show that it is a Banach space.

> [!theorem] Theorem (Completeness of $L^{p}$)
> Let $p \in[1,\infty]$ and $(f_{n}:n\in \mathbb{N})$ be a sequence in $L^{p}$ such that
> $$
> \| f_{n}-f_{m} \|_{p}\to 0
> $$
> as $n,m\to \infty$. Then, there exists $f\in L^{p}$ such that
> $$
> \| f_{n}-f \|_{p}\to 0
> $$
> as $n\to \infty$.

> [!proof]-
> The case of $p=\infty$ is left as an exercise; take $p<\infty$ for now. Pick some subsequence $(n_{k})$ such that
> $$
> S=\sum_{k=1}^{\infty}\| f_{n_{k+1}}-f_{n_{k}} \|_{p} < \infty.
> $$
> By [[Minkowski's Inequality#^38e149|Minkowski's inequality]], we know that
> $$
> \left\| \sum_{k=1}^{K}|f_{n_{k+1}}-f_{n_{k}}| \right\|_{p} \leq S.
> $$
> By [[Monotone Convergence Theorem|monotone convergence]], this bound holds also for $K=\infty$, so
> $$
> \sum_{k=1}^{\infty}|f_{n_{k+1}}-f_{n_{k}}| < \infty \quad \text{a.e.}
> $$
> Hence, by completeness of $\mathbb{R}$, $f_{n_{k}}$ converges a.e. We define a measurable function $f$ by
> $$
> f(x)=\begin{cases}
> \lim_{ k \to \infty } f_{n_{k}}(x) & \text{if the limit exists}, \\
> 0 & \text{otherwise}.
> \end{cases}
> $$
> Given $\varepsilon>0$, we can find $N$ so that $n\geq N$ implies
> $$
> \mu \left( |f_{n}-f_{m}|^{p} \right) \leq \varepsilon,\quad \text{for all }m\geq n,
> $$
> in particular $\mu(|f_{n}-f_{n_{k}}|^{p})\leq\varepsilon$ for all sufficiently large $k$. Hence, by Fatou's lemma, for $n\geq N$,
> $$
> \mu(|f_{n}-f|^{p})=\mu \left( \liminf_{ k \to \infty } |f_{n}-f_{n_{k}}|^{p} \right) \leq \liminf_{ k \to \infty } \mu \left( |f_{n}-f_{n_{k}}|^{p} \right)\leq\varepsilon. 
> $$
> Hence $f\in L^{p}$ and, since $\varepsilon>0$ was arbitrary, $\| f_{n}-f \|_{p}\to 0$.

Therefore, $\mathcal{L}^{p}$ is a Banach space!

---

**Next:** [[L2 as a Hilbert Space]]