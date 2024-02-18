> Our ultimate goal is to show the inversion formula and Plancherel work generally for functions. To do this, we first work with *Gaussians*, and then bootstrap to general functions $f$ by considering $f*g_{t}$ as $t\to 0$. 

In this section, we analyze convolutions $f*g_{t}$ of functions $f\in L^{1}(\mathbb{R}^{d})$ against Gaussians $g_{t}$ with $t\in(0,\infty)$.

## Basic Properties

Note that $f*g_{t}$ is a continuous function with
$$
\| f*g_{t} \|_{1}\leq \| f \|_{1},\quad \| f*g_{t} \|_{\infty}\leq(2\pi)^{-d/2}t^{-d/2}\| f \|_{1}. 
$$
Also $(\widehat{f*g_{t}})(u)=\hat{f}(u)\hat{g}_{t}(u)$ and we know $\hat{g}_{t}$ explicitly, so
$$
\| \widehat{f*g_{t}} \|_{1}\leq(2\pi)^{d/2}t^{-d/2}\| f \|_{1},\quad \| \widehat{f*g_{t}} \|_{\infty}\leq \| f \|_{1}.
$$
Further, $g_{s}*g_{s}=g_{2s}$ for $s \in(0,\infty)$ by simple calculation. Therefore, if $\mu$ is a probability measure on $\mathbb{R}^{d}$ and $t=2s \in(0,\infty)$, then $\mu*g_{s}\in L^{1}(\mathbb{R}^{d})$ by the above, and therefore $\mu*g_{t}=(\mu*g_{s})*g_{s}$, also a Gaussian convolution.
## Fourier Inversion

> [!claim]
> The Fourier inversion formula holds for all Gaussian convolutions $f*g_{t}$.

^e65e4f

> [!proof]-
> We use the [[Gaussian Fourier Transforms|Fourier inversion formula for the Gaussian]] $g_{t}$, as well as Fubini's theorem, to show that
> $$
> \begin{align*}
> (2\pi)^{d}(f*g_{t})(x)
> &= (2\pi)^{d}\int_{\mathbb{R}^{d}} f(x-y)g_{t}(y) \, dy \\
> &= \int_{\mathbb{R}^{d}\times \mathbb{R}^{d}} f(x-y)\hat{g}_{t}(u)e^{-i\langle u,y \rangle} \, du dy \\
> &= \int_{\mathbb{R}^{d}\times \mathbb{R}^{d}} f(x-y)e^{i\langle u,x-y \rangle }\hat{g}_{t}(u)e^{-i\langle u,x \rangle } \, du dy \\
> &= \int_{\mathbb{R}^{d}}\hat{f}(u)\hat{g}_{t}(u)e^{-i\langle u,x \rangle } \, du \\
> &= \int_{\mathbb{R}^{d}}(\widehat{f*g_{t}})(u)e^{-i\langle u,x \rangle } \, du,
> \end{align*}
> $$
> as desired.

## Function Approximation

You should think of $f*g_{t}$ as a "smoothed out" version of $f$: for each point in $f$, we average a small kernel around it to get the value in the convolution. We should expect as $t\to 0$ that this approaches the original function $f$.

> [!claim]
> Let $f\in L^{p}(\mathbb{R}^{d})$ with $p \in[0,\infty)$. Then, $\| f*g_{t}-f \|_{p}\to 0$ as $t\to 0$.

^b21a79

> [!proof]-
> Given $\varepsilon>0$, there exists a continuous function $h$ of compact support such that $\| f-h \|_{p}\leq\varepsilon/3$. Then, $\| f*g_{t}-h*g_{t} \|\leq \| f-h \|_{p}\leq\varepsilon/3$. Set
> $$
> e(y)=\int_{\mathbb{R}^{d}}|h(x-y)-h(x)|^{p} \, dx.
> $$
> Then, $e(y)\leq 2^{p}\| h \|_{p}^{p}$ for all $y$ and $e(y)\to 0$ as $y\to 0$ by dominated convergence. By Jensen's inequality and [[Bounded Convergence|bounded convergence]],
> $$
> \begin{align*}
> \| h*g_{t} - h \|_{p}^{p}
> &= \int _{\mathbb{R}^{d}}\left| \int _{\mathbb{R}^{d}} (h(x-y)-h(x))g_{t}(y) \, dy  \right|^{p}  \, dx \\
> &\leq \int_{\mathbb{R}^{d}}\int_{\mathbb{R}^{d}} |h(x-y)-h(x)|^{p} g_{t}(y) \, dy  \, dx \\
> &= \int_{\mathbb{R}^{d}} e(y)g_{t}(y) \, dy \\
> &= \int_{\mathbb{R}^{d}} e(\sqrt{ t }y)g_{1}(y) \, dy, 
> \end{align*}
> $$
> which goes to $0$ as $t\to 0$. Now,
> $$
> \| f*g_{t} - f \|_{p}\leq
> \| f*g_{t} - h*g_{t} \|_{p}+
> \| h*g_{t} - h \|_{p}+
> \| h-f \|_{p}, 
> $$
> which also becomes $<\varepsilon$ for all sufficiently small $t>0$, as desired.

---

**Next:** [[Uniqueness and Inversion]]