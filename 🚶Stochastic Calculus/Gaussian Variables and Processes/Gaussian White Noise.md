The heuristic for Gaussian white noise is that for every "pixel" of space, there is an independent Gaussian random variable.

> [!definition] (Gaussian white noise)
> Let $(E,\mathcal{E})$ be a measurable space, and let $\mu$ be a $\sigma$-finite measure on $(E,\mathcal{E})$. Then, a ==Gaussian white noise with intensity $\mu$== on $(E,\mathcal{E})$ is a linear isometry $G:L^{2}(E,\mathcal{E},\mu)\to H$, where $H\subseteq L^{2}(\Omega,\mathcal{F},\mathbb{P})$ is a centered Gaussian space.

An isometry is defined to preserve the inner product, so $\langle f,g \rangle=\langle G(f),G(g) \rangle$.

How do we interpret this definition?

> [!idea]
> First think about indicator functions over small chunks $dx$ in $E$. The isometry condition forces it to be mapped to a Gaussian variable $\mathcal{N}(0,\mu(dx))$.

Further, each of these patches must be independent from one another. In particular, an indicator function on any set $A$ gets mapped to a normal distribution with variance $\mu(A)$; if we want what $G(f)$ is for an arbitrary function $f$, just do a weighted sum of all these small Gaussians on patches.

---

**Next:** [[⛺Gaussian Variables and Processes Homepage]]

