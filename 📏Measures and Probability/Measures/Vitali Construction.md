This is a pathological example that demonstrates why it is impossible to define a sensible measure on *all* subsets of $[0,1)$. Sensible measures on all subsets of $\mathbb{R}$ are therefore impossible as well.

There are three natural conditions we'd want a sensible measure $\mu$ on $[0,1)$ to satisfy:
1. It respects the lengths of intervals: $\mu([a,b])=b-a$.
2. It satisfies [[Set Functions#^164613|countable additivity]], i.e.
$$
\mu \left( \bigsqcup_{i=1}^{\infty}A_{i} \right)=\sum_{i=1}^{n} \mu(A_{i}).
$$
3. It should be <span style="color:#0088ff">translation invariant</span>, i.e. if $A\subseteq[0,1)$ and $A_{x}=A+x\pmod{1}$, then $\mu(A)=\mu(A_{x})$.

> [!definition] Definition (Vitali construction)
> For $x,y\in[0,1)$, we will write $x\sim y$ if $x-y\in\mathbb{Q}$. Then, $\sim$ is an equivalence relation. By the Axiom of Choice, we can find a subset $V\subseteq[0,1)$ with exactly one representative from each equivalence class. We call $V$ the <span style="color:#0088ff">Vitali set</span>.

> [!proposition] Proposition 
> The Vitali set $V=\mathbb{R}/\mathbb{Q}$ is not Lebesgue measurable.

> [!proof]-
> We consider all shifts $V+q=\{ v+q\pmod{1} : v\in V \}$. Clearly, all these cosets $V+q$ are disjoint and their union is $[0,1)$. However, by translation invariance, this tells us that
> $$
> 1=\mu([0,1))=\sum_{q\in\mathbb{Q}}\mu(S+q)=\sum_{q\in\mathbb{Q}}\mu(S).
> $$
> However, since $\mathbb{Q}$ is countable, this is not possible: $\mu(S)$ cannot be zero nor positive.

For more pathology, visit the [[Banach-Tarski Paradox]].

---

**Next:** [[Lebesgue Measure]]

