The goal of this section is to define the *integral* for nonnegative measurable functions.

## Definition

Work over a measure space $(E,\mathcal{E},\mu)$. We will define the ==integral== of $f:E\to \mathbb{R}_{\geq 0}$, denoted in one of three ways:
$$
\mu(f)=\int_{E} f \, d\mu =\int_{E} f(x) \, \mu(dx). 
$$
When $(E,\mathcal{E})=(\mathbb{R},\mathcal{B}(\mathbb{R}))$ and $\mu$ is the Lebesgue measure, we can also use the familiar notation
$$
\mu(f)=\int_{\mathbb{R}} f(x) \, dx.
$$
For a random variable $X$ on a probability space $(\Omega,\mathcal{F},\mathbb{P})$, the integral is called the ==expectation== of $X$, denoted $\mathbb{E}[X]$.

### Simple Functions

So far, the only type of function we can reasonably integrate at the moment are indicator functions on measurable sets. This should agree with the value of the measure of the measurable set.

> [!definition] Definition (Integrals of simple functions)
> A ==simple function== is one of the form
> $$
> f = \sum_{k=1}^{m} a_{k} \cdot \mathbf{1}_{A_{k}},
> $$
> where $a_{k}\in[0,\infty)$ and $A_{k}\in \mathcal{E}$. For such functions, we define its integral as
> $$
> \mu(f)=\sum_{k=1}^{m}a_{k}\cdot \mu(A_{k}),
> $$
> where we take $0\cdot \infty=0$.

Despite the representation of $f$ not being unique, this definition is consistent.

### Measurable Functions

Now, we will extend integrals to arbitrary measurable functions $f$. First, the case where $f$ is nonnegative. We do this by considering all simple functions $g$ that can fit underneath $f$, and then taking the supremum of their integrals.

> [!definition] Definition (Integrals of nonnegative functions)
> For a nonnegative measurable function $f$, we define its integral as
> $$
> \mu(f)=\sup \{ \mu(g) : g\text{ simple},\ g\leq f \}.
> $$

How do we extend this to arbitrary $\mathbb{R}$-valued measurable functions? Well, decompose the function into its positive and negative components.

[!definition] Definition (Integrals of measurable functions)
For a measurable function $f$, 