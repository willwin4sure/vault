> The Fourier transform turns convolutions into multiplication. 

Convolutions show up everywhere: audio kernels, convolutional neural networks, and more. You might have even seen the discrete Dirichlet convolution appear in olympiad number theory.

## Definition

> [!definition] Definition (Convolve a function against a measure)
> For $p \in [0,\infty)$ and $f\in L^{p}(\mathbb{R}^{d})$ and for a probability measure $\nu$ on $\mathbb{R}^{d}$, we define the ==convolution==
> $$
> (f * \nu)(x)=\int_{\mathbb{R}^{d}} f(x-y) \, \nu(dy)
> $$
> whenever the integral exists, setting $(f * \nu)(x)=0$ otherwise. 

By Jensen's inequality and Fubini's theorem,
$$
\begin{align*}
	\int_{\mathbb{R}^{d}}\left( \int_{\mathbb{R}^{d}} |f(x-y)|\nu(dy) \right)^{p} \, dx 
	&\leq \int_{\mathbb{R}^{d}}\int_{\mathbb{R}^{d}}|f(x-y)|^{p} \, \nu(dy)  \, dx \\
	&\leq \int_{\mathbb{R}^{d}}\int_{\mathbb{R}^{d}}|f(x-y)|^{p} \, dx  \, \nu(dy) \\
	&=\int _{\mathbb{R}^{d}}\int _{\mathbb{R}^{d}}|f(x)|^{p} \, dx  \, \nu(dy)=\| f \|_{p}^{p} < \infty. 
\end{align*}
$$
Therefore, the integral defining the convolution exists for almost all $x$, and also
$$
\| f * \nu \|_{p}\leq \| f \|_{p}. 
$$
In the case where $\nu$ has density function $g$, we can also write $f*g=f*\nu$.

> [!definition] Definition (Convolve two measures)
> If $\mu$ and $\nu$ are both probability measures on $\mathbb{R}^{d}$, we define the ==convolution== $\mu*\nu$ to be the distribution of $X+Y$ for independent random variables $X$ and $Y$ with respective distributions $\mu$ and $\nu$. In particular,
> $$
> (\mu * \nu)(A)= \int_{\mathbb{R}^{d}\times \mathbb{R}^{d}} \mathbf{1}_{A}(x+y) \, \mu(dx)\nu(dy), 
> $$
> for $A\in \mathcal{B}$. In particular, if $\mu$ has density function $f$, then $\mu*\nu$ has density function $f*\nu$.

> [!idea]
> As promised, we have that $\widehat{f*\nu}(u)=\hat{f}(u)\hat{\nu}(u)$ for all $f\in L^{1}(\mathbb{R}^{d})$ and all probability measures $\nu$ on $\mathbb{R}^{d}$.

Similarly, we can check that
$$
\widehat{\mu*\nu}=\mathbb{E}[e^{i\langle u,X+Y \rangle }]=\mathbb{E}[e^{i\langle u,X \rangle }]\mathbb{E}[e^{i\langle u,Y \rangle }]=\hat{\mu}(u)\hat{\nu}(u).
$$
---

**Next:** [[Gaussian Fourier Transforms]]