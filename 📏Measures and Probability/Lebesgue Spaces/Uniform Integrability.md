> The (counter-)example to keep in mind this section is $X_{n}=n\mathbf{1}_{(0,1 / n)}$. This sequences converges in probability but not in $L^{1}$; this is because it is not UI.
## Definition

> [!definition] Definition (Uniformly integrable)
> Let $\mathcal{X}$ be a family of random variables. For $1\leq p\leq \infty$, we say that $\mathcal{X}$ is ==bounded in $L^{p}$== if $\sup_{X \in \mathcal{X}}\| X \|_{p}<\infty$. We define
> $$
> I_{\mathcal{X}}(\delta)=\sup \{ \mathbb{E}[|X|\mathbf{1}_{A}] : X \in \mathcal{X}, A \in \mathcal{F}, \mathbb{P}(A)\leq\delta \}.
> $$
> Evidently $\mathcal{X}$ is bounded in $L^{1}$ if and only if $I_{\mathcal{X}}(1)<\infty$. We say that $\mathcal{X}$ is ==uniformly integrable (UI)== if $\mathcal{X}$ is bounded in $L^{1}$ and
> $$
> I_{\mathcal{X}}(\delta)\downarrow 0
> $$
> as $\delta \downarrow 0$.

Note that being bounded in $L^{p}$ for $p \in(1,\infty)$ implies UI. This is because we can bound
$$
\mathbb{E}[|X|\mathbf{1}_{A}]\leq \mathbb{E}[|X|^{p}]^{1/p}\cdot\mathbb{P}(A)^{1/q}
$$
by [[Hölder's Inequality|Hölder's inequality]], where $q$ is the conjugate index of $p$. This does not work when $p=1$; take the classic counterexample of $X_{n}=n\mathbf{1}_{(0,1/n)}$.

> [!example] Example (Family of one integrable variable is UI)
> Let $X$ be an integrable variable and consider $\mathcal{X}=\{ X \}$. Then, $\mathcal{X}$ is UI.

> [!proof]-
> Suppose not. Then, for some $\varepsilon>0$, there exists a sequence of $A_{n}\in \mathcal{F}$ such that $\mathbb{P}(A_{n})\leq 2^{-n}$ and $\mathbb{E}[|X|\mathbf{1}_{A_{n}}]\geq\varepsilon$ for all $n$. However, by the [[Borel-Cantelli Lemmas|first Borel-Cantelli lemma]], $\mathbb{P}(A_{n}\text{ i.o.})=0$, yet by the dominated convergence theorem,
> $$
> \varepsilon \leq \mathbb{E}\left[ |X|\mathbf{1}_{\bigcup_{m\geq n}^{}A_{m}} \right] \to \mathbb{E}[|X|\mathbf{1}_{\{ A_{n}\text{ i.o.} \}}] = 0,
> $$
> a contradiction.

This easily extends to any *finite* collection of integrable variables. Moreover, for any integrable random variable $Y$, the set
$$
\mathcal{X}=\{ X : X\text{ a random variable}, |X|\leq Y \}
$$
is UI, because $\mathbb{E}[|X|\mathbf{1}_{A}]\leq \mathbb{E}[Y\mathbf{1}_{A}]$ for all $A$.

## Alternate Characterization of UI

> [!claim]
> Let $\mathcal{X}$ be a family of random variables. Then $\mathcal{X}$ is UI if and only if
> $$
> \sup \{ \mathbb{E}[|X|\mathbf{1}_{|X|\geq K}] : X \in \mathcal{X} \}\to 0
> $$
> as $K\to \infty$.

> [!proof]-
> First, suppose that $\mathcal{X}$ is UI. Given some $\varepsilon>0$, choose $\delta>0$ such that $I_{\mathcal{X}}(\delta)<\varepsilon$, then choose $K<\infty$ so that $I_{\mathcal{X}}(1)\leq K\delta$. Then, for $X \in \mathcal{X}$ and $A=\{ |X|\geq K \}$, we have $\mathbb{P}(A)\leq\delta$ so $\mathbb{E}[|X|\mathbf{1}_{A}]<\varepsilon$. Therefore, the desired condition holds.
> 
> For the other direction, note that if the condition holds, then, since
> $$
> \mathbb{E}[|X|]\leq K+\mathbb{E}[|X|\mathbf{1}_{|X|\geq K}],
> $$
> we know that $I_{\mathcal{X}}(1)<\infty$. Given some $\varepsilon>0$, choose $K<\infty$ so that $\mathbb{E}[|X|\mathbf{1}_{|X|\geq K}]<\varepsilon / 2$ for all $X \in \mathcal{X}$. Then, choose $\delta>0$ so that $K\delta<\varepsilon / 2$. For all $X \in \mathcal{X}$ and $A\in \mathcal{F}$ with $\mathbb{P}(A)<\delta$, we have
> $$
> \mathbb{E}[|X|\mathbf{1}_{A}]\leq \mathbb{E}[|X|\mathbf{1}_{|X|\geq K}]+K\mathbb{P}(A)<\varepsilon.
> $$
> Hence $\mathcal{X}$ is UI.
## Convergence in $L^{1}$

