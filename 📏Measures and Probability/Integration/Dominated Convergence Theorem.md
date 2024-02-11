This theorem showcases another time when it is possible to interchange limits and integrals, as in the [[Monotone Convergence Theorem#^aedc13|monotone convergence theorem]] we saw last time.

The proof follows almost directly from some preliminary reading:

1. [[Fatou-Lebesgue Theorem]]
## Statement

> [!theorem] Theorem (Dominated Convergence)
> Let $f$ be a measurable function and let $(f_{n}:n\in \mathbb{N})$ be a sequence of such functions. Suppose that $f_{n}(x)\to f(x)$ for all $x \in E$ and that $|f_{n}|\leq g$ for all $n$, for some integrable function $g$. Then $f$ and $f_{n}$ are integrable, for all $n$, and $\mu(f_{n})\to \mu(f)$.

^541a22

## Proof

Note that $|f|\leq g$ so $f$ is integrable. Since $\liminf_{ n \to \infty }f_{n}=\limsup_{ n \to \infty }f_{n}=f$, equality holds, and the liminf and limsup of $\mu(f_{n})$ are squeezed to $\mu(f)$.

---

**Next:** [[Transformations of Integrals]]