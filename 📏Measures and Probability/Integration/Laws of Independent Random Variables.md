Recall that a family $(X_{i})$ of random variables on $(\Omega,\mathcal{F},\mathbb{P})$ is said to be [[Random Variables#^1182a1|independent]] if the family of $\sigma$-algebras $(\sigma(X_{i}))$ is independent.

> [!proposition]
> Let $X_{1},\dots,X_{n}$ be random variables on $(\Omega,\mathcal{F},\mathbb{P})$, with values in $(E_{1},\mathcal{E}_{1}),\dots,(E_{n},\mathcal{E}_{n})$, say. Set $E=E_{1}\times \cdots \times E_{n}$ and $\mathcal{E}=\mathcal{E}_{1}\otimes \cdots\otimes\mathcal{E}_{n}$. Consider the function $X:\Omega\to E$ given by $X(\omega)=(X_{1}(\omega),\dots,X_{n}(\omega))$. Then $X$ is $\mathcal{E}$-measurable. Moreover, the following are equivalent:
> 
> 1. $X_{1},\dots,X_{n}$ are independent;
> 2. $\mu_{X}=\mu_{X_{1}}\otimes \cdots\otimes \mu_{X_{n}}$;
> 3. for all bounded measurable functions $f_{1},\dots,f_{n}$ we have
> $$
> \mathbb{E}\left( \prod_{k=1}^{n}f_{k}(X_{k}) \right) =\prod_{k=1}^{n}\mathbb{E}\left( f_{k}(X_{k}) \right) .
> $$ 

> [!remark]
> Statement 3 can also be replaced with "nonnegative measurable functions".

> [!proof]-
> Set $\nu=\mu_{X_{1}}\otimes \cdots\otimes \mu_{X_{n}}$. Consider the $\pi$-system $\mathcal{A}=\left\{  \prod_{k=1}^{n}A_{k}:A_{k}\in \mathcal{E}_{k}  \right\}$. 
> 
> First, we show that 1 implies 2. For all $A\in \mathcal{A}$ we have that
> $$
> \mu_{X}(A)=\mathbb{P}(X \in \mathcal{A})=\mathbb{P}\left( \bigcap_{k=1}^{n}\{ X_{k}\in A_{k} \} \right) =\prod_{k=1}^{n}\mathbb{P}(X_{k}\in A_{k})=\prod_{k=1}^{n}\mu_{X_{k}}(A_{k})=\nu(A),
> $$
> and since $A$ generates $\mathcal{E}$, $\mu=\nu$ on $\mathcal{E}$ by uniqueness of extensions.
> 
> Next, we show that 2 implies 3. By [[Fubini-Tonelli Theorem|Fubini-Tonelli]], we can convert the multivariate integral into iterated integrals, which then factor:
> $$
> \mathbb{E}\left( \prod_{k=1}^{n}f_{k}(X_{k}) \right) =\int _{E}\prod_{k=1}^{n}f_{k}(x_{k}) \, \mu_{X_{k}}(dx_{k})=\prod_{k=1}^{n}\int _{E_{k}}f_{k}(x_{k}) \, \mu_{X_{k}}(dx_{k})=\prod_{k=1}^{n}\mathbb{E}(f_{k}(X_{k})),  
> $$
> as desired.
> 
> Finally, 3 implies 1 since we can take $f_{k}=\mathbf{1}_{A_{k}}$ over any $A_{k}\in \mathcal{E}_{k}$.

---

**Next:** [[⛺Integration Homepage]]