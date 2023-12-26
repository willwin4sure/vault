Given a sequence of events, we may want to ask about the probability that infinitely many of them occur.

> [!definition] Definition (limsup and liminf of sets)
> Given $(A_{n}:n\in \mathbb{N})$, we define
> $$
> \limsup_{ n }A_{n}=\bigcap_{n}^{}\bigcup_{m\geq n}^{}A_{m},\quad
> \liminf_{ n }A_{n}=\bigcup_{n}^{}\bigcap_{m\geq n}^{}A_{m}.
> $$
> Sometimes, we write $\{ A_{n} \text{ infinitely often} \}:=\limsup_{ n }A_{n}$, since $\omega \in \limsup_{ n }A_{n}$ if and only if $\omega \in A_{n}$ for infinitely many $n$. Similarly, we write $\{ A_{n}\text{ eventually} \}:=\liminf_{ n }A_{n}$, since $\omega \in \liminf_{ n }A_{n}$ if and only if $\omega \in A_{n}$ for all $n>N$, for some $N$. We abbreviate these to i.o. and ev., respectively.

The first Borel-Cantelli lemma states that if the sum of the probabilities of all events is finite, then almost surely only finitely many of them occur.

The second Borel-Cantelli lemma states that if the sum of the probabilities of all events if infinite, and all the events are independent, then almost surely infinitely many of them occur.

> [!theorem] Theorem (Borel-Cantelli lemmas)
> 1. If $\sum_{n}^{}\mathbb{P}(A_{n})<\infty$, then $\mathbb{P}(A_{n}\text{ i.o.})=0$.
> 2. If $(A_{n}:n\in \mathbb{N})$ are independent and $\sum_{n}^{}\mathbb{P}(A_{n})=\infty$, then $\mathbb{P}(A_{n}\text{ i.o.})=1$.

> [!proof]-
> 1. For the first lemma, check that
> $$
> \mathbb{P}(A_{n}\text{ i.o.})\leq \mathbb{P}\left( \bigcup_{m\geq n}^{}A_{m} \right)\leq \sum_{m\geq n}^{}\mathbb{P}(A_{m})\to 0
> $$
> as $n\to \infty$.
> 2. For the second lemma, we use the inequality $1-x\leq e^{-x}$. The events $(A_{n}^{c})$ are also independent. If we set $a_{n}=\mathbb{P}(A_{n})$, then for all $n$ and $N\geq n$,
> $$
> \mathbb{P}\left( \bigcap_{m=n}^{N}A_{m}^{c} \right)=\prod_{m=n}^{N}(1-a_{m})\leq \exp \left( -\sum_{m=n}^{N}a_{m} \right) \to 0
> $$
> as $N\to \infty$. In other words, the probability that after some point $n$, no other events occur, is 0. Hence $\mathbb{P}\left( \bigcap_{m\geq n}^{} A_{m}^{c} \right)=0$ for all $n$, so $\mathbb{P}(A_{n}\text{ i.o.})=1-\mathbb{P}\left( \bigcup_{n}^{}\bigcap_{m\geq n}^{}A_{m}^{c} \right)=1$.

---

**Next:** [[⛺Measures Homepage]]