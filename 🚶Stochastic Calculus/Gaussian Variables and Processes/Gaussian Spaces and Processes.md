In this section, we introduce the notion of a Gaussian space, which allows us to think about Gaussians geometrically.

## Gaussian Spaces

Work over a probability space $(\Omega,\mathcal{F},\mathbb{P})$. Write $L^2(\Omega,\mathcal{F},\mathbb{P})$ as the space of $\mathbb{R}$-valued square-integrable functions on $(\Omega,\mathcal{F},\mathbb{P})$, modulo measure zero differences (we called this $\mathcal{L}^{2}$ in 18.675). This [[forms a Hilbert space]], equipped with the natural inner product
$$
\langle X,Y \rangle_{L^{2}(\Omega,\mathcal{F},\mathbb{P})}=\mathbb{E}[XY]=\int_{\Omega}X(\omega)Y(\omega) \, \mathbb{P}(d\omega). 
$$
> [!definition] Definition (Gaussian space)
> A ==(centered) Gaussian space== is a closed linear subspace $H$ of $L^2(\Omega,\mathcal{F},\mathbb{P})$ consisting only of Gaussian random variables.

> [!example] Example (Basic Gaussian spaces)
> * If $X\sim \mathcal{N}(0,1)$ is a one-dimensional Gaussian, then $\text{span}(X)$ is a Gaussian space.
> * $H=\text{span}(X,Y)$ is a Gaussian space if and only if $(X,Y)$ is jointly Gaussian (i.e. a 2D Gaussian vector).

