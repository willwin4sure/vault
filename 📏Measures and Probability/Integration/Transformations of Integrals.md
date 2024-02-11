This section contains a few ways of constructing integrals from other ones.

> [!proposition] Proposition (Restriction of integrals)
> Let $(E,\mathcal{E},\mu)$ be a measure space and let $A\in \mathcal{E}$. Then the set $\mathcal{E}_{A}$ of measurable subsets of $A$ is a $\sigma$-algebra and the restriction $\mu_{A}$ of $\mu$ to $\mathcal{E}_{A}$ is a measure. Moreover, for any nonnegative measurable function $f$ on $E$, we have
> $$
> \mu(f\mathbf{1}_{A})=\mu_{A}(f|_{A}).
> $$

In the case of the Lebesgue measure on $\mathbb{R}$, this allows us to integrate over any interval $I$ where $\inf I = a$ and $\sup I = b$ as
$$
\int _{R}f\mathbf{1}_{I}(x) \, dx =\int _{I}f(x) \, dx =\int_{a}^{b} f(x) \, dx ,
$$
the familiar notation. Since the sets $\{a\}$ and $\{ b \}$ have measure zero, we do not need to specify if they are included in $I$.

> [!proposition] Proposition (Pushing integrals)
> Let $(E,\mathcal{E})$ and $(G,\mathcal{G})$ be measurable spaces and let $f:E\to G$ be a measurable function. Given a measure $\mu$ on $(E,\mathcal{E})$, define $\nu=\mu \circ f^{-1}$, the image measure on $(G,\mathcal{G})$.  Then, for all nonnegative measurable functions $g$ on $G$,
> $$
> \nu(g)=\mu(g\circ f).
> $$

 For example, if $X$ is a $G$-valued random variable on a probability space $(\Omega,\mathcal{F},\mathbb{P})$, for any nonnegative measurable function $g$ on $G$, we have
 $$
\mathbb{E}[g(X)]=\mu_{X}(g).
$$
This says exactly what you expect: the expected value of $g(X)$ is the integral of $g$ over the image measure of $X$.

> [!proposition] Definition (Constructing measures from functions)
> Let $(E,\mathcal{E},\mu)$ be a measure space and let $f$ be a nonnegative measurable function on $E$. Define $\nu(A)=\mu(f\mathbf{1}_{A})$ where $A\in \mathcal{E}$. Then $\nu$ is a measure on $E$ and, for all nonnegative measurable functions $g$ on $E$,
> $$
> \nu(g)=\mu(fg).
> $$

^ce3afe

This reweighting each piece of $E$ by the value of $f$. For example, for each nonnegative Borel function $f$ on $\mathbb{R}$, there corresponds a Borel measure $\mu$ on $\mathbb{R}$ given by $\mu(A)=\int _{A}f(x) \, dx$. Then, for all nonnegative Borel functions $g$,
$$
\mu(g)=\int _{\mathbb{R}}g(x)f(x) \, dx .
$$
We say that $\mu$ has ==density== $f$, with respect to the Lebesgue measure.

> [!definition] Definition (Density function)
> If the law $\mu_{X}$ of a real-valued random variable $X$ has a density $f_{X}$, then we call $f_{X}$ a ==density function== for $X$.

Then, $\mathbb{P}(X \in A)=\int _{A}f_{X}(x) \, dx$, for all Borel sets $A$, and, for all nonnegative Borel functions $g$ on $\mathbb{R}$,
$$
\mathbb{E}[g(X)]=\mu_{X}(g)=\int _{\mathbb{R}}g(x)f_{X}(x) \, dx .
$$
---

**Next:** [[The Fundamental Theorem of Calculus]]
