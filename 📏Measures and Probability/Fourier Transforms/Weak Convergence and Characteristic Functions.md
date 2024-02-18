In this section, we prove the following two facts: the distribution of a random vector is determined by its characteristic function, and pointwise convergence of characteristic functions implies weak convergence.

> [!definition] Definition (Weak convergence)
> Let $\mu$ be a Borel probability measure on $\mathbb{R}^{d}$ and let $(\mu_{n}:n\in \mathbb{N})$ be a sequence of such measures. We say that $\mu_{n}$ ==converges weakly== to $\mu$ if $\mu_{n}(f)\to \mu(f)$ as $n\to \infty$ for all continuous bounded functions $f$ on $\mathbb{R}^{d}$.
> 
> For random variables, we say that $X_{n}$ ==converges weakly== to $X$ if $\mu_{X_{n}}$ converges weakly to $\mu_{X}$. They don't need to be defined on a common probability space.

> [!theorem]
> Let $X$ be a random variable in $\mathbb{R}^{d}$. Then the distribution $\mu_{X}$ of $X$ is uniquely determined by its characteristic function $\phi_{X}$. Moreover, in the case where $\phi_{X}$ is integrable, $\mu_{X}$ has a continuous bounded density function given by
> $$
> f_{X}(x)=\frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}}\phi_{X}(u)e^{-i\langle u,x \rangle } \, du. 
> $$
> Moreover, if $(X_{n}:n\in \mathbb{N})$ is a sequence of random variables in $\mathbb{R}^{d}$ such that $\phi_{X_{n}}(u)\to \phi_{X}(u)$ as $n\to \infty$ for all $u\in \mathbb{R}^{d}$, then $X_{n}$ converges weakly to $X$.

> [!proof]-
> Let $Z$ be a random variable in $\mathbb{R}^{d}$, independent of $X$, and having the standard Gaussian density $g_{1}$. Then, $\sqrt{ t }Z$ has density $g_{t}$, and $X+\sqrt{ t }Z$ has density given by the Gaussian convolution $f_{t}=\mu_{X}*g_{t}$. Therefore, $\hat{f}_{t}(u)=\phi_{X}(u)e^{-|u|^{2}t/2}$, so by the Fourier inversion formula,
> $$
> f_{t}(x)=\frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}}\phi_{X}(u)e^{-|u|^{2}t/2}e^{-i\langle u,x \rangle } \, du. 
> $$
> By bounded convergence, for all continuous bounded functions $g$ on $\mathbb{R}^{d}$, as $t\to 0$ we have
> $$
> \int_{\mathbb{R}^{d}}g(x)f_{t}(x) \, dx=\mathbb{E}[g(X+\sqrt{ t }Z)]\to \mathbb{E}[g(X)]=\int_{\mathbb{R}^{d}}g(x)\mu_{X}(dx) \, dx.   
> $$
> Hence $\phi_{X}$ determines $\mu_{X}$ uniquely.
> 
> In the case that $\phi_{X}$ is integrable, we have that $|f_{t}(x)|\leq(2\pi)^{-d}\| \phi_{X} \|_{1}$ for all $x$ and by dominated convergence with dominating function $|\phi_{X}|$, we have $f_{t}(x)\to f_{X}(x)$ for all $x$. Hence $f_{X}(x)\geq 0$ for all $x$ and, for $g$ continuous of compact support, by bounded convergence,
> $$
> \int_{\mathbb{R}^{d}}g(x)\, \mu_{X}(dx)
> =\lim_{ t \to 0 } \int_{\mathbb{R}^{d}}g(x)f_{t}(x) \, dx
> 	=\int_{\mathbb{R}^{d}}g(x)f_{X}(x) \, dx,
> $$
> which implies that $\mu_{X}$ has density $f_{X}$, as claimed.
> 
> Suppose now that $(X_{n}:n\in \mathbb{N})$ is a sequence of random variables such that $\phi_{X_{n}}(u)\to \phi_{X}(u)$ for all $u$. We shall show that $\mathbb{E}[g(X_{n})]\to \mathbb{E}[g(X)]$ for all integrable functions $g$ on $\mathbb{R}^{d}$ whose derivative is bounded, which implies that $X_{n}$ converges weakly to $X$. We approximate using Gaussian convolutions.
> 
> Given $\varepsilon>0$, we can choose $t>0$ so that $\sqrt{ t }\| \nabla g \|_{\infty}\mathbb{E}[Z]\leq\varepsilon/3$. Then $\mathbb{E}|g(X+\sqrt{ t }Z)-g(X)|\leq\varepsilon/3$ and $\mathbb{E}|g(X_{n}+\sqrt{ t }Z)-g(X_{n})|\leq\varepsilon/3$. On the other hand, by the Fourier inversion formula, and dominated convergence with dominating function $|g(x)|e^{-|u|^{2}t/2}$, we have
> $$
> \begin{align*}
> \mathbb{E}[g(X_{n}+\sqrt{ t }Z)]
> &=\frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}\times \mathbb{R}^{d}}g(x)\phi_{X_{n}}(u)e^{-|u|t^{2}/2}e^{-i\langle x,u \rangle } \, dudx\\
> &\to \frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}\times \mathbb{R}^{d}}g(x)\phi_{X}(u)e^{-|u|t^{2}/2}e^{-i\langle x,u \rangle } \, dudx\\
> &=\mathbb{E}[g(X+\sqrt{ t }Z)].
> \end{align*}
> $$
> Hence $|\mathbb{E}[g(X_{n})]-\mathbb{E}[g(X)]|<\varepsilon$ for all sufficiently large $n$, as required.

There is a stronger version of the last assertion called ==Lévy's continuity theorem== for characteristic functions: if $\phi_{X_{n}}(u)$ converges as $n\to \infty$ with limit $\phi(u)$, say, for all $u\in \mathbb{R}$, and if $\phi$ is continuous in a neighborhood of $0$, then $\phi$ is the characteristic function of some random variable $X$, and $X_{n}\to X$ in distribution.

---

**Next:** [[⛺Fourier Transforms Homepage]]