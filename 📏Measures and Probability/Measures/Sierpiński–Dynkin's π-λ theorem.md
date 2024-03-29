## Statement

>[!theorem] (Sierpiński–Dynkin's $\pi$-$\lambda$ Theorem)
>Let $\mathcal{A}$ be a [[Pi and Lambda Systems#^232a53|$\pi$-system]]. Then any [[Pi and Lambda Systems#^32fdbb|$\lambda$-system]] containing $\mathcal{A}$ contains also the $\sigma$-algebra generated by $\mathcal{A}$.

^002127


## Proof

Let $\mathcal{L}$ be the intersection of all $\lambda$-systems containing $\mathcal{A}$. Then $\mathcal{L}$ is itself a $\lambda$-system. It suffices to show that $\mathcal{L}$ is also a $\pi$-system, so it is a $\sigma$-algebra; hence it contains the $\sigma$-algebra generated by $\mathcal{A}$.

Let's work there in steps. Instead of showing directly that $\mathcal{L}$ is a $\pi$-system, we will define a set with the properties we want (i.e. is a $\pi$-system), and then show that it is a $\lambda$-system.

Consider
$$
\mathcal{L}'=\{ B\in\mathcal{L}:B\cap A\in\mathcal{L}\quad \forall A\in\mathcal{A} \}.
$$
In other words, this is the set of all subsets in $\mathcal{L}$ that stay inside $\mathcal{L}$ when intersected with stuff in $\mathcal{A}$. Since $\mathcal{A}$ itself is a $\pi$-system, $A\subseteq \mathcal{L}'$.

We show that $\mathcal{L}'$ is a $\lambda$-system. First, $E\in\mathcal{L}'$. Now, take $B_{1},B_{2}\in\mathcal{L}'$ such that $B_{1}\subseteq B_{2}$; then, for all $A\in\mathcal{A}$ we have
$$
(B_{2}\setminus B_{1})\cap A=(B_{2}\cap A)\setminus(B_{1}\cap A)\in\mathcal{L},
$$
so $B_{2}\setminus B_{1}\in\mathcal{L}'$. Finally, if we have a sequence $B_{n}\in\mathcal{L}'$ over $n\in\mathbb{N}$, and $B_{n}\uparrow B$, then for all $A\in\mathcal{A}$, we have
$$
B_{n}\cap A\uparrow B\cap A,
$$
so $B\cap A\in\mathcal{L}$ and $B\in\mathcal{L}'$. Hence $\mathcal{L}=\mathcal{L}'$.

Now, consider the set
$$
\mathcal{L}''=\{ B\in\mathcal{L}:B\cap A\in\mathcal{L}\quad\forall A\in\mathcal{L} \}.
$$
Since $\mathcal{L}'=\mathcal{L}$, we have that $\mathcal{A}\in\mathcal{L}''$. Once again, $\mathcal{L}''$ is a $\lambda$-system, so $\mathcal{L}''=\mathcal{L}$. Hence $\mathcal{L}$ is a $\pi$-system, which finishes.

>[!question]
>What would go wrong if $\mathcal{A}$ is not a $\pi$-system (i.e. some intersection is missing)?

---

**Next:** [[Uniqueness of Extensions]]