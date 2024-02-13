[[Gaussian Spaces and Processes|Last]], we defined what it means for a Gaussian process on $I=[0,\infty)$ to have covariance function $\Gamma(s,t)$. We also proposed a definition for Brownian motion to be such a Gaussian process where $\Gamma(s,t)=\min\{ s, t \}$.

The Kolmogorov extension theorem tells us that on the probability space $(\mathbb{R}^{I},\mathcal{B}_{\mathbb{R}}^{\otimes I})$, there exists a unique probability measure $\nu$ with finite dimensional marginals $\nu_{J}=\mathcal{N}(0,\Gamma_{J\times J})$ for all finite $J\subseteq I$. Now, for any $\omega \in \mathbb{R}^{I}$, we let $X_{t}(\omega)=\omega_{t}$ for $t\in I$. Then, $(X_{t})_{t\in I}$ is a Gaussian process with covariance function $\Gamma$.

However, there is a **pretty major issue**: for any $\omega$, the map $t\mapsto X_{t}(\omega)$ may not be continuous, or even measurable!

> [!definition] Definition (Standard Brownian motion)
> A standard Brownian motion is a stochastic process $(B_{t})_{t\geq 0}$ such that $B_{0}=0$, $B_{t}-B_{s}\sim \mathcal{N}(0,t-s)$, we have independent increments, and there are continuous sample paths $t\mapsto B_{t}(\omega)$ for all $\omega \in\Omega$.

The Kolmogorov extension theorem gives us some process $(X_{t})_{t\geq 0}$ that has all the requirements (via the covariance function) except the last one. We will modify the Gaussian process to guarantee continuity while preserving the other three properties.

## Kolmogorov Continuity Lemma

> [!definition] Definition ($\alpha$-Holder continuity)
> For a function $f:\mathbb{R}\to \mathbb{R}$, we say that it is ==$\alpha$-Holder continuous== if there exists some global constant $C$ such that $|f(x)-f(y)|\leq C|x-y|^{\alpha}$ for all $x,y\in \mathbb{R}$. 

> [!remark]
> The case of $\alpha=1$ is the Lipschitz condition. If $\alpha>1$, then the only such functions are constant.

Consider the dyadic rationals $D_{n}=\left\{  \frac{i}{2^{n}} : 0\leq i<2^{n}  \right\}$, and write $D=\bigcup_{n\geq 0}^{}D_{n}$.

> [!claim]
> Suppose $f:D\to \mathbb{R}$ satisfies
> $$
> \left| f\left( \frac{i}{2^{n}} \right) - f\left( \frac{i-1}{2^{n}} \right) \right|\leq K\left( \frac{1}{2^{n}} \right)^{\alpha} 
> $$
> for all $i,n$ and some global constant $K$. Then, $|f(s)-f(t)|\leq K'|s-t|^{\alpha}$ for all $s,t\in D$. We might get a worse global constant $K'$ compared to $K$.

> [!proof]-
> Binary lifting. Suppose we are going from $s$ to $t$ which are far away from each other. We don't want to add up each individual increment; instead, we group together the jumps that we can do over lower dyadics.
> 
> Suppose $\frac{1}{2^{p}}$ is the largest block that we can fit inside of the interval from $s$ to $t$. This allows us to find some $s_{0}\in[s,t]$ that is in $D_{p}$. Now do a dyadic expansion in both directions. The computation amounts to the fact that
> $$
> K\cdot\left( \frac{1}{2^{p}} \right)^{\alpha}+2\cdot\left[ K\left( \frac{1}{2^{p+1}} \right) ^{\alpha} + \cdots \right] \leq K'\cdot \frac{1}{2^{p\alpha}}
> $$
> for $K'= \frac{CK}{1-2^{-\alpha}}$ for some number $C$.
> 

> [!claim]
> Suppose $(X_{t})_{0\leq t\leq 1}$ is some stochastic process such that $\mathbb{E}\left[ |X_{s}-X_{t}|^{q} \right]\leq C|s-t|^{1+\varepsilon}$ for all $s,t\in[0,1]$. Then, for all $\alpha \in(0, \varepsilon / q)$, there exists a random variable $K_{\alpha}(\omega)<\infty$ such that
> $$
> \left| X_{\frac{i}{2^{n}}}-X_{\frac{i-1}{2^{n}}} \right| \leq \frac{K_{\alpha}(\omega)}{2^{n\alpha}}
> $$
> with probability $1$, for all $n,i$. 

