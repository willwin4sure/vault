
> [!definition] ($\alpha$-Hölder continuity)
> For a function $f:\mathbb{R}\to \mathbb{R}$, we say that it is ==$\alpha$-Hölder continuous== if there exists some global constant $C$ such that $|f(x)-f(y)|\leq C|x-y|^{\alpha}$ for all $x,y\in \mathbb{R}$. 

> [!remark]
> The case of $\alpha=1$ is the [[Universal Approximation#^bb2604|Lipschitz condition]]. If $\alpha>1$, then the only such functions are constant.

## Statement

> [!theorem] (Kolmogorov Continuity Lemma)
> Let $(X_{t})$ be a stochastic process indexed by a bounded interval $I$ of $\mathbb{R}$, taking values in a complete metric space $(E,d)$. Assume that there exist three reals $q,\varepsilon,C>0$ such that, for every $s,t\in I$, 
> $$
> \mathbb{E}[d(X_{s},X_{t})^{q}]\leq C(t-s)^{1+\varepsilon}.
> $$
> Then, there is a modification $\tilde{X}$ of $X$ whose sample paths are $\alpha$-Hölder continuous for every $\alpha \in(0,\varepsilon/q)$. In particular, this means for every $\omega \in \Omega$ and every such $\alpha$, there exists a finite constant $C_{\alpha}(\omega)$ such that, for every $s,t\in I$, 
> $$
> d(\tilde{X}_{s}(\omega),\tilde{X}_{t}(\omega))\leq C_{\alpha}(\omega)|t-s|^{\alpha}.
> $$
> In particular, $\tilde{X}$ is a modification of $X$ with continuous sample paths. Further, such a modification is unique up to indistinguishability.
## Proof

Consider the dyadic rational values $D_{n}=\left\{  \frac{i}{2^{n}} : 0\leq i<2^{n}  \right\}$, and write $D=\bigcup_{n\geq 0}^{}D_{n}$.

> [!claim] (Deterministic local-to-global Hölder bound on dyadics)
> Suppose $f:D\to \mathbb{R}$ satisfies
> $$
> \left| f\left( \frac{i}{2^{n}} \right) - f\left( \frac{i-1}{2^{n}} \right) \right|\leq K\left( \frac{1}{2^{n}} \right)^{\alpha} 
> $$
> for all $i,n$ and some global constant $K$. Then, $|f(s)-f(t)|\leq K'|s-t|^{\alpha}$ for all $s,t\in D$. We might get a worse global constant $K'$ compared to $K$.

> [!proof]- Binary lifting
> Suppose we are going from $s$ to $t$ which are far away from each other. We don't want to add up bounds from each individual increment; instead, we group together the jumps that we can do over lower dyadics.
> 
> Suppose $\frac{1}{2^{p}}$ is the largest block that we can fit inside of the interval from $s$ to $t$. This allows us to find some $s_{0}\in[s,t]$ that is in $D_{p}$. Now do a dyadic expansion in both directions. The computation amounts to the fact that
> $$
> K\cdot\left( \frac{1}{2^{p}} \right)^{\alpha}+2\cdot\left[ K\left( \frac{1}{2^{p+1}} \right) ^{\alpha} + \cdots \right] \leq K'\cdot \frac{1}{2^{p\alpha}}
> $$
> for $K'= \frac{CK}{1-2^{-\alpha}}$ for some number $C$.
> 

> [!claim] (Stochastic Hölder bound under mild moment bound)
> Suppose $(X_{t})_{0\leq t\leq 1}$ is some stochastic process such that $\mathbb{E}\left[ |X_{s}-X_{t}|^{q} \right]\leq C|s-t|^{1+\varepsilon}$ for all $s,t\in[0,1]$. Then, for all $\alpha \in(0, \varepsilon / q)$, there exists a random variable $K_{\alpha}(\omega)<\infty$ such that
> $$
> \left| X_{\frac{i}{2^{n}}}-X_{\frac{i-1}{2^{n}}} \right| \leq \frac{K_{\alpha}(\omega)}{2^{n\alpha}}
> $$
> with probability $1$, for all $n,i$. 

> [!proof]- Union + Markov bounds, then Borel-Cantelli
> Let $A_{n}=\left\{  \omega : \left| \left( X_{\frac{i}{2^{n}}} - X_{\frac{i-1}{2^{n}}} \right)(\omega) \right|\geq \frac{1}{2^{n\alpha}}\text{ for some }i  \right\}$. Then, by a Union bound and a Markov bound,
> $$
> \mathbb{P}(A_{n})\leq 2^{n}\cdot \frac{\mathbb{E}\left[ \left|X_{\frac{i}{2^{n}}} - X_{\frac{i-1}{2^{n}}}\right|^{q} \right] }{(1/2^{n\alpha})^{q}}
> \leq \frac{2^{n}\cdot C\cdot 2^{-n(1+\varepsilon)}}{2^{-n\alpha q}}=C\cdot 2^{-n(\varepsilon-\alpha q)}.
> $$
> Under the assumption that $\varepsilon>\alpha q$, we know that the sum of these over $n\geq 0$ converges, meaning by the first Borel-Cantelli lemma, $\mathbb{P}(A_{n}\text{ i.o.})=0$. Restrict to the case where $\omega$ falls outside of the measure-zero event where $\{ A_{n}\text{ i.o.} \}$. Therefore, there exists some $N(\omega)$ such that $\omega \not\in A_{n}$ for all $n > N(\omega)$. 
> 
> Therefore, we can compute
> $$
> \sup_{n\geq 1}\left\{ \max_{1\leq i< 2^{n}}\left| X_{\frac{i}{2^{n}}}(\omega)-X_{\frac{i-1}{2^{n}}}(\omega) \right| \cdot 2^{n\alpha} \right\}\leq
> \max\left\{ 1,\max_{n\leq N(\omega)}\left\{ \max_{1\leq i< 2^{n}}\left| X_{\frac{i}{2^{n}}}(\omega)-X_{\frac{i-1}{2^{n}}}(\omega) \right| \cdot 2^{n\alpha} \right\}  \right\},
> $$
> which is finite and can be set to $K_{\alpha}(\omega)$, as desired.

> [!definition] (Modification of stochastic process)
> We say that $\tilde{X}$ is a ==modification== of $X$ if $\mathbb{P}(X_{t}=\tilde{X}_{t})=1$ for any fixed time $t$, though $\mathbb{P}(X_{t}=\tilde{X}_{t}\text{ for all }t)$ might not be $1$ (such a condition is called ==indistinguishability==).

A modification will always have identical finite-dimensional marginals. This preserves the first three properties of Brownian motion that we want.

> [!claim] ($\alpha$-Hölder continuous modification of stochastic processes)
> Under the assumptions of the last claim, there exists a modification $\tilde{X}$ of $X$ whose sample paths are $\alpha$-Hölder continuous, i.e. for a.e. $\omega$,
> $$
> \left| \tilde{X}_{t}(\omega)-\tilde{X}_{s}(\omega) \right| \leq K_{\alpha}'(\omega)|s-t|^{\alpha}
> $$
> for all $s,t\in[0,1]$.
>

> [!proof]- Complete from dyadics
> Define $\tilde{X}_{t}(\omega)=\lim_{ s \to t,\ s \in D }X_{s}(\omega)$. Note that this limit only looks at $X$ on dyadics $D$. $\tilde{X}$ is $\alpha$-Hölder continuous almost surely because of the last two claims: if we look at the difference $|\tilde{X}_{s}-\tilde{X}_{t}|$, we approximate using $s',t'\in D$. In the case that $\omega$ lies in the measure-zero event that this doesn't hold, we can set $\tilde{X}_{t}(\omega)=0$ instead.
> 
> We do need to show that $\tilde{X}$ is actually a modification of $X$. For any given $t$, the assumption $\mathbb{E}\left[ |X_{s}-X_{t}|^{q} \right]\leq C|s-t|^{1+\varepsilon}$ implies that as $D\ni s\to t$, we have that $X_{s}\to X_{t}$ in $L^{q}$ but also that $X_{s}\to \tilde{X}_{t}$ a.s. by definition, so $X_{t}=\tilde{X}_{t}$ almost surely.

This applies to any stochastic process $(X_{t})_{t\geq 0}$, as long as the moment estimate holds.

---

**Next:** [[Constructing Brownian Motion]]