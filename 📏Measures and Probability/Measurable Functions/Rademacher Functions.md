We will use a trick to define an infinite sequence of independent random variables with given distribution functions on $\Omega=(0,1]$.

Each $\omega \in\Omega$ has a unique binary expansion
$$
\omega=0.\omega_{1}\omega_{2}\omega_{3}\cdots,
$$
provided that we forbid infinite sequences of 0s.

> [!definition] Definition (Rademacher functions)
> The ==Rademacher functions== are random variables $R_{n}:\Omega\to \{ 0,1 \}$ given by $R_{n}(\omega)=\omega_{n}$. In particular,
> $$
> R_{1}=\mathbf{1}_{\left( \frac{1}{2},1 \right]},\quad
> R_{2}=\mathbf{1}_{\left( \frac{1}{4}, \frac{1}{2} \right]}
> + \mathbf{1}_{\left( \frac{3}{4}, 1 \right]}, \quad
> R_{3}=\mathbf{1}_{\left( \frac{1}{8}, \frac{1}{4} \right] }
> + \mathbf{1}_{\left( \frac{3}{8}, \frac{1}{2} \right] }
> + \mathbf{1}_{\left( \frac{5}{8}, \frac{3}{4} \right] }
> + \mathbf{1}_{\left( \frac{7}{8}, 1 \right] }.
> $$
> and so on. These are independent Bernoulli random variables.

> [!proposition] Proposition (Building independent r.v.s from CDFs)
> Let $(\Omega,\mathcal{F},\mathbb{P})$ be the probability space of the Lebesgue measure on the Borel subsets of $(0,1]$. Let $(F_{n}:n\in \mathbb{N})$ be a sequence of distribution functions. Then, there exists a sequence $(X_{n}:n\in \mathbb{N})$ of independent random variables on $(\Omega,\mathcal{F},\mathbb{P})$ such that $X_{n}$ has distribution function $F_{X_{n}}=F_{n}$ for all $n$.

> [!proof]-
> The idea is that we can use our Rademacher functions to extend our uniform randomness on $(0,1]$ to countably many copies of uniform randomness on $(0,1]$.
> 
> Pick some bijection $m:\mathbb{N}^{2}\to \mathbb{N}$ and define $Y_{k,n}=R_{m(k,n)}$. Now, set
> $$
> Y_{n}=\sum_{k=1}^{\infty}2^{-k}Y_{k,n}.
> $$
> Then, $Y_{i}$ are all independent uniform random variables, i.e. it can be checked that $\mathbb{P}(Y_{n}\leq x)=x$ for all $x$. Now, just pass each through the corresponding CDF, as in [[Random Variables#^d66555|the method for constructing a single random variable]], to finish.

---

**Next:** [[Random Variables]]