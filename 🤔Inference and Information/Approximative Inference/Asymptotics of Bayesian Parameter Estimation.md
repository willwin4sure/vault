Now our parameter $x \in \mathcal{X}$ is random and generated from a prior distribution $p_{\mathsf{x}}(\bullet)$, and our data $y_{1},\dots,y_{N}$ are generated i.i.d. according to $p_{\mathsf{y}|\mathsf{x}}(\bullet|x)\in \mathcal{P}^{\mathcal{Y}}$.

The normalization of the posterior distribution is given by
$$
\int p_{\mathsf{x}}(a)\prod_{n=1}^{N}p_{\mathsf{y}|\mathsf{x}}(y_{n}|a) \, da. 
$$
but it is rather difficult to evaluate. One method is through [[Monte Carlo Methods|stochastic approximations]], but we present another method.

## Laplace's Method

This seeks to approximate an integral of the form
$$
\int_{a}^{b} e^{Ng(z)} \, dz, 
$$
where $g$ is smooth and $N$ is large. We do this by considering a Taylor series for $g(z)$ about its maximum $z_{*}=\arg\max_{z \in [a,b]}g(z)$, which gives
$$
\int_{a}^{b}e^{Ng(z)} \, dz \approx e^{Ng(z_{*})}\int_{a}^{b}e^{-N/2\cdot|\ddot{g}(z_{*})|(z-z_{*})^{2}} \, dz
\approx e^{Ng(z_{*})}\sqrt{ \frac{2\pi}{N|\ddot{g}(z_{*})|} },
$$
by approximating the integral using a Gaussian.

> [!theorem] (Laplace's method)
> Suppose $g:[a,b]\to \mathbb{R}$ is a twice differentiable and has a unique maximum at $z_{*}\in(a,b)$ such that $\ddot{g}(z_{*})<0$. Then,
> $$
> \frac{\int_{a}^{b}e^{Ng(z)} \, dz }{e^{Ng(z_{*})}\sqrt{ \frac{2\pi}{|N\ddot{g}(z_{*})|} }}\to 0
> $$
> as $N\to \infty$.

TODO

---

**Next:** [[Universal Inference]]