We begin with a basic but easy to use condition for convergence in $L^{1}$.

> [!theorem] Theorem (Bounded Convergence)
> Let $(X_{n}:n\in \mathbb{N})$ be a sequence of random variables, with $X_{n}\to X$ in probability and $|X_{n}|\leq C$ for all $n$, for some constant $C<\infty$. Then $X_{n}\to X$ in $L^{1}$.

> [!proof]-
> By [[Convergence of Measurable Functions#^dda4e5|a previous theorem]], $X$ is the almost sure limit of some subsequence of $X_{n}$, which means that $|X|\leq C$ almost surely. For $\varepsilon>0$, there exists $N$ such that $n\geq N$ implies
> $$
> \mathbb{P}(|X_{n}-X|>\varepsilon/2)\leq\varepsilon/(4C).
> $$
> Then,
> $$
> \mathbb{E}[|X_{n}-X|]=\mathbb{E}[|X_{n}-X|\mathbf{1}_{|X_{n}-X|>\varepsilon / 2}]
> +\mathbb{E}[|X_{n}-X|\mathbf{1}_{|X_{n}-X|\leq\varepsilon / 2}]
> \leq 2C(\varepsilon / 4C)+\varepsilon / 2 = \varepsilon.
> $$

---

**Next:** [[Uniform Integrability]]