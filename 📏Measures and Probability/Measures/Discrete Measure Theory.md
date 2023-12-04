---

---
If you've heard of probability mass functions, discrete measure theory is essentially equivalent.

Suppose that $E$ is countable, and let $\mathcal{E}=\mathcal{P}(E)$ be the set of all subsets of $E$.

>[!definition] Definition (Mass function)
>A <span style="color:#0088ff">mass function</span> is any function $m:E\to[0,\infty]$. If $\mu$ is a measure on $(E,\mathcal{E})$, then by countable additivity,
>$$\mu(A)=\sum_{x\in A}\mu(\{x\})$$
>for all $A\subseteq E$. Therefore, discrete measures are in one-to-one correspondence with mass functions, given by
>$$\mu(\{x\})=m(x),\qquad\mu(A)=\sum_{x\in A}m(x).$$

So, discrete measure theory is quite boring. However, it is essentially the only form of measure theory where we can specify a nontrivial measure explicitly. In particular, general $\sigma$-algebras are not amenable to an explicit presentation which allows us to make such a specification.

Instead, we will specify the values of the measure on a [[smaller set of subsets]] which can generate the full $\sigma$-algebra.

---

**Next:** [[Carathéodory's Extension Theorem]]