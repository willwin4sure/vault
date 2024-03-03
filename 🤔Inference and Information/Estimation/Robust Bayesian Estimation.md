This page contains some alternate loss functions. The quadratic cost function for BLS is computationally attractive, but it is quite sensitive to outliers. Ideally, we do some mix with an absolute error cost function.

> [!definition] (Huber loss)
> Define the ==Huber loss== as
> $$
> C_{\delta}(x,\hat{x})=
> \begin{cases}
> (x-\hat{x})^{2}/2 & |x-\hat{x}|\leq\delta \\
> \delta(|x-\hat{x}|-\delta/2) & |x-\hat{x}|>\delta
> \end{cases}
> $$
> which is continuously differentiable in $|x-\hat{x}|$.

You basically take the parabola, and then at some point extend in a straight line instead.

> [!definition] (Pseudo-Huber loss)
> The ==pseudo-Huber loss== is an even smoother variant
> $$
> C_{\delta}'(x,\hat{x})=\delta^{2}\left( \sqrt{ 1+ \frac{(x-\hat{x})^{2}}{\delta^{2}} } -1 \right),
> $$
> which matches the behavior of $\frac{(x-\hat{x})^{2}}{2}$ as $|x-\hat{x}| \to 0$ and the behavior of $\delta(x-\hat{x})$ as $|x-\hat{x}| \to \infty$.

Here's yet another cost function with similar behavior:

> [!definition] (Log-cosh loss)
> The loss
> $$
> C''(x,\hat{x})=\log(\cosh(x-\hat{x}))
> $$
> is twice-differentiable everywhere and matches the behavior of $\frac{(x-\hat{x})^{2}}{2}$ as $|x-\hat{x}| \to 0$ and the behavior of $|x-\hat{x}|-\log 2$ as $|x-\hat{x}| \to \infty$.

---

**Next:** [[Bayesian Parameter Estimation]]