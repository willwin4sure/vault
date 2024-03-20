This is a useful information-theoretic interpretation of sufficient statistics.

> [!theorem] (Data Processing Inequality)
> If $\mathsf{t}$ is any statistic, then
> $$
> I(\mathsf{x};\mathsf{y})\geq I(\mathsf{x};\mathsf{t}),
> $$
> with equality if and only if $\mathsf{t}$ is a sufficient statistic.

> [!proof]-
> Use the chain rule of mutual information to get
> $$
> I(\mathsf{x};\mathsf{t})+I(\mathsf{x};\mathsf{y}|\mathsf{t})=I(\mathsf{x};\mathsf{y})+I(\mathsf{x};\mathsf{t}|\mathsf{y}).
> $$
> However, $I(\mathsf{x};\mathsf{t}|\mathsf{y})=0$ since $\mathsf{t}$ is a function of $\mathsf{y}$, which finishes since all terms are positive. Equality holds exactly when $I(\mathsf{x};\mathsf{y}|\mathsf{t})=0$ as well, which would mean that $\mathsf{t}$ is sufficient.

> [!idea]
> Thus a statistic $\mathsf{t}$ is sufficient if and only if provides as much information about $\mathsf{x}$ as $\mathsf{y}$ does. This is rather intuitively pleasing.

---

**Next:** [[KL Divergence]]