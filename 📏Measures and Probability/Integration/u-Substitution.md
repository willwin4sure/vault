Also from your AP Calculus class. The integral is with respect to the Lebesgue measure.
## Statement

> [!proposition] Proposition ($u$-substitution)
> Let $\phi:[a,b]\to \mathbb{R}$ be continuously differentiable and strictly increasing. Then, for all nonnegative Borel functions $g$ on $[\phi(a),\phi(b)]$,
> $$
> \int_{\phi(a)}^{\phi(b)} g(y) \, dy = \int_{a}^{b} g(\phi(x))\phi'(x) \, dx .
> $$

Here, $y$ takes the place of $u$, and the $u$-substitution is by $u=\phi(x)$.

## Proof Sketch

The general procedure follows the [[Measurable Functions#^f79049|monotone class theorem]]. The statement is true for indicator functions on intervals by the [[The Fundamental Theorem of Calculus#^52fa03|fundamental theorem of calculus]]. This is a $\pi$-system generating the Borel $\sigma$-algebra. Then, we need to show that the set of all Borel sets $B$ such that $\mathbf{1}_{B}$ is a valid function forms a $\lambda$-system, so by Dynkin's $\pi$-$\lambda$ Theorem it is the full Borel $\sigma$-algebra.

The identity then extends to simple functions by linearity and then to arbitrary nonnegative measurable functions $g$ by monotone convergence, using dyadic grid simple functions $(2^{-n}\lfloor 2^{n}g \rfloor)\land n$.

---

**Next:** [[Differentiation Under the Integral Sign]]