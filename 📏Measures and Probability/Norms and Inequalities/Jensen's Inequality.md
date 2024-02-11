You know all this from olympiad algebra. Let $I\subseteq \mathbb{R}$ be an interval.

> [!definition] Definition (Convex function)
> A function $c:I\to \mathbb{R}$ is called ==convex== if for all $x,y\in I$ and $t\in[0,1]$,
> $$
> c(tx+(1-t)y)\leq tc(x)+(1-t)c(y).
> $$

If a function is convex, then it lies above all tangent lines underneath it. 

> [!lemma]
> Let $c:I\to \mathbb{R}$ be convex and let $m$ be a point in the interior of $I$. Then, there exist $a,b\in \mathbb{R}$ such that $c(x)\geq ax+b$ for all $x$, with equality at $x=m$.

> [!proof]-
> By convexity, for $m,x,y\in I$ with $x<m<y$, we have
> $$
> \frac{c(m)-c(x)}{m-x}\leq \frac{c(y)-c(m)}{y-m}.
> $$
> So, if we fix an interior point $m$, there exists $a\in \mathbb{R}$ such that for all $x<m$ and all $y>m$,
> $$
> \frac{c(m)-c(x)}{m-x}\leq a\leq \frac{c(y)-c(m)}{y-m}.
> $$
> Then, $c(x)\geq a(x-m)+c(m)$ for all $x \in I$, for this value of $a$.

Now we can state and prove the theorem.

> [!theorem] Theorem (Jensen's Inequality)
> Let $X$ be an integrable random variable with values in $I$ and let $c:I\to \mathbb{R}$ be convex. Then $\mathbb{E}[c(X)]$ is well defined and
> $$
> \mathbb{E}[c(X)]\geq c(\mathbb{E}[X]).
> $$

^77aeb6

> [!proof]-
> The case where $X$ is almost surely constant is easy. Then $m=\mathbb{E}[X]$ must lie in the interior of $I$. Then, pick $a,b\in \mathbb{R}$ as in the lemma above, so $c(X)\geq aX+b$. In particular, $\mathbb{E}[c(X)^{-}]\leq|a|\mathbb{E}(|X|)+|b|<\infty$, so $\mathbb{E}[c(X)]$ is well-defined. Further,
> $$
> \mathbb{E}[c(X)]\geq a\mathbb{E}[X]+b=am+b=c(m)=c(\mathbb{E}[X]),
> $$
> as desired.

We can deduce from Jensen's inequality the monotonicity of $L^{p}$-norms **with respect to a probability measure**.

> [!claim] Claim (Monotonicity of $L^{p}$ with respect to probability measures)
> Let $1\leq p<q<\infty$. Set $c(x)=|x|^{q/p}$, so it is convex on $\mathbb{R}$. For any $X \in L^p(\mathbb{P})$,
> $$
> \| X \| _{p}=(\mathbb{E}|X|^{p})^{1/p}=(c(\mathbb{E}|X|^{p}))^{1/q}
> \leq (\mathbb{E}c(|X|^p))^{1/q}=(\mathbb{E}|X|^q)^{1/q}=\| X \| _{q}.
> $$
> In particular, this implies that $L^{q}(\mathbb{P})\subseteq L^{p}(\mathbb{P})$.

Warning, this does not work for general measures! In particular, we could consider $f$ as the indicator function on a measurable set with finite measure greater than $1$. Then, as $p$ increases, the $L^{p}$-norm of $f$ decreases! This is because Jensen's inequality requires *averages*: for example, even though $\frac{a^{2}+b^{2}}{2}\geq \left( \frac{a+b}{2} \right)^{2}$, obviously it is not true that $a^{2}+b^{2}\geq (a+b)^{2}$.

---

**Next:** [[Hölder's Inequality]]