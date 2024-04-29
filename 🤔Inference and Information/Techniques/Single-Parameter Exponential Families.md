> Pretty much every named, parameterized distribution you know (other than uniform) is an exponential family.

An exponential family is a parameterized family of distributions. To build our intuition, we start with the simplest case of a single scalar parameter $x$.
## Definitions

> [!definition] (Single-parameter exponential family)
> A parameterized family of distributions $\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;x) : x \in \mathcal{X} \}$ over the alphabet $\mathcal{Y}$ is a ==one-parameter exponential family== if it can be expressed in the form
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\exp \left\{ \lambda(x)t(\mathbf{y})-\alpha(x)+\beta(\mathbf{y}) \right\} 
> $$
> for all $x \in \mathcal{X}$ and $y \in \mathcal{Y}$, for some choice of functions $\lambda$, $t$, and $\beta$.

We have some names for these special functions:

* $\lambda(\bullet):\mathcal{X}\to \mathbb{R}$ is called the ==natural parameter==. It converts $x$ into the parameter actually "used" by the distribution.
* $t(\bullet):\mathcal{Y}\to \mathbb{R}$ is called the ==natural statistic==. It is the only part of the data that the parameters care about with regards to the value of the distribution. ^2a98de
* $\alpha(\bullet):\mathcal{X}\to \mathbb{R}$ is called the ==log-partition function==. It normalizes the distribution; if we set $Z(x)=e^{\alpha(x)}$, it is called the **partition function** and can be expressed as
$$
Z(x)=\int \exp \{ \lambda(x)t(\mathbf{y})+\beta(\mathbf{y}) \} \, \nu(d\mathbf{y}).
$$
* $\beta(\bullet):\mathcal{Y}\to \mathbb{R}$ is called the ==log-base function==. (If it exists,) the normalized $q(\mathbf{y})=e^{\beta(\mathbf{y})-c}$ is referred to as the **base distribution**. 

Note that $\mathcal{X}$ can only include values of $x$ where the normalization by $Z(x)$ can occur. 

> [!notation]
> We write
> $$
> p \in \mathcal{E}(\mathcal{X},\mathcal{Y}; \lambda(\bullet), t(\bullet), \beta(\bullet))
> $$
> to indicate that $p$ is drawn from the above exponential family. Note that this specification is not unique; both $t$ and $\beta$ can be shifted by a constant, and it will get absorbed into $\alpha$.

> [!definition] (Regular exponential families)
> In general, the support of a member distribution is allowed to depend on $x$ as well. However, we will restrict our attention to ==regular== exponential families, where this is not allowed to occur.

Accordingly, uniform distributions $\mathtt{U}([0,x])$ will not be considered a part of exponential families, as their support depends on $x$.

> [!claim] (Log-partition function properties)
> Derivatives of the log-partition function extract moments of the natural statistic. In particular,
> $$
> \dot{\alpha}(x)=\dot{\lambda}(x)\cdot\mathbb{E}\left[ t(\boldsymbol{\mathsf{y}}) \right],\quad
> \ddot{\alpha}(x)=\ddot{\lambda}(x)\cdot\mathbb{E}[t(\boldsymbol{\mathsf{y}})]+\dot{\lambda}(x)^{2}\cdot\text{Var}[t(\boldsymbol{\mathsf{y}})].
> $$
> Further, the Fisher information can be expressed as
> $$
> J_{\boldsymbol{\mathsf{y}}}=\dot{\lambda}(x) \frac{d}{dx}\mathbb{E}[t(\boldsymbol{\mathsf{y}})].
> $$

^42919e

> [!proof] Omitted, computation

## Basic Examples

Almost all named distributions you know are exponential families. Here are some basic examples:

> [!example] (Bernoulli distribution)
> Parameterized by a success probability $x$, we can write $p_{\mathsf{y}}(y;x)=x^{\mathbf{1}_{y=1}}(1-x)^{\mathbf{1}_{y=0}}$, so the log probability is
> $$
> \ln p_{\mathsf{y}}(y;x)=y\ln \left( \frac{x}{1-x} \right) +\ln(1-x).
> $$
> As an exponential family, we have that
> $$
> \lambda(x)=\ln \left( \frac{x}{1-x} \right),\quad
> t(y)=y,\quad
> \alpha(x)=-\ln(1-x),\quad
> \beta(y)=0.
> $$
> This family is regular so long as $x \in(0,1)$.

> [!example] (Binomial distribution)
> Given a constant $N$ number of flips, this is also parameterized by a success probability $x$, where $p_{\mathsf{y}}(y;x)=\binom{N}{y}x^{y}(1-x)^{N-y}$, so the log probability is
> $$
> \ln p_{\mathsf{y}}(y;x)=y\ln \left( \frac{x}{1-x} \right) +N\ln(1-x)+\ln \binom{N}{y}.
> $$
> As an exponential family, we have that
> $$
> \lambda(x)=\ln \left( \frac{x}{1-x} \right),\quad
> t(y)=y,\quad
> \alpha(x)=-N\ln(1-x),\quad
> \beta(y)=\ln\binom{N}{y}.
> $$

