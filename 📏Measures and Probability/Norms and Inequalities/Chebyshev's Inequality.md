This is also called a Markov bound.

> [!claim] Claim (Chebyshev's inequality)
> Let $f$ be a nonnegative measurable function and let $\lambda \geq 0$. We use the notation $\{ f\geq\lambda \}$ for the set $\{ x \in E : f(x)\geq\lambda \}$. Then,
> $$
> \lambda \mu(f\geq \lambda)\leq \mu(f).
> $$

The proof is trivial: it directly follows from integrating on the fact $\lambda \mathbf{1}_{f\geq\lambda}\leq f$.

This allows us to obtain certain *tail estimates*. For example, if $g\in L^{p}$ for $p<\infty$ and $\lambda>0$, then
$$
\mu(|g|\geq\lambda)=\mu(|g|^{p}\geq\lambda^{p})\leq\lambda^{-p}\mu(|g|^{p})<\infty.
$$
This tells us that $\mu(|g|\geq\lambda)=\mathcal{O}(\lambda^{-p})$ as $\lambda\to \infty$ for any functions in $L^{p}$.

---

**Next:** [[Jensen's Inequality]]