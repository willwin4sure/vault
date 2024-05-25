> Large deviation analysis over finite alphabets.

In this section and the next, we restrict to the case of discrete distributions over a finite alphabet $\mathcal{Y}$. This framework, called the method of types, leads to Sanov's theorem, which is stronger than [[Large Deviations and Cramér's Theorem#^3ce5f3|Cramér's theorem]] for this special case.

## Types and Type Classes

Recall the definition of the [[Alternating Projections#^3b50bd|type]] of a sequence $\mathbf{y}$ on $\mathcal{Y}$ as the empirical distribution of values.

This is an estimate of the true distribution $p$ that generated $\mathbf{y}$, and the WLLN implies
$$
\lim_{ N \to \infty } \mathbb{P}\left( \left| \hat{p}(b;\mathbf{y})-p(b) \right| >\varepsilon \right)=0,
$$
i.e. $\hat{p}(b;\mathbf{y})\xrightarrow[]{\mathbb{P}}p(b)$.

> [!definition] (Set of types)
> The ==set of types== $\mathcal{P}_{N}^{\mathcal{Y}}$ is the set of all possible types for sequences of length $N$ generated from an alphabet $\mathcal{Y}$.

> [!definition] (Type class)
> Let $p\in \mathcal{P}_{N}^{\mathcal{Y}}$. The set of all length-$N$ sequences whose type is $p$ is
> $$
> \mathcal{T}_{N}^{\mathcal{Y}}(p)=\left\{ \mathbf{y}\in \mathcal{Y}^{N}:\hat{p}(\bullet;\mathbf{y})\equiv p(\bullet) \right\},
> $$
> and this is called the ==type class==. 

> [!claim]
> $$
> \frac{1}{N}\sum_{n=1}^{N}t(y_{n})=\sum_{b=1}^{M}\hat{p}(b;\mathbf{y})t(b).
> $$

> [!proof] Really obvious

## Exponential Rate Notation

> [!notation]
> We use $f(N) \doteq 2^{N\alpha}$ to denote
> $$
> \lim_{ N \to \infty } \frac{\log f(N)}{N}=\alpha.
> $$
> This says that the approximation $f(N)\approx 2^{N\alpha}$ is accurate to the first-order in the exponent.

The limiting cases of this notation are defined as follows:

* When $\alpha \to \infty$, i.e. $f(N)\doteq \infty$, $f(\bullet)$ grows super-exponentially, e.g. $f(N)=2^{N^{2}}$.
* When $\alpha\to-\infty$, i.e. $f(N)\doteq 0$, $f(\bullet)$ decays super-exponentially, e.g. $f(N)=2^{-N^{2}}$.
* When $\alpha=0$, i.e. $f(N)\doteq 1$, $f(\bullet)$ grows or decays sub-exponentially, e.g. $f(N)=N^{2}$ or $f(N)=\frac{1}{\sqrt{ N }}$.

> [!claim]
> If $f(N)\doteq 2^{N\alpha}$ and $g(N)\doteq 2^{N\beta}$, then
> $$
> f(N)+g(N)\doteq 2^{N\max\{\alpha,\beta\}},\quad
> f(N)\cdot g(N)\doteq 2^{N(\alpha+\beta)}.
> $$

## Properties of Types

Once again, consider how likely it is that sampling a distribution $q$ will generate a sequence whose type is $p$. Intuition from the previous set of notes should suggest that
$$
Q\{ T_{N}^{\mathcal{Y}}(p) \}\approx 2^{-ND(p\parallel q)}.
$$
Even though there are exponential (in $N$) many sequences, there are only polynomial many types:

> [!claim]
> For any finite alphabet $\mathcal{Y}$,
> $$
> \left| \mathcal{P}_{N}^{\mathcal{Y}} \right| \leq (N+1)^{|\mathcal{Y}|}.
> $$

^aae286

This is clear, since each type can be expressed as a list of $|\mathcal{Y}|$ integers in the interval $[0,N]$. Therefore, at least one of the type classes is exponentially large. In fact, essentially *all* type classes are exponentially large. 

Since the probability of a sequence occurring is permutation invariant, it only depends on its type. The following theorem quantifies this exponentially small probability:

> [!claim]
> Let $\mathbf{y}=(y_{1},\dots,y_{N})$ be a sequence drawn from a finite alphabet $\mathcal{Y}$, and let $q$ be any distribution defined on the same alphabet. Then,
> $$
> q^{N}(\mathbf{y})=2^{-N(D(\hat{p}(\bullet;\mathbf{y})\parallel q)+H(\hat{p}(\bullet;\mathbf{y})))}.
> $$

Next, we establish that every type class contains exponentially many sequences.

> [!claim]
> Let $p \in \mathcal{P}_{N}^{\mathcal{Y}}$ for a finite alphabet $\mathcal{Y}$. Then, $\left| \mathcal{T}_{N}^{\mathcal{Y}}(p) \right|\doteq 2^{NH(p)}$. In particular,
> $$
> cN^{- |\mathcal{Y}|}2^{NH(p)}\leq \left| \mathcal{T}_{N}^{\mathcal{Y}}(p) \right| \leq 2^{NH(p)},
> $$
> where $c$ is a constant that may depend on $|\mathcal{Y}|$ and $p$ but not on $N$.

We combine these to establish our main result, that there is an exponentially small probability that a sequence sampled from $q$ will have type $p$, when $q\neq p$.

> [!theorem]
> Let $p \in \mathcal{P}_{N}^{\mathcal{Y}}$, and let $q \in \mathcal{P}^{\mathcal{Y}}$ be a distribution over the same alphabet. Then,
> $$
> cN^{- |\mathcal{Y}|}2^{-ND(p\parallel q)}\leq Q\left\{ \mathcal{T}_{N}^{\mathcal{Y}}(p) \right\} \leq 2^{-ND(p\parallel q)},
> $$
> where $c$ is a constant that may depend on $|\mathcal{Y}|$ and $p$ but not $N$, so
> $$
> Q\left\{ \mathcal{T}_{N}^{\mathcal{Y}}(p) \right\}\doteq 2^{-ND(p\parallel q)}.
> $$

^0abe2c

> [!remark]
> The special case where $q=p$ establishes that $P\left\{ \mathcal{T}_{N}^{\mathcal{Y}}(p) \right\}\doteq 1$, which just says the probability we observe a sequence with type $p$ when sampling from $p$ is not exponentially small.

---

**Next:** [[Sanov's Theorem]]


