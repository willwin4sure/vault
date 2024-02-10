Now we will begin our formal treatment of stochastic calculus. We'll begin by defining Gaussians. This should mostly be review.

> [!definition] Definition (Gaussian vectors)
> A ==$d$-dimensional Gaussian vector== is an $\mathbb{R}^{d}$-valued random variable $X$  such that $\langle X,u \rangle$ is a one-dimensional Gaussian variable for any $u\in \mathbb{R}^{d}$.

This is a fancier definition than the one you've seen from other courses, but it turns out to be equivalent. It also doesn't explicitly specify that $\langle X,u \rangle$ and $\langle X,v \rangle$ are jointly Gaussian for $u,v\in \mathbb{R}^{d}$, but this is a true consequence.

> [!proposition]
> The law of $X$ is uniquely determined by the ==mean vector== $\mu=\mathbb{E}[X]\in \mathbb{R}^{d}$ and the ==covariance matrix== $\Sigma=\mathbb{E}[(X-\mu)(X-\mu)^T]\in \mathbb{R}^{d\times d}$.

> [!proof]-
> The characteristic function is
> $$
> \phi_{X}(\theta)=\mathbb{E}\left[ e^{i\langle X,\theta \rangle } \right].
> $$
> By definition, $\langle X,\theta \rangle$ is a one-dimensional Gaussian; we can compute its mean as $\langle \mu,\theta \rangle$ and its variance as $\theta^T\Sigma\theta$. Therefore, we can write the above as
> $$
> \phi_{X}(\theta)=\exp \left( i\langle \theta,\mu \rangle -\frac{1}{2}\theta^T\Sigma\theta \right) .
> $$
> Since we know the characteristic function for all $\theta$, this uniquely determines the law of $X$, and coincides with the definition you are used to.

Now, we can simply write $X\sim \mathcal{N}(\mu,\Sigma)$, where $\mu \in \mathbb{R}^{d}$ and $\Sigma \in \mathbb{R}^{d\times d}$ is a symmetric, positive semi-definite matrix.

In general, $X$ can be written as a change-of-basis from a standard Gaussian $Z\sim \mathcal{N}(0,I_{r\times r})\in \mathbb{R}^{r}$, where $r=\text{rank}(\Sigma)\leq d$. The change-of-basis matrix is $A\in \mathbb{R}^{d\times r}$ so $X=\mu+AZ$. We can then check that the covariance of $X$ is $A A^T$, which is called the ==Cholesky decomposition== of $\Sigma$.

In the case that $r<d$, $X$ does not have a density function; instead, it is supported on a lower-dimensional subspace of $\mathbb{R}^{d}$.

Otherwise, $r=d$ and $\Sigma$ is full rank (and positive definite). Then, $A$ is invertible and $X$ has the familiar density
$$
f(x)=\frac{1}{(2\pi)^{d/2}\cdot|\text{det}\Sigma|^{1/2}}\cdot\exp \left( -\frac{1}{2}(x-\mu)^T\Sigma ^{-1}(x-\mu) \right) 
$$
over $x \in \mathbb{R}^{d}$.

[!claim] Claim (For Gaussians, uncorrelated $\Longleftrightarrow$ independent)