The [[Gaussian Variables#^03c5af|nonexample from last section]] still works: even though $Z$ and $\varepsilon Z$ are both Gaussian, they do not span a Gaussian space. For example, $Z+\varepsilon Z$ is $0$ half the time, so it is assuredly not Gaussian.

## Geometric Interpretations

> [!example] Example (Gaussian space isometry)
> Consider some standard Gaussian $Z\sim \mathcal{N}(0,I_{n\times n})\in \mathbb{R}^{n}$. For any $v\in \mathbb{R}^{n}$, we define $Gv=\langle Z,v \rangle$ as the projection of $Z$ onto $v$. This is a Gaussian random variable, [[Gaussian Variables#^373590|by definition]]. Then, note that for any $u,v\in \mathbb{R}^n$,
> $$
> \langle Gu,Gv \rangle_{L^{2}}=\langle u,v \rangle_{\mathbb{R}^{n}}.
> $$
> This tells us that the mapping $G$ from $u\in\mathbb{R}^n$ to $Gu\in H\subseteq L^{2}(\Omega,\mathcal{F},\mathbb{P})$, the Gaussian space, is an isometry. This rigorizes a connection between geometry and probability.

Independence of Gaussian variables corresponds to orthogonality in this geometric space!

> [!theorem] Theorem (Orthogonality $\Longleftrightarrow$ Independence in Gaussian spaces)
> Let $H\subseteq L^{2}(\Omega,\mathcal{F},\mathbb{P})$ be a centered Gaussian space, and let $(H_{\alpha})_{\alpha \in I}$ be linear subspaces of $H$. Then, the $\sigma$-algebras $(\sigma(H_{\alpha}))_{\alpha \in I}$ are independent if and only if the $H_{\alpha}$ are pairwise orthogonal.

> [!proof]- Omitted
> The proof is omitted. It is basically an extension of [[Gaussian Variables#^6037a8|a claim from the last section]], and the full proof can be found in the textbook.

Conditional expectation among Gaussian variables corresponds to orthogonal projection in this geometric space! Let's start with a basic example problem.

> [!question]
> Suppose we have a bivariate Gaussian given by
> $$
> \begin{bmatrix}
> X \\
> Y
> \end{bmatrix}\sim \mathcal{N}\left( 
> \begin{bmatrix}
> 0 \\
> 0
> \end{bmatrix},
> \begin{bmatrix}
> a & b \\
> b & c
> \end{bmatrix}
> \right),
> $$
> where we assume that the covariance matrix is positive definite. What is the law of $Y$ conditional on $X$?

> [!proof]- Solution
> The goal is to write $Y=Y^{\parallel}+Y^{\perp}$ where $Y^{\parallel}\in \text{span}(X)$ and $Y^{\perp}\perp \text{span}(X)$. There exists some $\theta \in \mathbb{R}$ such that $Y^{\parallel}=\theta X$, and we need that $Y-\theta X$ is independent to $X$; this is equivalent to
> $$
> 0=\langle Y-\theta X,X \rangle = b - \theta a\implies \theta=\frac{b}{a},
> $$
> as everything is jointly Gaussian. 
> 
> Thus
> $$
> Y=\frac{b}{a}X + \left( Y - \frac{b}{a}X \right).
> $$
> The variance of the second term is equal to
> $$
> \text{Var}\left( Y-\frac{b}{a}X \right) =\text{Cov}\left( Y-\frac{b}{a}X,Y-\frac{b}{a}X \right) = c - \frac{b^{2}}{a},
> $$
> which also matches what we'd expect from the Pythagorean theorem! Therefore, the conditional distribution is
> $$
> Y\mid X\sim \mathcal{N}\left( \frac{bX}{a},c-\frac{b^{2}}{a} \right). 
> $$
> 

The more general case is left as an exercise:

> [!question]
> Now suppose $X \in \mathbb{R}^{k}$ and $Y \in \mathbb{R}^{\ell}$ are jointly Gaussian, with
> $$
> \begin{bmatrix}
> X \\
> Y
> \end{bmatrix}
> \sim
> \mathcal{N}\left( 
> \begin{bmatrix}
> 0 \\
> 0
> \end{bmatrix},
> \begin{bmatrix}
> A & B \\
> B^{T} & C
> \end{bmatrix}
> \right).
> $$
> What is the law of $Y$ conditional on $X$?

We can write a statement in full generality:

> [!theorem]
> Let $H\subseteq L^{2}(\Omega,\mathcal{F},\mathbb{P})$ be a centered Gaussian space, and let $K\subseteq H$ be a closed subspace. Then, for any $X \in H$, we have
> $$
> X|\sigma(K)=\mathcal{N}\left(\pi_{K}(X),\mathbb{E}[(X-\pi_{K}(X))^{2}]\right),
> $$
> where $\pi_{K}$ denotes the orthogonal projection of $X$ onto $K$.

> [!remark]-
> We can contrast this with the general case where we take some arbitrary $X \in L^{2}(\Omega,\mathcal{F},\mathbb{P})$ and condition on $\sigma(K)$; the expected value would be
> $$
> \mathbb{E}[X|\sigma(K)]=\pi_{L^{2}(\Omega,\sigma(K),\mathbb{P})}(X).
> $$
> Here, we project onto a much larger space: if $K$ is the span of some variable $Z$, then $L^{2}(\Omega,\sigma(K),\mathbb{P})$ contains all measurable functions of $Z$. So the theorem says that we can project onto a much smaller subspace in the Gaussian space.
> 

Here's a problem on the homework, where the idea is to consider the Gaussian space spanned by all our noise $\varepsilon_{n}$ and $\eta_{n}$; all of our Gaussians lie in this space. Then, conditional expectation just corresponds to orthogonal projections.

> [!problem] Problem (Kalman Filter)
> Suppose we have independent Gaussians $\varepsilon_{n}\sim \mathcal{N}(0,\sigma^{2})$ and $\eta_{n}\sim \mathcal{N}(0,\delta^{2})$ and we have some true unknown state of a system which evolves over time:
> $$
> 0=X_{0}\to X_{1}\to X_{2}\to \cdots \to X_{n},\quad X_{n+1}=a_{n}X_{n}+\varepsilon_{n+1}.
> $$
> We try to infer this state from noisy observations $Y_{n}=cX_{n}+\eta_{n}$. Given the values of $a_{n}$, $c$, $\sigma^{2}$, and $\delta^{2}$, determine $\mathbb{E}[X_{n}|Y_{1},\dots,Y_{n}]$.

## Gaussian Processes

This is a generalization of Gaussian spaces.

[!definition] Definition (Gaussian processes)
Let $I$ be an arbitrary (possibly uncountable) index set. A collection of random variables $(X_{t})_{t\in I}$ is a ==(centered) Gaussian process== if any finite linear combination of the $X_{t}$s is a one-dimensional Gaussian. The ==Gaussian space generated by the process $(X_{t})_{t\in I}$== is the closure of the linear span of the $X_{t}$s. 