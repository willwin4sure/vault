The first goal of this section is to define the product measure. This will allow us to integrate over multiple variables.

> [!definition] Definition (Product $\sigma$-algebra)
> Let $(E_{1},\mathcal{E}_{1},\mu_{1})$ and $(E_{2},\mathcal{E}_{2},\mu_{2})$ be *finite* measure spaces. The set
> $$
> \mathcal{A}=\{ A_{1}\times A_{2} : A_{1}\in \mathcal{E}_{1},A_{2}\in \mathcal{E}_{2} \}
> $$
> is a $\pi$-system of subsets of $E=E_{1}\times E_{2}$. We define the ==product $\sigma$-algebra== as
> $$
> \mathcal{E}=\mathcal{E}_{1}\otimes \mathcal{E}_{2}=\sigma(\mathcal{A}).
> $$

^40b30e

Let's prove some lemmas that ought to be true.

> [!claim] Claim (Measurable functions are component-wise measurable)
> Let $f:E\to \mathbb{R}$ be $\mathcal{E}$-measurable. Then, for all $x_{1}\in E_{1}$, the function $x_{2}\mapsto f(x_{1},x_{2}):E_{2}\to \mathbb{R}$ is $\mathcal{E}_{2}$-measurable.

> [!proof]-
> We apply the [[Measurable Functions#^f79049|monotone class theorem]]. Let $\mathcal{V}$ be the set of bounded $\mathcal{E}$-measurable functions for which the conclusion holds. Then $\mathcal{V}$ is a vector space, and it contains the indicator function $\mathbf{1}_{A}$ of every set $A\in \mathcal{A}$ (by definition; these are the rectangle sets). It also satisfies the property that for any $f_{n}\in \mathcal{V}$ if $f$ is bounded with $0\leq f_{n}\uparrow f$, then also $f\in V$. 
> 
> The monotone class theorem guarantees that $\mathcal{V}$ contains all bounded $\mathcal{E}$-measurable functions. This finishes, as any arbitrary $\mathcal{E}$-measurable function $f$ can be partitioned by its range into countably many bounded $\mathcal{E}$-measurable functions. Then, check that the preimage of any measurable set is measurable by taking countable unions.

> [!claim] Claim (Marginalization works)
> Let $f$ be a bounded or nonnegative measurable function on $E$. Define for $x_{1}\in E_{1}$
> $$
> f_{1}(x_{1})=\int _{E_{2}}f(x_{1},x_{2}) \, \mu_{2}(dx_{2}). 
> $$
> If $f$ is bounded, then $f_{1}:E\to \mathbb{R}$ is a bounded $\mathcal{E}_{1}$-measurable function. On the other hand, if $f$ is nonnegative, then $f_{1}:E_{1}\to[0,\infty]$ is also an $\mathcal{E}_{1}$-measurable function.

> [!proof]-
> We can also apply the monotone class theorem, as in the previous proof. The finiteness of $\mu_{2}$ is needed for the boundedness of $f_{1}$ when $f$ is bounded.

Finally, we will define the product measure. We do this in the natural way: specify what we want on the $\pi$-system of rectangle sets, and then extend.

> [!theorem] Theorem (Product measure)
> There exists a unique measure $\mu=\mu_{1}\otimes \mu_{2}$ on $\mathcal{E}$ such that
> $$
> \mu(A_{1}\times A_{2})=\mu_{1}(A_{1})\mu_{2}(A_{2})
> $$
> for all $A_{1}\in \mathcal{E}_{1}$ and $A_{2}\in \mathcal{E}_{2}$.

^8d56b9

> [!proof]-
> Uniqueness holds because $\mathcal{A}$ is a $\pi$-system generating $\mathcal{E}$, so it is enough to [[Pi and Lambda Systems#^280456|specify the entire measure uniquely]]. For existence, by the lemmas, we can directly define
> $$
> \mu(A)=\int _{E_{1}}\left( \int _{E_{2}}\mathbf{1}_{A}(x_{1},x_{2}) \, \mu_{2}(dx_{2})  \right)  \, \mu_{1}(dx_{1})
> $$
> and use [[Monotone Convergence Theorem#^129ee0|monotone convergence]] to see that $\mu$ is countably additive.

Finally, we can also swap the order:

> [!proposition] Proposition (Swapping product order does what you expect)
> Let $\hat{\mathcal{E}}=\mathcal{E}_{2}\otimes \mathcal{E}_{1}$ and $\hat{\mu}=\mu_{2}\otimes \mu_{1}$. For a function $f$ on $E_{1}\times E_{2}$, write $\hat{f}$ for the function on $E_{2}\times E_{1}$ given by $\hat{f}(x_{2},x_{1})=f(x_{1},x_{2})$. Let $f$ be a nonnegative $\mathcal{E}$-measurable function. Then $\hat{f}$ is a nonnegative $\hat{\mathcal{E}}$-measurable function and $\hat{\mu}(\hat{f})=\mu(f)$.

---

**Next:** [[Fubini-Tonelli Theorem]]