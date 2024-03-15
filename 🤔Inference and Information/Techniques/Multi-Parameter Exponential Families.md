Now, we will extend our ideas to multiple parameters. 

## Definitions

> [!definition] ($K$-parameter exponential family)
> A parameterized family of distributions $p(\bullet;\mathbf{x})$ is a $K$-parameter exponential family with ==natural parameter== $\boldsymbol{\lambda}(\bullet)=\begin{bmatrix}\lambda_{1}(\bullet) & \cdots & \lambda_{K}(\bullet)\end{bmatrix}:\mathcal{X}\to \mathbb{R}^{K}$, ==natural statistic== $\mathbf{t}(\bullet)=\begin{bmatrix}t_{1}(\bullet) & \cdots & t_{K}(\bullet)\end{bmatrix}:\mathcal{Y}\to \mathbb{R}^{K}$, and ==log-base function== $\beta(\bullet):\mathcal{Y}\to \mathbb{R}$ if each member of the family is of the form
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})=\exp \left\{ \boldsymbol{\lambda}(\mathbf{x)})^{T}\mathbf{t}(\mathbf{y})-\alpha(\mathbf{x})+\beta(\mathbf{y}) \right\}.
> $$

> [!example] (Gaussian parameterized by mean and variance)
> Let $\mathsf{y}$ be a scalar Gaussian with mean $x_{1}$ and variance $x_{2}$. Then,
> $$
> \ln p_{\mathsf{y}}(y;\mathbf{x})=-\frac{1}{2}\ln(2\pi x_{2})-\frac{(y-x_{1})^{2}}{2x_{2}}=\frac{x_{1}}{x_{2}}y-\frac{1}{2x_{2}}y^{2}-\frac{x_{1}^{2}}{2x_{2}}-\frac{1}{2}\ln(2\pi x_{2}).
> $$
> This corresponds to a $2$-parameter exponential family with
> $$
> \boldsymbol{\lambda}(\mathbf{x})=\begin{bmatrix}
> x_{1}/x_{2} \\
> -1/2x_{2}
> \end{bmatrix},\quad
> \mathbf{t}(y)=\begin{bmatrix}
> y \\
> y^{2}
> \end{bmatrix},\quad
> \beta(y)=0,\quad
> \alpha(\mathbf{x})=\frac{x_{1}^{2}}{2x_{2}}+\frac{1}{2}\ln(2\pi x_{2}).
> $$

We use $\mathcal{E}(\mathcal{X},\mathcal{Y};\boldsymbol{\lambda}(\bullet),\mathbf{t}(\bullet),\beta(\bullet))$ to represent a multi-parameter exponential family. Note that the equivalence classes are rather large. For example, when we are in the finite setting $\mathcal{Y}=\{ 1,2,\dots,M \}$, we can characterize our entire probability distribution via
$$
\begin{bmatrix}
\ln p_{\mathsf{y}}(1;\mathbf{x}) \\
\vdots \\
\ln p_{\mathsf{y}}(M;\mathbf{x})
\end{bmatrix}
=
\underbrace{
\begin{bmatrix}
t_{1}(1) & \cdots & t_{K}(1) \\
\vdots & \ddots & \vdots \\
t_{1}(M) & \cdots & t_{K}(M)
\end{bmatrix}
}_{\mathbf{T}}
\begin{bmatrix}
\lambda_{1}(\mathbf{x}) \\
\vdots \\
\lambda_{K}(\mathbf{x})
\end{bmatrix}
-\alpha(\mathbf{x})
\begin{bmatrix}
1 \\
\vdots \\
1
\end{bmatrix}
+
\begin{bmatrix}
\beta(1) \\
\vdots \\
\beta(M)
\end{bmatrix}.
$$
Now, note that if $\mathbf{T}$ is not full rank, then we can express the whole breadth of the distribution using fewer parameters $K$. However, if $\text{rank}(\mathbf{T})=K$ exactly, then we refer to the representation as ==minimal==. 

