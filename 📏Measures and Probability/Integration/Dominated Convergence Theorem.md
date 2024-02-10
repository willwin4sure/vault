We're actually gonna prove another theorem that is a corollary of [[Fatou's Lemma]], which will allow us to immediately show the Dominated Convergence Theorem.
## Statements

> [!theorem] Theorem (Fatou-Lebesgue Theorem)
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of such functions. Suppose that $g$ is some integrable function such that $|f_{n}|\leq g$ for all $n$. Then, the inequality
> $$
> \mu \left( \liminf_{ n \to \infty } f_{n} \right)\leq \liminf_{ n \to \infty } \mu(f_{n})\leq \limsup_{ n \to \infty } \mu(f_{n})\leq \mu \left( \limsup_{ n \to \infty } f_{n} \right)  
> $$
> holds.

> [!theorem] Theorem (Dominated Convergence Theorem)
> Let $f_{n}$ and $g$ be as above, but this time suppose that $f_{n}(x)\to f(x)$ for all $x \in E$. Then, $\mu(f_{n})\to \mu(f)$.
## Proof of Fatou-Lebesgue

The first inequality follows by [[Fatou's Lemma]] on $g+f_{n}$, which is nonnegative. In particular, we can write
$$
\mu(g)+\mu(f)=\mu \left( \liminf_{ n \to \infty } (g+f_{n}) \right) \leq \liminf_{ n \to \infty }\mu(g+f_{n})=\mu(g)+\liminf_{ n \to \infty } \mu(f_{n}).
$$
Similarly, by Fatou on $g-f_{n}$, we get
$$
\mu(g)-\mu(f)=\mu \left( \liminf_{ n \to \infty } (g-f_{n}) \right) \leq \liminf_{ n \to \infty } \mu(g-f_{n})=\mu(g)-\limsup_{ n \to \infty } \mu(f_{n}).
$$
Since $g$ is integrable, we can subtract from both sides to get the desired inequality.

## Extension to Dominated Convergence

Note that $|f|\leq g$ so $f$ is integrable. Since $\liminf_{ n \to \infty }f_{n}=\limsup_{ n \to \infty }f_{n}=f$, so equality holds, and the liminf and limsup of $\mu(f_{n})$ is squeezed to $\mu(f)$.

---

**Next:** [[Transformations of Integrals]]