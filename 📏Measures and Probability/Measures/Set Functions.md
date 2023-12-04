In this section, we weaken our concept of measures.

>[!definition] Definition (Set function)
>Let $\mathcal{A}$ be any set of subsets of $E$ containing the empty set $\varnothing$. A <span style="color:#0088ff">set function</span> is a function $\mu:\mathcal{A}\to[0,\infty]$ with $\mu(\varnothing)=0$.

^b94a95

We give names to some of the nice properties that a set function can have.

>[!definition] Definition (Set function properties)
>Work with a [[Set Functions#^b94a95|set function]] $\mu$ on $\mathcal{A}$. 
>* $\mu$ is <span style="color:#0088ff">increasing</span> if whenever $A\subseteq B$ are in $\mathcal{A}$,
>$$\mu(A)\leq\mu(B).$$
>* $\mu$ is <span style="color:#0088ff">additive</span> if for all disjoint sets $A,B\in\mathcal{A}$ with $A\sqcup B\in\mathcal{A}$,
>$$\mu(A\sqcup B)=\mu(A)+\mu(B).$$
>* $\mu$ is <span style="color:#0088ff">countably additive</span> if for all sequences of disjoint sets $(A_n:n\in\mathbb{N})\in\mathcal{A}$ with $\bigsqcup_{n}A_n\in\mathcal{A}$,
>$$\mu\left(\bigsqcup_{n}A_n\right)=\sum_{n}\mu(A_{n}).$$
>* $\mu$ is <span style="color:#0088ff">countably subadditive</span> if for all sequences $(A_n:n\in\mathbb{N})\in\mathcal{A}$ with $\bigcup_{n}A_n\in\mathcal{A}$,
>$$\mu\left(\bigcup_{n}A_n\right)\leq \sum_{n}\mu(A_n).$$

^164613

In this language, a measure is a countably additive set function on a $\sigma$-algebra.

>[!question]
>Show that a countably additive set function on a ring is additive, increasing, and countably subadditive.

---

**Next:** [[Carathéodory's Extension Theorem]]


