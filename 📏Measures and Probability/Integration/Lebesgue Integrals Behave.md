This is just some housekeeping work to prove some basic properties of Lebesgue integrals. These are things that you should already expect to be true.

> [!proposition]
> For all nonnegative measurable functions $f,g$ and all constants $\alpha,\beta \geq 0$, 
> 
> 1. $\mu(\alpha f+\beta g)=\alpha \mu(f)+\beta \mu(g)$.
> 2. $f\leq g$ implies $\mu(f)\leq \mu(g)$.
> 3. $f=0$ a.e. if and only if $\mu(f)=0$.

^38742c

> [!proof]-
> Define simple functions $f_{n},g_{n}$ by
> $$
> f_{n}=(2^{-n}\lfloor 2^{n}f \rfloor )\land n,\quad g_{n}=(2^{-n}\lfloor 2^{n}g \rfloor )\land n.
> $$
> Then $f_{n}\uparrow f$ and $g_{n}\uparrow g$, so $\alpha f_{n}+\beta g_{n}\uparrow\alpha f+\beta g$. Hence, by monotone convergence,
> $$
> \mu(f_{n})\uparrow \mu(f),\quad \mu(g_{n})\uparrow \mu(g),\quad\mu(\alpha f_{n}+\beta g_{n})\uparrow \mu(\alpha f+\beta g).
> $$
> We know for simple functions that $\mu(\alpha f_{n}+\beta g_{n})=\alpha \mu(f_{n})+\beta \mu(g_{n})$, so the first property is proven.
> 
> The second property is obvious from the definition of the integral.
> 
> The third property is true since if $f=0$ a.e., then $f_{n}=0$ a.e. for all $n$, so $\mu(f_{n})=0$; hence $\mu(f)=0$. For the other direction, if $\mu(f)=0$ then $\mu(f_{n})=0$ for all $n$, so $f_{n}=0$ a.e. and therefore $f=0$ a.e.

The above also holds for all [[Lebesgue Integration#^9d6fef|integrable]] functions $f,g$, but property 3 is weakened to only the forward direction (obviously the reverse fails if we can have negative parts as well).

Here is another occasionally useful result:

> [!proposition]
> Let $\mathcal{A}$ be a $\pi$-system containing $E$ and generating $\mathcal{E}$. Then, for any integrable function $f$, $\mu(f\mathbf{1}_{A})=0$ for all $A\in \mathcal{A}$ implies $f=0$ a.e.

---

**Return:** [[Monotone Convergence Theorem]]