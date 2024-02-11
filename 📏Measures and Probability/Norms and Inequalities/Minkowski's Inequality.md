It turns out that the $L^p$-norm satisfies the triangle inequality. This is essential for the theory of [[⛺Lebesgue Spaces Homepage|Lebesgue spaces]], the topic of the next section.

## Statement

> [!theorem] Theorem (Minkowski's Inequality)
> For $p \in[1,\infty)$ and measurable functions $f$ and $g$, we have
> $$
> \| f+g \| _{p}\leq \| f \|_{p}+\| g \| _{p}.
> $$

## Proof

The cases where $p=1$ or where $\| f \|_{p}=\infty$ or $\| g \|_{p}=\infty$ are easy. Now, note that $|f+g|^{p}\leq 2^{p}(|f|^{p}+|g|^{p})$ pointwise,  so integrating gives
$$
\mu(|f+g|^{p})\leq 2^p\left( \mu(|f|^{p})+\mu(|g|^{p}) \right) < \infty.
$$
We can also assume that $\| f+g \|_{p}>0$, since otherwise the inequality is clear. Let $q$ be the [[Hölder's Inequality#^fe7139|conjugate index]] of $p$. Observe that
$$
\| |f+g|^{p-1} \| _{q}=\mu \left( |f+g|^{(p-1)q} \right) ^{1/q}=\mu(|f+g|^{p})^{1-1/p}.
$$
Also, by [[Hölder's Inequality#^314c0f|Hölder's inequality]], we know that
$$
\begin{align*}
\mu(|f+g|^{p})&\leq \mu(|f| |f+g|^{p-1})+\mu(|g| |f+g|^{p-1})\\
&\leq(\| f \|_{p}+\| g \|_{p})\| |f+g|^{p-1} \| _{q},
\end{align*}
$$
so the result follows by dividing both sides by the common term $\| |f+g|^{p-1} \|_{q}$.

---

**Next:** [[Various Inequalities]]