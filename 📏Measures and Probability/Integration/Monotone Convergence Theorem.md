This is the key result in the theory of integration. It allows us to interchange limits and integrals for monotonic sequences of nonnegative measurable functions.
## Statement

> [!theorem] Theorem (Monotone Convergence)
> Let $f$ be a nonnegative measurable function and let $(f_{n}:n\in \mathbb{N})$ be a sequence of such functions. Suppose that $f_{n}\uparrow f$. Then, $\mu(f_{n})\uparrow \mu(f)$.

^aedc13

## Proof

Set $M=\sup_{n}\mu(f_{n})$. We know that
$$
\mu(f_{n})\uparrow M\leq \mu(f)=\sup \{ \mu(g) : g\text{ simple}, g\leq f \}.
$$
Therefore, to show equality, it suffices to show that $\mu(g)\leq M$ for all [[Lebesgue Integration#^a0fb65|simple functions]]
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

## Alternate Proof

Here's an alternate proof using [[Fatou-Lebesgue Theorem#^11e9b4|Fatou's lemma]]. This is not our ground truth, since the proof we present of Fatou's lemma uses monotone convergence.

We can write
$$
\begin{align*}
\mu(f)&=\mu\left(\liminf_{ n \to \infty } f_{n}\right)\\
&\leq \liminf_{ n \to \infty } \mu(f_{n})\\
&\leq \limsup_{ n \to \infty } \mu(f_{n})\\
&\leq \mu(f).
\end{align*}
$$
The first inequality is by Fatou, the second is obvious, and the final one is because $f_{n}\leq f$ always so $\mu(f_{n})\leq \mu(f)$ [[Lebesgue Integrals Behave#^38742c|as well]]. Since equality holds, $\mu(f)=\lim_{ n \to \infty }\mu(f_{n})$, as desired.

## Reformulations

Here are some minor variants on the monotone convergence theorem.

> [!proposition]
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of nonnegative measurable functions. Then,
> $$
> f_{n}\uparrow f\text{ a.e.}\implies \mu(f_{n})\uparrow \mu(f).
> $$
 
> [!proposition]
> Let $(g_{n}:n\in \mathbb{N})$ be a sequence of nonnegative measurable functions. Then,
> $$
> \sum_{n=1}^{\infty}\mu(g_{n})=\mu \left( \sum_{n=1}^{\infty}g_{n} \right).
> $$

^129ee0

> [!idea]
> The second reformulation makes it clear that the monotone convergence theorem is the counterpart for the integration of functions of the countable additivity property of the measure on sets.

Using monotone convergence, you can check that Lebesgue integrals satisfy some basic properties:

1. [[Lebesgue Integrals Behave]]

---

**Next:** [[Dominated Convergence Theorem]]