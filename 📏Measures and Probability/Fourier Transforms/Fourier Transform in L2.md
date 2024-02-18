> The Fourier transform gives us a **Hilbert space automorphism** on $\mathcal{L}^{2}$. Some functions are not traditionally Fourier transformable, but we can approximate them with a sequence in $\mathcal{L}^{1}\cap \mathcal{L}^{2}$.

Now, we can define the leveled up Fourier transform, which works on the entirety of $\mathcal{L}^{2}$. Before, we could only take the Fourier transform inside of $L^{1}$, but we will patch this by taking limits. 

## Statement

> [!theorem]
> The Plancherel identity holds for all $f\in L^{1}\cap L^{2}(\mathbb{R}^{d})$. Moreover, there is a unique Hilbert space automorphism $F$ on $\mathcal{L}^{2}$ such that
> $$
> F[f]=[(2\pi)^{-d/2}\hat{f}]
> $$
> for all $f\in L^{1}\cap L^{2}(\mathbb{R}^{d})$.

We need to rescale down by this value because of the pesky $(2\pi)^{d}$ term that we got from the inversion formula. Further, this explains why there is a negative sign in the exponent of the inversion: $F$ is a unitary operator, and to get its inverse, we take the *conjugate* transpose.

## Proof

Suppose first that $f,\hat{f} \in L^{1}$. Then, the inversion formula holds and $f,\hat{f}\in L^{\infty}$ as well. Also, $(x,u)\mapsto f(x)\hat{f}(u)$ is integrable on $\mathbb{R}^{d}\times \mathbb{R}^{d}$, which means we can apply Fubini's theorem to get
$$
\begin{align*}
(2\pi)^{d}\| f \|_{2}^{2}
&= (2\pi)^{d}\int_{\mathbb{R}^{d}} f(x)\overline{f(x)} \, dx \\
&= \int_{\mathbb{R}^{d}} \left( \int_{\mathbb{R}^{d}} \hat{f}(u)e^{-i\langle u,x \rangle } \, du  \right) \overline{f(x)}  \, dx \\
&= \int_{\mathbb{R}^{d}} \hat{f}(u) \overline{\left( \int_{\mathbb{R}^{d}} f(x)e^{i\langle u,x \rangle } \, dx  \right) } \, du \\
&= \int_{\mathbb{R}^{d}} \hat{f}(u)\overline{\hat{f}(u)} \, du = \| \hat{f} \|_{2}^{2},
\end{align*}
$$
which confirms the Plancherel identity for $f$. However, this doesn't actually work directly since we don't know that $\hat{f}\in L^{1}$.

Take any $f\in L^{1}\cap L^{2}$ and consider for $t>0$ the Gaussian convolution $f_{t}=f*g_{t}$. We consider the limit $t\to 0$, in which case by a [[Gaussian Convolutions#^b21a79|previous claim]] we have that $f_{t}\to f$ in $L^{2}$, so $\| f_{t} \|_{2}\to \| f \|_{2}$. Further, we have that $\hat{f}_{t}=\hat{f}\hat{g}_{t}$ and $\hat{g}_{t}(u)=e^{-|u|^{2}t/2}$, so by monotone convergence, $\| \hat{f}_{t} \|_{2}^{2} \uparrow \| \hat{f} \|_{2}^{2}$. The Plancherel identity holds for $f_{t}$ because both it and $\hat{f}_{t}$ are in $L^{1}$. Now, letting $t\to 0$ gives the desired identity.

Finally, we need to show the existence of the operator $F$. First, define $F_{0}:\mathcal{L}^{1}\cap \mathcal{L}^{2}\to \mathcal{L}^{2}$ via $F_{0}[f]=[(2\pi)^{-d/2}f]$. Then, $F_{0}$ preserves the $L^{2}$-norm. Since $\mathcal{L}^{1}\cap \mathcal{L}^{2}$ is dense in $\mathcal{L}^{2}$, $F_{0}$ extends uniquely to an isometry $F$ of $\mathcal{L}^{2}$ onto itself. By the inversion formula, $F$ maps the set $V=\{ [f]:f,\hat{f} \in L^{1} \}$ into itself, and $F^{2}[f]=[f]$ for all $[f] \in V$. Yet $V$ contains all Gaussian convolutions and is therefore dense in $\mathcal{L}^{2}$, so $F$ must be onto $\mathcal{L}^{2}$.

---

**Next:** [[Weak Convergence and Characteristic Functions]]