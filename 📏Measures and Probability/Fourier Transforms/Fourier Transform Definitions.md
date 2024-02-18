For the rest of our discussion of Fourier transforms, we write $L^{p}=L^{p}(\mathbb{R}^{d})$ for the set of *complex-valued* Borel measurable functions on $\mathbb{R}^{d}$ such that
$$
\| f \|_{p}=\left( \int_{\mathbb{R}^{d}}|f(x)|^{p} \, dx  \right)^{1/p}<\infty. 
$$
## The Fourier Transform

This is the basic definition of a Fourier transform. 

> [!definition] Definition (Fourier transform)
> The ==Fourier transform== $\hat{f}$ of a function $f\in L^{1}(\mathbb{R}^{d})$ is defined by
> $$
> \hat{f}(u)=\int_{\mathbb{R}^{d}}f(x)e^{i\langle u,x \rangle } \, dx,\quad u\in \mathbb{R}^{d}.
> $$
> Here, $\langle \bullet,\bullet \rangle$ denotes the usual inner product on $\mathbb{R}^{d}$.

Note that $|\hat{f}(u)|\leq \| f \|_{1}$ and, by the dominated convergence theorem, $\hat{f}(u_{n})\to \hat{f}(u)$ whenever $u_{n}\to u$. Thus $\hat{f}$ is a continuous function in $L^{\infty}(\mathbb{R}^{d})$.

## Identities

Not always is $\hat{f}\in L^{1}(\mathbb{R}^{d})$, but when it also is, we say the ==Fourier inversion formula== holds for $f$ if
$$
f(x)=\frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}}\hat{f}(u)e^{-i\langle u,x \rangle } \, du 
$$
for almost all $x \in \mathbb{R}^{d}$ (Fourier transform back, up to scaling).

When $f\in L^{1}\cap L^{2}(\mathbb{R}^{d})$, we say that the ==Plancherel identity== holds for $f$ if
$$
\| f \|_{2} = (2\pi)^{d/2}\| f \|_{2.}
$$
The main results that we will show are that for all $f\in L^{1}(\mathbb{R}^{d})$, the inversion formula holds whenever $\hat{f}\in L^{1}(\mathbb{R}^{d})$, and that the Plancherel identity holds whenever $f\in L^{2}(\mathbb{R}^{d})$. 

> [!idea]
> Under suitable rescaling of the Fourier transform, this gives a **unitary transformation** of $\mathcal{L}^{2}$! 

## Characteristic Functions

> [!definition] Definition (Fourier transform of a measure)
> The ==Fourier transform== $\hat{\mu}$ of a finite Borel measure $\mu$ on $\mathbb{R}^{d}$ is defined by
> $$
> \hat{\mu}(u)=\int_{\mathbb{R}^{d}}e^{i\langle u,x \rangle } \, \mu(dx),\quad u\in \mathbb{R}^{d}. 
> $$
> Then, $\hat{\mu}$ is a continuous function on $\mathbb{R}^{d}$ with $|\hat{\mu}(u)|\leq \mu(\mathbb{R}^{d})$ for all $u$. 

These definitions are consistent in that, if $\mu$ has density $f$ with respect to the Lebesgue measure, then $\hat{\mu}=\hat{f}$.

> [!definition] Definition (Characteristic function)
> The ==characteristic function== $\phi_{X}$ of a random variable $X$ in $\mathbb{R}^{d}$ is the Fourier transform of its law $\mu_{X}$. Thus,
> $$
> \phi_{X}(u)=\hat{\mu}_{X}(u)=\mathbb{E}[e^{i\langle u,X \rangle }],\quad u\in \mathbb{R}^{d}.
> $$

---

**Next:** [[Convolutions]]
