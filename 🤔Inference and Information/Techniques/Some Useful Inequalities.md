> [!theorem] (Csiszár's inequality)
> Given positive finite-length sequences $a_{1},\dots,a_{N}$ and $b_{1},\dots,b_{N}$ and a strictly convex function $f(\bullet)$, we have
> $$
> \sum_{i=1}^{N}b_{i}f\left( \frac{a_{i}}{b_{i}} \right) \geq \left( \sum_{i=1}^{N} b_{i} \right) f\left( \frac{\sum_{i=1}^{N}a_{i}}{\sum_{i=1}^{N}b_{i}} \right)
> $$
> with equality if and only if $\frac{a_{i}}{b_{i}}$ is constant over $i$.

> [!proof]-
> Let $a=\sum_{i=1}^{N}a_{i}$ and $b=\sum_{i=1}^{N}b_{i}$. Then,
> $$
> \sum_{i=1}^{N}b_{i}f\left( \frac{a_{i}}{b_{i}} \right)=b\sum_{i=1}^{N} \frac{b_{i}}{b}f\left( \frac{a_{i}}{b_{i}} \right) \geq b f\left( \sum_{i=1}^{N} \frac{b_{i}}{b} \frac{a_{i}}{b_{i}} \right) = b f\left( \frac{a}{b} \right)
> $$
> by [[Jensen's Inequality|Jensen's inequality]] on the probability distribution $\frac{b_{i}}{b}$.

We can apply this directly to $f(x)=x\log x$ to get:

> [!claim] (Log-sum inequality)
> Given positive finite-length sequences $a_{1},\dots,a_{N}$ and $b_{1},\dots,b_{N}$,
> $$
> \sum_{i=1}^{N}a_{i}\log \frac{a_{i}}{b_{i}}\geq \left( \sum_{i=1}^{N}a_{i} \right) \log \frac{\sum_{i=1}^{N}a_{i}}{\sum_{i=1}^{N}b_{i}}.
> $$
> 

This is exactly the discrete case of the following inequality. These both show up soon in information geometry, when we show that the [[KL divergence is positive]].

> [!theorem] (Gibbs' inequality)
> Let $\mathsf{v}$ be a random variable distributed according to the distribution $p(\bullet)$. Then, for any distribution $q(\bullet)$,
> $$
> \mathbb{E}_{p}\left[ \log p(\mathsf{v}) \right] \geq \mathbb{E}_{p}\left[ \log q(\mathsf{v}) \right].
> $$

^e52776

> [!proof]-
> By the concavity of the log function,
> $$
> \mathbb{E}_{p}\left[\log \frac{q(\mathsf{v})}{p(\mathsf{v})}\right]\leq \log \mathbb{E}_{p}\left[ \frac{q(\mathsf{v})}{p(\mathsf{v})} \right] = 0.
> $$

---

**Next:** [[The Expectation Maximization (EM) Algorithm]]