The relationship between the actual underlying parameter $\mathbf{x} \in \mathcal{X}$ and the natural parameter $\boldsymbol{\lambda}(x)\in \mathbb{R}^{K}$ is also of interest. If $\mathcal{X}\subseteq \mathbb{R}^{L}$, then there are three coarse classes of behavior:

1. $\text{rank}(\partial \boldsymbol{\lambda}(\mathbf{x})/\partial \mathbf{x})<L$, in which case the family is ==overparameterized== and the mapping $\boldsymbol{\lambda}(\bullet)$ is not invertible. A key example when this occurs is if $L>K$. These cases *should not occur* in well-formulated inference problems. One simple example is if $\lambda(\mathbf{x})=x_{1}+x_{2}$.

2.  $\text{rank}(\partial \boldsymbol{\lambda}(\mathbf{x})/\partial \mathbf{x})=L=K$, in which case the family is ==full==. For example, $\boldsymbol{\lambda}(\mathbf{x})=\mathbf{x}$ or $\boldsymbol{\lambda}(\mathbf{x})=\begin{bmatrix}x_{1}+x_{2}^{2}\\ x_{1}^{2}+x_{2}\end{bmatrix}$.

3. $\text{rank}(\partial \boldsymbol{\lambda}(\mathbf{x})/\partial \mathbf{x})=L<K$, in which case the family is ==curved==. For example, $\boldsymbol{\lambda}(x)=\begin{bmatrix}x & x^{2}\end{bmatrix}^{T}$.

> [!idea]
> Case 1 is when $\mathcal{X}$ gets stuffed into a tight space, overlapping itself. Case 2 is when $\mathcal{X}$ gets mapped cleanly into the natural parameter space of the same dimension. Case 3 is when $\mathcal{X}$ gets mapped cleanly into a lower-dimensional submanifold of the full natural parameter space.

> [!example]
> Let $\mathcal{Y}=\mathbb{R}$ and consider the family of distributions $p_{\mathsf{y}}(\bullet;x)=\mathtt{N}(x,x^{2})$ for $x \in \mathcal{X}=(0,\infty)$. Then,
> $$
> \ln p_{\mathsf{y}}(y;x)=-\ln (x\sqrt{ 2\pi })-\frac{1}{2}\left( \frac{y-x}{x} \right)^{2}, 
> $$
> so this is a curved $2$-parameter exponential family with
> $$
> \boldsymbol{\lambda}(x)=
> \begin{bmatrix}
> -1/(2x^{2}) \\
> 1/x
> \end{bmatrix},\quad
> \mathbf{t}(y)=
> \begin{bmatrix}
> y^{2} \\
> y
> \end{bmatrix},\quad
> \beta(y)=1.
> $$

> [!definition] (Canonical exponential family)
> This is when $\boldsymbol{\lambda}(\mathbf{x})=\mathbf{x}$.

## Properties that Carry Over

The derivatives of the log-partition function [[Single-Parameter Exponential Families#^42919e|still]] extract moments:
$$
\begin{align*}
\frac{ \partial \alpha(\mathbf{x}) }{ \partial \mathbf{x} } &= \mathbb{E}[\mathbf{t}(\boldsymbol{\mathsf{y}})]^{T} \\
J_{\boldsymbol{\mathsf{y}}}(\mathbf{x})=\frac{ \partial^{2}\alpha(\mathbf{x}) }{ \partial \mathbf{x} } &= \text{Cov}(\mathbf{t}(\boldsymbol{\mathsf{y}})).
\end{align*}
$$
Also, members of a $K$ parameter exponential family are [[Single-Parameter Exponential Families#^617004|still]] characterized by the weighted geometric mean of $K+1$ probability distributions.

There is also [[Efficient Estimators and Exponential Families|still]] a connection to efficient estimators and the MLE, but this is omitted (for now).

---

**Next:** [[Markov Structure as an Exponential Family]]
