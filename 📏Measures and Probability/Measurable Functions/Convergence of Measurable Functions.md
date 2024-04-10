## Definitions

For the following definitions, consider a sequence of measurable functions $(f_{n}:n\in \mathbb{N})$ over a measure space $(E,\mathcal{E},\mu)$. These definitions are given in approximate decreasing order of strength (we will see some theorems that rigorize this).

> [!definition] Definition (Convergence a.e.)
> We say that $f_{n}$ ==converges almost everywhere== to $f$ if
> $$
> \mu(\{ x \in E : f_{n}(x)\not\to f(x) \})=0.
> $$
> In the context of probability spaces, we write ==converges almost surely==. This means that in almost all worlds $\omega \in\Omega$, $f_{n}(\omega)$ converges to $f(\omega)$.

^061df9

> [!definition] Definition (Convergence in measure)
> We say that $f_{n}$ ==converges in measure== to $f$ if
> $$
> \mu(\{ x \in E : |f_{n}(x)-f(x)|>\varepsilon \})\xrightarrow[n\to \infty]{} 0
> $$
> for all $\varepsilon>0$. In the context of probability spaces, we write ==converges in probability==. This means that the probability that $f_{n}$ is $\varepsilon$-far from $f$ goes to $0$ as $n$ goes to $\infty$.

^dfe486

The last two definitions require that each of the $f_{n}$ are *jointly* defined on the same space. The following definition for convergence of random variables does not even impose such a condition.

> [!definition] (Convergence in distribution)
> For a sequence of $\mathbb{R}$-valued random variables $(X_{n}:n\in \mathbb{N})$, we say that $X_{n}$ ==converges in distribution== to $X$ if $F_{X_{n}}(x)\xrightarrow[n\to \infty]{}F_{X}(x)$ at all points $x \in \mathbb{R}$ where $F_{X}$ is continuous.

^8b5c0e

## Implications

The following two theorems discuss some relationships between the above types of convergence.

> [!theorem]
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of measurable functions.
> 
> 1. Take $\mu(E)<\infty$. If $f_{n}\to 0$ a.e., then $f_{n}\to 0$ in measure.
> 2. If $f_{n}\to 0$ in measure, then $f_{n_{k}}\to 0$ a.e. for some subsequence $(n_{k})$.

^dda4e5

