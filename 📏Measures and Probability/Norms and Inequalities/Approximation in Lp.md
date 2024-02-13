The following theorem states that linear combinations of indicator functions over a $\pi$-system are dense over $L^{p}$. 

## Statement

> [!theorem]
> Let $\mathcal{A}$ be a $\pi$-system on $E$ generating $\mathcal{E}$, with $\mu(A)<\infty$ for all $A\in \mathcal{A}$, and such that $E_{n}\uparrow E$ for some sequence $(E_{n}:n\in \mathbb{N})$ in $\mathcal{A}$. Define
> $$
> V_{0}=\left\{  \sum_{k=1}^{n}a_{k}\mathbf{1}_{A_{k}} : a_{k}\in \mathbb{R}, A_{k}\in \mathcal{A}, n\in \mathbb{N} \right\}.
> $$
> Let $p \in[1,\infty)$. Then $V_{0}\subseteq L^p$, and moreover for all $f\in L^{p}$ and all $\varepsilon>0$, there exists $v\in V_{0}$ such that $\| v-f \|_{p}\leq\varepsilon$.

## Proof

For each $A\in \mathcal{A}$, $\| \mathbf{1}_{A} \|_{p}=\mu(A)^{1 / p}<\infty$, so $\mathbf{1}_{A}\in L^{p}$. Hence, $V_{0}\subseteq L^{p}$ since $L^{p}$ is a vector space.

Write $V$ for the set of all $f\in L^{p}$ for which the conclusion holds, i.e. it can be closely approximated in $L^{p}$. By Minkowski's inequality, $V$ is a vector space.

First, we will solve the problem under the assumption that $E\in \mathcal{A}$.

Define $\mathcal{L}=\{ A\in \mathcal{E} : \mathbf{1}_{A}\in V \}$; we will show that this is a $\lambda$-system. Evidently $\mathcal{A}\subseteq \mathcal{L}$, so $E\in \mathcal{L}$. If we pick any two $A,B\in \mathcal{D}$ with $A\subseteq B$, then $\mathbf{1}_{B\setminus A}=\mathbf{1}_{B}-\mathbf{1}_{A}\in V$, so $B\setminus A\in \mathcal{L}$. Further, for any ascending sequence $A_{n}\in \mathcal{L}$ with $A_{n}\uparrow A$, we have
$$
\| \mathbf{1}_{A}-\mathbf{1}_{A_{n}} \|_{p} = \mu(A\setminus A_{n})^{1/p}\to 0.
$$
Hence $A\in \mathcal{L}$ as well. Therefore, $\mathcal{L}$ is a $\lambda$-system that contains the $\pi$-system $\mathcal{A}$, so $\mathcal{L}=\mathbb{E}$ by Dynkin's lemma.

Therefore, $V$ contains all simple functions. We extend this to all nonnegative $f\in L^{p}$ by considering the sequence of simple functions $f_{n}=2^{-n}\lfloor 2^{n}f \rfloor\land n \uparrow f$. Then, pointwise, $|f-f_{n}|^{p}\to 0$, and every function is bounded above by $|f|^{p}$, which means we can apply the [[Dominated Convergence Theorem|dominated convergence theorem]] to get that $\| f-f_{n} \|_{p}\to 0$, so $f\in V$. Therefore, $V=L^{p}$ as vector spaces.

Now, remove the assumption that $E\in A$, and instead consider our sequence $(E_{n}:n\in \mathbb{N})$ in $\mathcal{A}$ approaching $E$. For each $n\in \mathbb{N}$, we can consider restricting everything to that subset; this tells us that $f\mathbf{1}_{E_{n}}\in V$. Yet $|f-f\mathbf{1}_{E_{n}}|^{p}\to 0$ pointwise, and they are all bounded above by $|f|^{p}$. This allows us to apply the dominated convergence theorem again to get that $\| f-f\mathbf{1}_{E_{n}} \|_{p}\to 0$, so $f\in V$.

---

**Next:** [[⛺Norms and Inequalities Homepage]]