In this section, we present the definitive result on $L^{1}$-convergence of random variables.

> [!theorem]
> Let $X$ be a random variable and let $(X_{n}:n\in \mathbb{N})$ be a sequence of random variables. The following are equivalent:
> 
> 1. $X_{n}\in L^{1}$ for all $n$, $X \in L^{1}$, and $X_{n}\to X$ in $L^{1}$.
> 2. $\{ X_{n}:n\in \mathbb{N} \}$ is UI and $X_{n}\to X$ in probability.
> 

> [!proof]-
> We'll first prove the forward direction. By [[Chebyshev's Inequality|Chebyshev's inequality]], for $\varepsilon>0$, we have
> $$
> \mathbb{P}(|X_{n}-X|>\varepsilon)\leq\varepsilon^{-1}\mathbb{E}[|X_{n}-X|]\to 0
> $$
> so $X_{n}\to X$ in probability. Moreover, given $\varepsilon>0$, there exists $N$ such that $\mathbb{E}[|X_{n}-X|]<\varepsilon / 2$ whenever $n\geq N$. Then, we can find $\delta>0$ so that $\mathbb{P}(A)\leq\delta$ implies
> $$
> \mathbb{E}[|X|\mathbf{1}_{A}]\leq\varepsilon/2,\quad \mathbb{E}[|X_{n}|\mathbf{1}_{A}]\leq\varepsilon
> $$
> over $n=1,\dots,N$. Then, for $n\geq N$ and $\mathbb{P}(A)\leq\delta$,
> $$
> \mathbb{E}[|X_{n}|\mathbf{1}_{A}]\leq \mathbb{E}[|X_{n}-X|]+\mathbb{E}[|X|\mathbf{1}_{A}]\leq\varepsilon.
> $$
> Hence $\{ X_{n}:n\in \mathbb{N} \}$ is UI.
> 
> Now, we'll prove the backward direction. By [[Convergence of Measurable Functions#^dda4e5|a previous theorem]], we know there exists a subsequence $(n_{k})$ such that $X_{n_{k}}$ converges to $X$ almost surely. Therefore, by Fatou's lemma, $\mathbb{E}[|X|]\leq \liminf_{ k \to \infty }\mathbb{E}[|X_{n_{k}}|]<\infty$.
> 
> By our alternate characterization of UI, given any $\varepsilon>0$, there exists some $K<\infty$ such that, for all $n$,
> $$
> \mathbb{E}[|X_{n}|\mathbf{1}_{|X_{n}\geq K}]<\varepsilon / 3,\quad \mathbb{E}[|X|\mathbf{1}_{|X|\geq K}]<\varepsilon / 3.
> $$
> Consider the uniformly bounded sequence $X_{n}^{K}=(-K)\lor X_{n}\land K$, and set $X^{K}=(-K)\lor X\land K$. Then $X_{n}^{K}\to X^{K}$ in probability, so, by bounded convergence, there exists $N$ such that for all $n\geq N$ $$
> \mathbb{E}|X_{n}^{K}-X^{K}|<\varepsilon / 3.
> $$
> But then, for all $n\geq N$,
> $$
> \mathbb{E}|X_{n}-X|\leq \mathbb{E}[|X_{n}|\mathbf{1}_{|X_{n}|\geq K}]+\mathbb{E}[X_{n}^{K}-X^{K}]+\mathbb{E}[|X|\mathbf{1}_{|X|\geq K}] < \varepsilon.
> $$
> Yet $\varepsilon$ was arbitrary, which finishes.
>

---

**Next:** [[⛺Lebesgue Spaces Homepage]]
