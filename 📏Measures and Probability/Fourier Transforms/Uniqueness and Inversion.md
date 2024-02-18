> The Fourier inversion formula works if both $f$ and $\hat{f}$ are in $L^{1}(\mathbb{R}^{d})$.
## Statement

> [!theorem]
> Let $f\in L^{1}(\mathbb{R}^{d})$. Define for $t>0$ and $x \in \mathbb{R}^{d}$
> $$
> f_{t}(x)=\frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}} \hat{f}(u)e^{-|u|^{2}t/2}e^{-i\langle u,x \rangle } \, du. 
> $$
> Then, $\| f_{t}-f \|_{1}\to 0$ as $t\to 0$. Moreover, the Fourier inversion formula holds whenever $f\in L^{1}(\mathbb{R}^{d})$ and $\hat{f}\in L^{1}(\mathbb{R}^{d})$.

## Proof

Consider the Gaussian convolution $f*g_{t}$. Then, $(\widehat{f*g_{t}})(u)=\hat{f}(u)e^{-|u|^{2}t/2}$. In particular, $f_{t}=f*g_{t}$ since [[Gaussian Convolutions#^e65e4f|Fourier inversion works]] for Gaussian convolutions. Thus, $\| f_{t}-f \|_{1}\to 0$ as $t\to 0$ by our [[Gaussian Convolutions#^b21a79|approximation claim]]. 

In the case that $\hat{f}\in L^{1}(\mathbb{R}^{d})$, by the dominated convergence theorem with dominating function $|\hat{f}|$, we have that
$$
f_{t}(x)\to \frac{1}{(2\pi)^{d}}\int_{\mathbb{R}^{d}}\hat{f}(u)e^{-i\langle u,x \rangle } \, du  
$$
as $t\to 0$ for all $x$. On the other hand, $f_{t_{n}}\to f$ almost everywhere for some sequence $t_{n}\to 0$, so the inversion formula holds for $f$.

---

**Next:** [[Fourier Transform in L2]]