[!proof]
Let $A_{n}=\left\{  \omega : \left| \left( X_{\frac{i}{2^{n}}} - X_{\frac{i-1}{2^{n}}} \right)(\omega) \right|\geq \frac{1}{2^{n\alpha}}\text{ for some }i  \right\}$. Then, by a Union bound and a Markov bound,
$$
\mathbb{P}(A_{n})\leq 2^{n}\cdot \frac{\mathbb{E}\left[ \left|X_{\frac{i}{2^{n}}} - X_{\frac{i-1}{2^{n}}}\right|^{q} \right] }{(1/2^{n\alpha})^{q}}
\leq \frac{2^{n}\cdot C\cdot 2^{-n(1+\varepsilon)}}{2^{-n\alpha q}}=C\cdot 2^{-n(\varepsilon-\alpha q)}.
$$
Under the assumption that $\varepsilon>\alpha q$, we know that the sum of these over $n\geq 0$ converges, meaning by the first Borel-Cantelli lemma, $\mathbb{P}(A_{n}\text{ i.o.})=0$. 

> [!definition] Definition (Modification)
> We say that $\tilde{X}$ is a ==modification== of $X$ if $\mathbb{P}(X_{t}=\tilde{X}_{t})=1$ for any fixed time $t$, though $\mathbb{P}(X_{t}=\tilde{X}_{t}\text{ for all }t)$ might not be $1$.

A modification will always have identical finite-dimensional marginals. This preserves the first three properties of Brownian motion that we want.

> [!claim]
> Under the assumptions of the last claim, there exists a modification $\tilde{X}$ of $X$ whose sample paths are $\alpha$-Holder continuous, i.e. for a.e. $\omega$,
> $$
> \left| \tilde{X}_{t}(\omega)-\tilde{X}_{s}(\omega) \right| \leq K_{\alpha}'(\omega)|s-t|^{\alpha}
> $$
> for all $s,t\in[0,1]$.
>

[!proof]-
Define $\tilde{X}_{t}(\omega)=\lim_{ s \to t,\ s \in D }X_{s}(\omega)$. Note that this only looks at $X$ on dyadics $D$. $\tilde{X}$ is $\alpha$-Holder continuous because of the last two claims: if we look at the difference $|\tilde{X}_{s}-\tilde{X}_{t}|$, we approximate using $s',t'\in D$.

We do need to show that $\tilde{X}$ is actually a modification of $X$. For any given $t$, the assumption $\mathbb{E}\left[ |X_{s}-X_{t}|^{q} \right]\leq C|s-t|^{1+\varepsilon}$ implies that as $D\ni s\to t$, we have that $X_{s}\to X_{t}$ in $L^{q}$ but also that $X_{s}\to \tilde{X}_{t}$ a.s. by definition, so $X_{t}=\tilde{X}_{t}$ almost surely.

The three claims above constitute the ==Kolmogorov continuity lemma==. This applies to any stochastic process $(X_{t})_{t\geq 0}$ as long as the moment estimate holds.

## Application to Brownian Motion

We know that $|X_{s}-X_{t}|\sim \mathcal{N}(0,t-s)$, which means that
$$
\mathbb{E}[|X_{s}-X_{t}|^{q}]\sim(t-s)^{q/2}.
$$
Then, $\varepsilon=\frac{q}{2}-1$, so our bound gives us that Brownian motion is $\alpha$-Holder continuous for all $\alpha< \frac{1}{2}$. This is essentially optimal: even $\alpha=\frac{1}{2}$ does not work!

So far, we've worked on $[0,1]$, which can be scaled to $[0,M]$ for any $M$. How would we extend to $\mathbb{R}$?

> [!theorem]
> Let $(X_{t})_{t\geq 0}$ be a Gaussian process with covariance function $\Gamma(s,t)=\min\{ s,t \}$. Then $X$ has a modification $B$ that is locally $\alpha$-Holder continuous, i.e. $|B_{t}(\omega)-B_{s}(\omega)|\leq K_{\alpha,M}(\omega)|s-t|^{\alpha}$ for all $s,t\in[0,M]$.

This is because we cannot get a uniform moment bound on the entire real line, so our constant $K$ must depend on the size $M$.

---



