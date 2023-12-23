Probability theory is enriched by the significance attached to the notion of independence.

> [!definition] Definition (Independence of events)
> We say that a family $(A_{i}:i\in I)$ of events indexed by a countable set $I$ is <span style="color:#0088ff">independent</span> if, for all finite subsets $J\subseteq I$,
> $$
> \mathbb{P}\left( \bigcap_{i\in J}^{}A_{i} \right) =\prod_{i\in J}^{}\mathbb{P}(A_{i}).
> $$

> [!definition] Definition (Independence of $\sigma$-algebras)
> We say that a family $(\mathcal{A}_{i}:i\in I)$ of sub-$\sigma$-algebras of $\mathcal{F}$ indexed by a countable set $I$ is <span style="color:#0088ff">independent</span> if the family $(A_{i}:i\in I)$ of events is independent for all $A_{i}\in \mathcal{A}_{i}$ for all $i$.

[!example] Example (Independent coin flips)


> [!question]
> Let $(A_{n}:n\in \mathbb{N})$ be a sequence of events in a probability space $\Omega$. Show that the events $A_{n}$ are independent if and only if the $\sigma$-algebras $\sigma(A_{n})=\{ \varnothing,A_{n},A_{n}^{c},\Omega \}$ are independent.

---

**Next:** [[Borel-Cantelli Lemmas]]
