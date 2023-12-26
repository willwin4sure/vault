Probability theory is enriched by the significance attached to the notion of independence.

> [!definition] Definition (Independence of events)
> We say that a family $(A_{i}:i\in I)$ of events indexed by a countable set $I$ is <span style="color:#0088ff">independent</span> if, for all finite subsets $J\subseteq I$,
> $$
> \mathbb{P}\left( \bigcap_{i\in J}^{}A_{i} \right) =\prod_{i\in J}^{}\mathbb{P}(A_{i}).
> $$

> [!definition] Definition (Independence of $\sigma$-algebras)
> We say that a family $(\mathcal{A}_{i}:i\in I)$ of sub-$\sigma$-algebras of $\mathcal{F}$ indexed by a countable set $I$ is <span style="color:#0088ff">independent</span> if the family $(A_{i}:i\in I)$ of events is independent for all $A_{i}\in \mathcal{A}_{i}$ for all $i$.

> [!example] Example (Independent coin flips)
> Consider the result of two independent fair coin flips: $\Omega=\{HH,HT,TH,TT\}$, all with equal probability. Then, $\mathcal{F}=\mathcal{P}(\Omega)$ with the obvious measure $\mathbb{P}$. Then, the events "the first coin lands heads" and "the second coin lands tails" are independent: $\mathbb{P}(\{HH, HT\})\cdot\mathbb{P}(\{HT,TT\})=\mathbb{P}(\{HT\})$. Further, the sub $\sigma$-algebras 
> $$
>     \begin{align*}
>         \mathcal{F}_1&=\{\emptyset,\{HH,HT\},\{TH,TT\},\{HH,HT,TH,TT\}\}\\
>         \mathcal{F}_2&=\{\emptyset,\{HH,TH\},\{HT,TT\},\{HH,HT,TH,TT\}\}
>     \end{align*}
> $$
> which correspond to the results of the respective coin flips are independent. Notice how sub $\sigma$-algebras correspond to coarser partitions of possible events.

> [!question]
> Let $(A_{n}:n\in \mathbb{N})$ be a sequence of events in a probability space $\Omega$. Show that the events $A_{n}$ are independent if and only if the $\sigma$-algebras $\sigma(A_{n})=\{ \varnothing,A_{n},A_{n}^{c},\Omega \}$ are independent.

> [!theorem] Theorem ($\pi$-system independence check)
> Let $\mathcal{A}_{1}$ and $\mathcal{A}_{2}$ be $\pi$-systems contained in $\mathcal{F}$ and suppose that
> $$
> \mathbb{P}(A_{1}\cap A_{2})=\mathbb{P}(A_{1})\mathbb{P}(A_{2})
> $$
> whenever $A_{1}\in \mathcal{A}_{1}$ and $A_{2}\in \mathcal{A}_{2}$. Then $\sigma(\mathcal{A}_{1})$ and $\sigma(\mathcal{A}_{2})$ are independent.

> [!proof]-
> Fix some $A_{1}$ and define for $A\in \mathcal{F}$ the two measures
> $$
> \mu(A)=\mathbb{P}(A_{1}\cap A),\quad \nu(A)=\mathbb{P}(A_{1})\mathbb{P}(A).
> $$
> These agree on the $\pi$-system $\mathcal{A}_2$, with $\mu(\Omega)=\nu(\Omega)=\mathbb{P}(A_{1})<\infty$. Therefore, by [[Uniqueness of Extensions|uniqueness of extension]], we know for all $A_{2}\in\sigma(\mathcal{A}_{2})$ that
> $$
> \mathbb{P}(A_{1}\cap A_{2})=\mu(A_{2})=\nu(A_{2})=\mathbb{P}(A_{1})\mathbb{P}(A_{2}).
> $$
> Now fix an $A_{2}\in\sigma(\mathcal{A}_{2})$ and repeat the argument to show that $\mathbb{P}(A_{1}\cap A_{2})=\mathbb{P}(A_{1})\mathbb{P}(A_{2})$ for all $A_{1}\in\sigma(\mathcal{A}_{1})$.

---

**Next:** [[Borel-Cantelli Lemmas]]
