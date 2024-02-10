## Statement

> [!theorem] Fatou's Lemma
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of nonnegative measurable functions. Then,
> $$
> \mu\left(\liminf_{ n \to \infty } f_{n}\right)\leq \liminf_{ n \to \infty } \mu(f_{n}).
> $$
> 

## Proof

For $k\geq n$, we have
$$
\inf_{m\geq n}f_{m}\leq f_{k}.
$$
Therefore,
$$
\mu\left(\inf_{m\geq n}f_{m}\right)\leq \inf_{k\geq n}\mu(f_{k})\leq \liminf_{ n \to \infty } \mu(f_{n}).
$$
However, as $n\to \infty$, we have that
$$
\inf_{m\geq n}f_{m}\uparrow \sup_{n}\left( \inf_{m\geq n}f_{m} \right) =\liminf_{ n \to \infty } f_{n}.
$$
Therefore, by the monotone convergence theorem,
$$
\mu \left( \inf_{m\geq n}f_{m} \right) \uparrow \mu\left(\liminf_{ n \to \infty } f_{n}\right).
$$
The result follows.

---

**Next:** [[Dominated Convergence Theorem]]