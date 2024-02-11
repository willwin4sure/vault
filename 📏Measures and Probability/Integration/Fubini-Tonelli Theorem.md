You've seen this in your multivariable calculus class. We want to be able to swap order of summations, and this theorem tells us when that is possible.

We do this by giving conditions for when a double integral can be expressed as an iterated integral.
## Statement

Use the notation of the last section on [[Product Measure|product measures]]. We are working over $\mathcal{E}=\mathcal{E}_{1}\otimes \mathcal{E}_{2}$, equipped with the product measure $\mu$.

> [!theorem] Theorem (Fubini-Tonelli Theorem)
> 1. Let $f$ be a nonnegative $\mathcal{E}$-measurable function. Then
> $$
> \mu(f)=\int _{E_{1}}\left( \int _{E_{2}} f(x_{1},x_{2}) \, \mu_{2}(dx_{2})  \right)  \, \mu_{1}(dx_{1}). 
> $$
> 2. Let $f$ be a $\mu$-integrable function. Define
> $$
> A_{1}=\left\{  x_{1}\in E_{1} : \int_{E_{2}} |f(x_{1},x_{2})| \, \mu_{2}(dx_{2})<\infty   \right\}
> $$
> and define $f_{1}:E_{1}\to \mathbb{R}$ by
> $$
> f_{1}(x_{1})=\int _{E_{2}}f(x_{1},x_{2}) \, \mu_{2}(dx_{2}) 
> $$
> for $x_{1}\in A_{1}$ and $f_{1}(x_{1})=0$ otherwise. Then $\mu_{1}(E_{1}\setminus A_{1})=0$ and $f_{1}$ is $\mu_{1}$-integrable with $\mu_{1}(f_{1})=\mu(f)$.

## Proof Sketch

For the first statement, first prove for indicator functions on measurable sets, then simple functions, then arbitrary nonnegative measurable functions, using linearity and monotone convergence.

For the second statement, note that $\mu_{1}(E_{1}\setminus A_{1})=0$ else $\mu(f)$ would blow up, using the first formula. The integral identity holds by using the first formula, as well.

## Extension

The existence of the product measure and Fubini's theorem extend to $\sigma$-finite measure space as well. The measure obtained by taking the $n$-fold product of the Lebesgue measure on $\mathbb{R}$ is called the ==Lebesgue measure on $\mathbb{R}^{n}$==, and the corresponding integral is written
$$
\int _{\mathbb{R}^{n}} f(x) \, dx.
$$

---

**Next:** [[Laws of Independent Random Variables]]