> [!example] (Gaussian distribution)
> Consider a Gaussian with fixed variance $1$ parameterized by its mean $x$. Then,
> $$
> \ln p_{\mathsf{y}}(y;x)=-\frac{1}{2}\ln(2\pi)-\frac{1}{2}(y-x)^{2}.
> $$
> This can be expanded to give us that
> $$
> \lambda(x)=x,\quad
> t(y)=y,\quad
> \alpha(x)=\frac{x^{2}}{2}+\frac{1}{2}\ln(2\pi),\quad
> \beta(y)=-\frac{y^{2}}{2}.
> $$

> [!example] (Exponential distribution)
> Consider an exponential random variable parameterized by its rate $x$. Then,
> $$
> \ln p_{\mathsf{y}}(y;x)=\ln x-xy,
> $$
> which gives us that
> $$
> \lambda(x)=x,\quad
> t(y)=-y,\quad
> \alpha(x)=-\ln x,\quad
> \beta(y)=0.
> $$

## Canonical Exponential Families

> [!definition] (Canonical exponential family)
> A ==canonical exponential family== is an exponential family whose natural parameter is equal to the underlying parameter, i.e. $\lambda(x)=x$, so
> $$
> \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=xt(\mathbf{y})-\alpha(x)+\beta(\mathbf{y}).
> $$

The ==natural parameter space== consists of all possible $x$ such that the distribution can be normalized, i.e.
$$
\mathcal{X}=\{ x \in \mathbb{R} : \alpha(x)<\infty \}.
$$
In this case, the [[Single-Parameter Exponential Families#^42919e|claim above]] specializes to $\dot{\alpha}(x)=\mathbb{E}[t(\boldsymbol{\mathsf{y}})]$ and $\ddot{\alpha}(x)=\text{Var}[t(\boldsymbol{\mathsf{y}})]=J_{\boldsymbol{\mathsf{y}}}(x)$. Further, the partition function $Z(x)$ can be interpreted as the moment-generating function corresponding to the base distribution $q(\bullet)$.

## Other Constructions

### Geometric Means of Distributions

> [!definition]
> Consider two strictly positive distributions $p_{1}(\bullet)$ and $p_{2}(\bullet)$ and interpolate between them via their ==weighted geometric mean==
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\frac{p_{1}(\mathbf{y})^{x}p_{2}(\mathbf{y})^{1-x}}{Z(x)},
> $$
> parameterized by $x \in \mathcal{X}=[0,1]$.

Then,
$$
\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=x\ln \frac{p_{1}(\mathbf{y})}{p_{2}(\mathbf{y})}+\ln p_{2}(\mathbf{y})-\ln Z(x),
$$
so it defines a canonical exponential family with
$$
\lambda(x)=x,\quad
t(\mathbf{y})=\ln \frac{p_{1}(\mathbf{y})}{p_{2}(\mathbf{y})},\quad
\alpha(x)=\ln Z(x),\quad
\beta(y)=\ln p_{2}(\mathbf{y}).
$$
> [!theorem]
> Let $\mathcal{P}=\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;x) : x \in \mathcal{X}\subseteq \mathbb{R} \}$ denote a one-dimensional family of distributions over $\mathcal{Y}$, where $\mathcal{X}$ includes all $x$ such that $p(\bullet;x)$ is a distribution. Then $\mathcal{P}$ is an exponential family if and only if for every $p_{1}(\bullet),p_{2}(\bullet),p_{3}(\bullet)\in \mathcal{P}$, there exists some $\lambda \in \mathbb{R}$ such that
> $$
> p_{3}(\mathbf{y})=\frac{p_{1}(\mathbf{y})^{\lambda}p_{2}(\mathbf{y})^{1-\lambda}}{Z(\lambda)}.
> $$

^617004

> [!proof] Omitted, rather trivial

### Tilting Distributions

> [!example] (Tilting distributions)
> Given a base distribution $q(\bullet)$ over an alphabet $\mathcal{Y}\subseteq \mathbb{R}$, we define an exponential family parameterized by $x$ via
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\frac{q(\mathbf{y})e^{xt(\mathbf{y})}}{Z(x)},
> $$
> where $\mathbf{y}\in \mathcal{Y}$ and $x \in \mathbb{R}$. Each member of this family is called a ==tilted distribution==.

If $x>0$, this weights large values of $t(\mathbf{y})$ exponentially more than the base distribution; the opposite occurs when $x<0$. Explicitly, this is a one-parameter exponential family via
$$
\lambda(x)=x,\quad
t(\mathbf{y}),\quad
\alpha(x)=\ln Z(x),\quad
\beta(\mathbf{y})=\ln q(\mathbf{y}).
$$
---

**Next:** [[Efficient Estimators and Exponential Families]]
