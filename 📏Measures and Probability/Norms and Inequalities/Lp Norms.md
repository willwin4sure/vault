This short section introduces the $L^{p}$ norm, which is a way of measuring distance between two measurable functions.

Work over a measure space $(E,\mathcal{E},\mu)$. 

> [!definition] Definition ($L^{p}$-norm)
> For $1\leq p<\infty$, we define the ==$L^{p}$-norm== of a measurable function $f:E\to \mathbb{R}$ as
> $$
> \| f \|_{p}=\left( \int _{E}|f|^{p} \, d\mu  \right) ^{1/p}.
> $$
> For the case of $p=\infty$, we define the ==$L^{\infty}$-norm== as
> $$
> \| f \| _{\infty}=\inf\{ \lambda:|f|\leq\lambda \text{ a.e.} \}.
> $$ 

^ac55c6

> [!notation]
> We denote by $L^{p}$ the space of measurable functions with *finite* $L^{p}$-norm, for all $p \in[0,\infty ]$.

Note that $\| f \|_{p}\leq \mu(E)^{1/p}\| f \|_{\infty}$ for all $1\leq p<\infty$.

Here is another notion of [[Convergence of Measurable Functions|convergence of measurable functions]]. This is stronger than convergence in measure, and on a separate branch from convergence almost everywhere (neither implies the other).

> [!definition] Definition (Convergence in $L^{p}$)
> For any $1\leq p\leq \infty$ and $f_{n},f\in L^{p}$, we say that $f_{n}$ ==converges in $L^p$== to $f$ if $\| f_{n}-f \|_{p}\to 0$.

---

**Next:** [[Various Inequalities]]