> [!proof]-
> 1. Suppose that $f_{n}\to 0$ a.e. For each $\varepsilon>0$, we have that
> $$
> \mu(|f_{n}|\leq\varepsilon)\geq \mu \left( \bigcap_{m\geq n}^{}\{ |f_{m}|\leq\varepsilon \} \right),
> $$
> i.e. the set of points where $f_{n}$ is $\varepsilon$-close to $0$ is larger than the set of points where $f_{n}$ will keep being $\varepsilon$-close to $0$. However, note that as $n\to \infty$,
> $$
> \mu \left( \bigcap_{m\geq n}^{}\{ |f_{m}|\leq\varepsilon \} \right) \uparrow \mu (|f_{n}|\leq\varepsilon \text{ ev.})\geq \mu(f_{n}\to 0)=\mu(E)
> $$
> by convergence almost everywhere. Thus $\mu(|f_{n}|>\varepsilon)\to 0$ for every $\varepsilon$, by finiteness.
> 2. Suppose $f_{n}\to 0$ in measure. Then, for all $k\in \mathbb{N}$, we can find some $n_{k}$ such that $\mu(|f_{n_{k}}|\geq 2^{-k})\leq 2^{-k}$ (as the left hand side tends to zero when $n\to \infty$). Therefore,
> $$
> \sum_{k}^{}\mu(|f_{n_{k}}|\geq 2^{-k})<\infty,
> $$
> so by the [[Borel-Cantelli Lemmas#^8e9b48|first Borel-Cantelli lemma]],
> $$
> \mu(|f_{n_{k}}|\geq 2^{-k}\text{ i.o.})=0
> $$
> and thus $f_{n_{k}}\to 0$ a.e.

> [!remark]- Remark (Finiteness matters for the first case)
> In the case where $\mu(E)=\infty$, we could have functions $(f_{n})$ that represent a nonzero wave travelling from left to right (all functions are all 0 but with spikes at around $x=n$, for example).
> 
> Then, $f_{n}\to 0$ a.e. since at any point, it is eventually always zero. However, $f_{n}$ does not converge to 0 in measure, since there always exist some points $x$ where each $f_{n}$ is far from $f$.

> [!remark]- Remark (Functions that converge in measure but not a.e.)
> The canonical example of a sequence of functions that converge in probability but not almost surely is a sequence of independent Bernoulli random variables $R_{n}$ with probability $\frac{1}{n}$ of being $1$. This is by the [[Borel-Cantelli Lemmas#^8e9b48|second Borel-Cantelli lemma]], as the harmonic series diverges.
> 
> However, by picking a subsequence, we can only consider $R_{2^k}$, which converges to $0$ a.s.

> [!theorem]
> Let $X$ and $(X_{n}:n\in \mathbb{N})$ be $\mathbb{R}$-valued random variables.
> 
> 1. If $X$ and $(X_{n}:n\in \mathbb{N})$ are defined on the same probability space $(\Omega,\mathcal{F},\mathbb{P})$ and $X_{n}\to X$ in probability, then $X_{n}\to X$ in distribution.
> 2. If $X_{n}\to X$ in distribution, then there exist random variables $\tilde{X}$ and $(\tilde{X}_{n}:n\in \mathbb{N})$ defined on a common probability space $(\Omega,\mathcal{F},\mathbb{P})$ such that $\tilde{X}$ has the same distribution as $X$, $\tilde{X}_{n}$ has the same distribution as $X_{n}$ for all $n$, and $\tilde{X}_{n}\to \tilde{X}$ almost surely.

> [!proof]-
> Let $S\subseteq \mathbb{R}$ consist of points where $F_{X}$ is continuous.
> 
> 1. For any $x \in S$ and $\varepsilon>0$, since $F_{X}$ is continuous, there exists some $\delta>0$ such that
> $$
> F_{X}(x-\delta)\geq F_{X}(x)-\frac{\varepsilon}{2},\quad F_{X}(x+\delta)\leq F_{X}(x)+\frac{\varepsilon}{2}.
> $$
> Further, since $X_{n}\to X$ in probability, there exists some $N$ such that for all $n\geq N$, we have that $\mathbb{P}(|X_{n}-X|>\delta)< \frac{\varepsilon}{2}$. Combining gives
> $$
> F_{X_{n}}(x)\leq F_{X}(x+\delta)+ \mathbb{P}(|X_{n}-X|>\delta)\leq F_{X}(x)+\varepsilon
> $$
> for all such $n\geq N$. In other words, the $X_{n}\leq x$ always occurs if at least one of the following holds: $X\leq x+\delta$, or $X_{n}$ drifted far away from $X$. Similarly, we can write
> $$
> F_{X_{n}}(x)\geq F_{X}(x-\delta)-\mathbb{P}(|X_{n}-X|>\delta)\geq F_{X}(x)-\varepsilon.
> $$
> Hence $X_{n}\to X$ in distribution.
> 2. We need to exhibit a coupling of these random variables (i.e. define a joint distribution) that exhibits almost sure convergence (we want to do it tightly). It turns out we can use our standard trick of passing a uniform random variable on $(0,1]$ through the inverse CDFs. We will use the same uniform random variable for all $X_{n}$ and $X$ in order to define them jointly.
> The rest is just details checking that this actually works. Work over $(\Omega,\mathcal{F},\mathbb{P})$ as the interval $(0,1)$ equipped with its Borel $\sigma$-algebra and the Lebesgue measure. Then, to define each random variable $\tilde{X}_{n}$, push $\Omega$ through the inverse CDF of $X_{n}$. In particular, for each $\omega \in(0,1]$, define
> $$
> \tilde{X}_{n}(\omega)=\inf\{ x \in \mathbb{R}:\omega \leq F_{X_{n}}(x) \}, \quad
> \tilde{X}=\inf\{ x \in \mathbb{R} : \omega \leq F_{X}(x) \}.
> $$
> Let $\Omega_{0}$ denote the subset of $(0,1)$ on which $\tilde{X}$ is continuous. Since $\tilde{X}$ is non-decreasing, $(0,1)\setminus\Omega_{0}$ is countable. Therefore, $\mathbb{P}(\Omega_{0})=1$. Similarly, since $F_{X}$ is non-decreasing, $\mathbb{R}\setminus S$ is countable; $S$ is dense. In particular, $\Omega_{0}$ consists of points where $F_{X}$ is not flat, whereas $S$ consists of points where $F_{X}$ does not jump. (If there were uncountable discontinuities, this corresponds to uncountably many positive differences between left and right limits, which is impossible.)
> We claim that for any $\omega \in\Omega_{0}$, $\tilde{X}_{n}(\omega)\to \tilde{X}(\omega)$. Note that $\Omega_{0}$ consists of points where $F_{X}$ is not flat. Pick any $\varepsilon>0$. There exist $x^{+},x^{-}\in S$ such that $x^{-}<\tilde{X}(\omega)<x^{+}$ and $x^{+}-x^{-}\leq\varepsilon$; we can further find $\omega^{+}\in(\omega,1)$ such that $\tilde{X}(\omega^{+})\leq x^{+}$. This allows us to write strict bounds
> $$
> F_{X}(x^{-})<\omega,\quad F_{X}(x^{+})\geq\omega^{+}>\omega.
> $$
> We can find $\omega^{+}$ since we are at non-flat regions of $F_{X}$; we only need to worry about issues on the positive side since $\tilde{X}$ is left-continuous.
> Now the values of $F_{X}(x^{-})$, $F_{X}(\tilde{X}(\omega))$, and $F_{X}(x^{+})$ are all distinct. Applying convergence in distribution at our points $x^{-},x^{+}\in S$ tells us that there exists some $N$ such that for all $n\geq N$, $F_{X_{n}}$ exhibits this distinguishing power too: $F_{X_{n}}(x^{-})<\omega$ and $F_{X_{n}}(x^{+})>\omega$. Thus $\tilde{X}_{n}(\omega)\in(x^{-},x^{+})$; hence $|\tilde{X}_{n}(\omega)-\tilde{X}(\omega)|\leq\varepsilon$ eventually. Therefore, almost surely, $\tilde{X}_{n}(\omega)\to \tilde{X}(\omega)$, as desired.

---

**Next:** [[Tail Events]]