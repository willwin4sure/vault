In this section, we introduce some notions familiar to you from a basic probability class.
## Variance and Covariance

In this section, we look at some $L^{2}$ notions relevant to probability. For $X,Y\in L^{2}(\mathbb{P})$ with means $\mu_{X}=\mathbb{E}[X]$ and $\mu_{Y}=\mathbb{E}[Y]$, we define:

* The ==variance== of $X$ is $\text{var}(X)=\mathbb{E}[(X-\mu_{X})^{2}]$.
* The ==covariance== of $X$ and $Y$ is $\text{cov}(X,Y)=\mathbb{E}[(X-\mu_{X})(Y-\mu_{Y})]$.
* The ==correlation== between $X$ and $Y$ is $\text{corr}(X,Y)=\text{cov}(X,Y) / \sqrt{ \text{var}(X)\text{var}(Y) }$.

Note that $\text{var}(X)=0$ if and only if $X=\mu_{X}$ almost surely. Note also that if $X$ and $Y$ are independent, $\text{cov}(X,Y)=0$, but the converse is generally false. For a random variable $X=(X_{1},\dots,X_{n})\in \mathbb{R}^{n}$, we define its ==covariance matrix== as
$$
\text{var}(X)=\left( \text{cov}(X_{i},X_{j}) \right)_{i,j=1}^{n}.
$$
> [!proposition]
> Every covariance matrix is nonnegative definite.

> [!proof]-
> This is because we can write it as $\mathbb{E}[(X-\mu)(X-\mu)^{T}]$, so if we use it as a form,
> $$
> v^{T}\mathbb{E}[(X-\mu)(X-\mu)^T]v=\mathbb{E}\left[ \theta^T\theta \right]\geq 0, 
> $$
> where $\theta=(X-\mu)^{T}v$. 

## Conditional Expectation

In this section, we introduce the notion of conditional expectation, which we will [[build upon in the second half of the course]]. We start with a basic version.

> [!definition] Definition (Conditional expectation on a partition)
> Suppose we are given a countable family of disjoint events $(G_{i}:i\in I)$ whose union is $\Omega$. Set $\mathcal{G}=\sigma(G_{i}:i\in I)$. Let $X$ be an integrable random variable. The ==conditional expectation== of $X$ given $\mathcal{G}$ is given by
> $$
> Y=\sum_{i\in I}^{}\mathbb{E}[X|G_{i}]\mathbf{1}_{G_{i}},
> $$
> where we set $\mathbb{E}[X|G_{i}]=\frac{\mathbb{E}[X\mathbf{1}_{G_{i}}]}{\mathbb{P}(G_{i})}$ when $\mathbb{P}(G_{i})>0$, and define $\mathbb{E}[X|G_{i}]$ in any arbitrary way when $\mathbb{P}(G_{i})=0$.

Set $V=L^{2}(\mathcal{G},\mathbb{P})$ and note that $Y\in V$ (it is $\mathcal{G}$-measurable). Note that $V$ is a subspace of $L^{2}(\mathcal{F},\mathbb{P})$, and $V$ is complete and therefore closed.

> [!proposition] Proposition (Conditioning is orthogonal projection)
> If $X \in L^{2}$, then $Y$ is a version of the orthogonal projection of $X$ on $V$.

We will see a more sophisticated version of conditional expectation later.

---

**Next:** [[Bounded Convergence]]
