Integration in one variable and differentiation in another can be interchanged subject to some regularity conditions. This is what is used in Feynman's trick for integration.
## Statement

> [!theorem] Theorem (Differentiation Under the Integral Sign)
> Let $U\subseteq \mathbb{R}$ be open and suppose that $f:U\times E\to \mathbb{R}$ satisfies:
> 1. $x\mapsto f(t,x)$ is integrable for all $t$.
> 2. $t\mapsto f(t,x)$ is differentiable for all $x$.
> 3. For some integrable function $g$, for all $x \in E$ and all $t\in U$,
> $$
> \left| \frac{ \partial f }{ \partial t } (t,x) \right| \leq g(x).
> $$
> Then the function $x\mapsto(\partial f / \partial t)(t,x)$ is integrable for all $t$. Moreover, the function $F:U\to \mathbb{R}$, defined by
> $$
> F(t)=\int _{E}f(t,x) \, \mu(dx), 
> $$
> is differentiable and
> $$
> \frac{d}{dt}F(t)=\int _{E}\frac{ \partial f }{ \partial t } (t,x) \, \mu(dx). 
> $$

^df8870

## Proof

 Take any sequence $h_{n}\to 0$ and set
 $$
g_{n}(x)=\frac{f(t+h_{n},x)-f(t,x)}{h_{n}}-\frac{ \partial f }{ \partial t } (t,x).
$$
Then $g_{n}(x)\to 0$ for all $x \in E$ and, by the mean value theorem, $|g_{n}|\leq 2g$ for all $n$. In particular, for all $t$, the function $x\mapsto (\partial t / \partial x)(t,x)$ is the limit of measurable functions, hence measurable, and hence integrable. Then, by the [[Dominated Convergence Theorem#^541a22|dominated convergence theorem]],
$$
\frac{F(t+h_{n})-F(t)}{h_{n}}-\int _{E}\frac{ \partial f }{ \partial t } (t,x) \, \mu(dx)
=\int _{E}g_{n}(x) \, \mu(dx),
$$
which goes to 0.

---

**Next:** [[Integration Techniques]]