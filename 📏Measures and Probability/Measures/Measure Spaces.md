Intuitively, the goal of measure theory is to define the "size" of sets. Unfortunately, we encounter [[difficulties]] trying to simply assign a reasonable measure to, say, all subsets of $[0,1]$. Instead, we must restrict our attention to "nice" subsets.

Work over a set $E$. $\sigma$-algebras tell you what these "nice" subsets of $E$ are.

>[!definition] Definition ($\sigma$-algebra, Measurable space)
>A <span style="color:#0088ff">$\sigma$-algebra</span> $\mathcal{E}$ is a nonempty set of subsets of $E$ closed under complementation and countable unions.
>
>In other words, for all $A\in\mathcal{E}$ and sequences $(A_n:n\in\mathbb{N})$ in $\mathcal{E}$,
>$$A^c\in\mathcal{E}, \qquad\bigcup_{n}A_n\in\mathcal{E}.$$
>In this case, the pair $(E,\mathcal{E})$ is called a <span style="color:#0088ff">measurable space</span>. Any $A\in\mathcal{E}$ is then called a <span style="color:#0088ff">measurable set</span>.

^130382

>[!question]
>Show that $\emptyset\in\mathcal{E}$. Then, show that $\mathcal{E}$ is closed under countable intersection as well.

>[!notation]
>As Evan Chen points out in [Napkin](https://venhance.github.io/napkin/Napkin.pdf): this terminology is phonetically confusing, because it can be confused with "measure space" later. The way to think about is that "measur*able* spaces have a $\sigma$-algebra, so we *could* try to put a measure on it, but we *haven’t*, yet."

Alright, well now that we know which sets are potentially measurable, let's assign a measure to them!

>[!definition] Definition (Measure)
>A <span style="color:#0088ff">measure</span> $\mu$ on a [[Measure Spaces#^130382|measurable space]] $(E,\mathcal{E})$ is a function $\mu:\mathcal{E}\to[0,\infty]$ with $\mu(\emptyset)=0$ that satisfies <span style="color:#0088ff">countable additivity</span>, i.e. for any sequence $(A_n:n\in\mathbb{N})$ of disjoint elements of $\mathcal{E}$,
>$$\mu\left(\bigsqcup_{n}A_n\right)=\sum_n\mu(A_n).$$
>Then, the triple $(E,\mathcal{E},\mu)$ is called a <span style="color:#0088ff">measure space</span>.

^996cdc

>[!question]
>What would go wrong if we imposed *uncountable* additivity?

---

**Next:** [[Discrete Measure Theory]]