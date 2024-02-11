For $p,q\in[1,\infty]$, we say that $p$ and $q$ are ==conjugate indices== if ^fe7139
$$
\frac{1}{p}+\frac{1}{q}=1.
$$
## Statement

> [!theorem] Theorem (Hölder's Inequality)
> Let $p,q\in(1,\infty)$ be conjugate indices. Then, for all measurable functions $f$ and $g$, we have
> $$
> \mu(|fg|)\leq \| f \|_{p}\| g \|_{q}.
> $$

^314c0f

## Proof

The cases where $\| f \|_{p}=0$ or $\| f \|_{p}=\infty$ are obvious. Therefore, we can rescale $f$ so that $\| f \|_{p}=1$. Therefore, we can let $|f|^{p}$ [[Transformations of Integrals#^ce3afe|induce a probability measure]] $\mathbb{P}$ on $\mathcal{E}$, given by
$$
\mathbb{P}(A)=\int _{A}|f|^{p} \, d\mu. 
$$
For measurable functions $X\geq 0$,
$$
\mathbb{E}[X]=\mu(X|f|^{p}),\quad \mathbb{E}[X]\leq \mathbb{E}[X^{q}]^{1/q}.
$$
Note that $p=q(p-1)$ by assumption, so
$$
\begin{align*}
\mu(|fg|)&=\mu \left( \frac{|g|}{|f|^{p-1}}\mathbf{1}_{\{ |f|>0 \}}|f|^{p} \right) \\
&=\mathbb{E}\left( \frac{|g|}{|f|^{p-1}}\mathbf{1}_{\{ |f|>0 \}} \right) \\
&\leq \mathbb{E}\left( \frac{|g|^{q}}{|f|^{q(p-1)}} \mathbf{1}_{\{ |f| > 0 \}} \right)^{1/q}\\
&\leq \mu(|g|^{q})^{1/q}=\| f \| _{p}\| g \| _{q},
\end{align*}
$$
where the first inequality is by [[Jensen's Inequality#^77aeb6|Jensen's inequality]] (remember that $\| f \|_{p}=1$).

> [!remark]
> The case where $p=q=2$ is the famous Cauchy-Schwarz inequality.

---

**Next:** [[Minkowski's Inequality]]