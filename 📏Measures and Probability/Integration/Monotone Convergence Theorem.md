This is the key result in the theory of integration.

## Statement

> [!theorem] Theorem (Monotone convergence)
> Let $f$ be a nonnegative measurable function and let $(f_{n}:n\in \mathbb{N})$ be a sequence of such functions. Suppose that $f_{n}\uparrow f$. Then, $\mu(f_{n})\uparrow \mu(f)$.

## Proof

Set $M=\sup_{n}\mu(f_{n})$. We know that
$$
\mu(f_{n})\uparrow M\leq \mu(f)=\sup \{ \mu(g) : g\text{ simple}, g\leq f \}.
$$
Therefore, it suffices to show that $\mu(g)\leq M$ for all simple functions
$$
g=\sum_{i=1}^{m}a_{k}\mathbf{1}_{A_{k}}\leq f.
$$
Without loss of generality, assume that the measurable sets $A_{k}$ are disjoint. Define functions $g_{n}$ by
$$
g_{n}(x)=(2^{-n}\lfloor 2^nf_{n}(x) \rfloor )\land g(x),\quad x \in E.
$$
In other words, round $f_{n}$ down to dyadic grid size, and then cap it with $g(x)$. Now, $g_{n}$ is simple and $g_{n}\leq f_{n}$ for all $n$. Fix some $\varepsilon \in(0,1)$ and consider the sets
$$
A_{k}(n)=\{ x \in A_{k}:g_{n}(x)\geq(1-\varepsilon)a_{k} \}.
$$
That is, we consider the points $x$ where $g_{n}$ is $\varepsilon$-close to $g$. Now $g_{n}\uparrow g$ and $g=a_{k}$ on $A_{k}$, so $A_{k}(n)\uparrow A_{k}$. Thus $\mu(A_{k}(n))\uparrow \mu(A_{k})$ by upward continuity. Further, we have that
$$
\mathbf{1}_{A_{k}}\cdot g_{n}\geq(1-\varepsilon)a_{k}\cdot \mathbf{1}_{A_{k}(n)}
$$
so
$$
\mu(\mathbf{1}_{A_{k}}\cdot g_{n})\geq (1-\varepsilon)a_{k}\cdot \mu(A_{k}(n)).
$$
Finally, use the fact that
$$
g_{n}=\sum_{k=1}^{m}\mathbf{1}_{A_{k}}\cdot g_{n}
$$
to get that
$$
\mu(g_{n})=\sum_{k=1}^{m}\mu(\mathbf{1}_{A_{k}}\cdot g_{n})\geq(1-\varepsilon)\sum_{k=1}^{m}a_{k}\mu(A_{k}(n))\uparrow (1-\varepsilon)\sum_{k=1}^{m}a_{k}\mu(A_{k})=(1-\varepsilon)\mu(g).
$$
Basically, on each section $A_{k}$, eventually $g_{n}$ gets arbitrarily close to $g$, so in totality, the measure eventually gets close too. Finally, note that $\mu(g_{n})\leq \mu(f_{n})\leq M$ for all $n$, and that $\varepsilon \in(0,1)$ is arbitrary, so $\mu(g)\leq M$, as desired.

> [!remark]
> You don't actually need monotonicity of $f_{n}$. All you need is that $f_{n}\to f$ and $f_{n}\leq f$.

---

**Next:** [[Lebesgue Integrals Behave]]