How do measures interact with [[Measurable Functions#^3c3914|measurable functions]] $(E,\mathcal{E})\to(G,\mathcal{G})$? Well, it turns out that if we have a measure $\mu$ on the first space, we can *push it forward* into a measure $\nu$ on the second space.

## Definition

> [!definition] Definition (Image measure)
> Let $(E,\mathcal{E})$ and $(G,\mathcal{G})$ be measurable spaces and let $\mu$ be a measure on $\mathcal{E}$. Then, any measurable function $f:E\to G$ induces an <span style="color:#0088ff">image measure</span> $\nu=\mu \circ f^{-1}$ on $\mathcal{G}$, given by
> $$
> \nu(A)=\mu(f^{-1}(A)).
> $$

^94991f

In other words, to measure anything in $\mathcal{G}$, we pull it back into $\mathcal{E}$ and use $\mu$.

## Constructing New Measures

This allows us to construct some new measures from our basic [[Lebesgue Measure|Lebesgue measure]] on $\mathbb{R}$.

> [!claim] Claim (Inverse CDFs)
> Let $g:\mathbb{R}\to \mathbb{R}$ be non-constant, right-continuous, and non-decreasing. Set $g(\pm \infty)=\lim_{ x \to \pm \infty }g(x)$ and write $I=(g(-\infty),g(\infty))$. Define $f:I\to \mathbb{R}$ by $f(x)=\inf\{ y\in \mathbb{R}: x\leq g(y) \}$. Then $f$ is left-continuous and non-decreasing. Moreover, for $x \in I$ and $y\in \mathbb{R}$,
> $$
> f(x)\leq y\quad\text{if and only if}\quad x\leq g(y).
> $$

^6db858

> [!proof]-
> Fix some $x \in I$ and consider $J_{x}=\{ y\in \mathbb{R} : x\leq g(y) \}$. Note that $f(x)=\inf J_{x}$. 
> 
> Here, $J_{x}$ is non-empty and is not the whole of $\mathbb{R}$. Since $g$ is non-decreasing, if $y\in J_{x}$ and $y'\geq y$, then $y'\in J_{x}$. Further, since $g$ is right-continuous, if $y_{n}\in J_{x}$ and $y_{n}\downarrow y$, then $y\in J_{x}$. Therefore, $J_{x}=[f(x),\infty)$ and $x\leq g(y)$ if and only if $f(x)\leq y$.
> 
> Finally, for $x\leq x'$, we have $J_{x}\supseteq J_{x'}$, so $f(x)\leq f(x')$. For $x_{n}\uparrow x$, we have $J_{x}=\bigcap_{n}^{}J_{x_{n}}$, so $f(x_{n})\to f(x)$. So $f$ is non-decreasing and left-continuous, as desired.

> [!remark]
> When $I=(0,1)$, you can think of $g$ like a CDF, and $f$ like an inverse CDF, where we break ties by going to the left.

> [!theorem] Theorem (Lebesgue-Stieltjes Measure)
> Let $g:\mathbb{R}\to \mathbb{R}$ be non-constant, right-continuous, and non-decreasing. Then, there exists a unique Radon measure $dg$ on $\mathbb{R}$ such that for all $a,b\in \mathbb{R}$ with $a<b$,
> $$
> dg((a,b])=g(b)-g(a).
> $$
> Moreover, we obtain in this way *all* non-zero Radon measures on $\mathbb{R}$. Such a measure $dg$ is called a <span style="color:#0088ff">Lebesgue-Stieltjes measure</span> associated with $g$.

^a35ae2

> [!proof]-
> Define $I$ and $f$ as in the previous proof. The idea is that we can push the Lebesgue measure on $I$ through $f$ (which is Borel measurable by pulling back open sets) to get the measure $dg$ on $\mathbb{R}$. Then, we can check that
> $$
> dg((a,b])=\mu(\{ x: f(x)\in(a,b] \})=\mu((g(a),g(b)])=g(b)-g(a).
> $$
> We can use the same [[Lebesgue Measure#^70558a|uniqueness proof for the Lebesgue measure]] here to show that $dg$ is unique (we had to adapt uniqueness of extensions from a $\pi$-system to the infinite $\mathbb{R}$). 
> 
> Finally, if $\nu$ is any Radon measure on $\mathbb{R}$, we can define $g:\mathbb{R}\to \mathbb{R}$, right-continuous and non-decreasing, by
> $$
> g(y)=\begin{cases}
> \nu((0,y]), & \text{if }y\geq 0, \\
> -\nu((y,0]), & \text{if }y<0.
> \end{cases}
> $$
> This is exactly what you should expect. Now, $\nu((a,b])=g(b)-g(a)$ by additivity whenever $a<b$ (note we use the finiteness of the Radon measure), so $\nu=dg$ by uniqueness.

---

**Next:** [[Random Variables]]
