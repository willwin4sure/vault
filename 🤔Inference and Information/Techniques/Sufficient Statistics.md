> Distilling the data without loss.

Recall the definition of a [[The Likelihood Ratio Test#^8317c7|statistic]] $\boldsymbol{\mathsf{t}}=\mathbf{t}(\boldsymbol{\mathsf{y}})$, which is a random variable that is a deterministic function of the random observation $\boldsymbol{\mathsf{y}}$.
## Sufficiency

> [!definition] (Sufficient statistic)
> A statistic $\mathbf{t}(\bullet)$ is ==sufficient== with respect to a model family $\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x}) : \mathbf{x}\in \mathcal{X} \}$ if $p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{t}}}(\bullet|\bullet;\mathbf{x})$ does not depend on $\mathbf{x}\in \mathcal{X}$, i.e. $p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{t}}}(\bullet|\bullet;\mathbf{x}_{1})=p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{t}}}(\bullet|\bullet;\mathbf{x}_{2})$ for all $\mathbf{x}_{1},\mathbf{x}_{2}\in \mathcal{X}$. 

> [!idea]
> Once we know the value of $\boldsymbol{\mathsf{t}}$, any remaining uncertainty about $\boldsymbol{\mathsf{y}}$ does not depend on $\mathbf{x}$. In other words, after you know $\mathbf{t}$, $\mathbf{x}$ does not affect the distribution of $\boldsymbol{\mathsf{y}}$.

Therefore, dropping $\boldsymbol{\mathsf{y}}$ and just keeping $\boldsymbol{\mathsf{t}}$ is *sufficient* for any inferences about $\mathbf{x}$. 

> [!theorem] (Likelihood characterization)
> A statistic $\mathbf{t}(\bullet)$ is sufficient with respect to a model family $\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x}) : \mathbf{x}\in \mathcal{X} \}$ if and only if
> $$
> \frac{L_{\mathbf{y}}(\mathbf{x})}{L_{\mathbf{t}}(\mathbf{x})}=\frac{p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})}{p_{\boldsymbol{\mathsf{t}}}(\mathbf{t}(\mathbf{y});\mathbf{x})}
> $$
> is not a function of $\mathbf{x}\in \mathcal{X}$ for every $\mathbf{y}\in \mathcal{Y}$.

> [!proof]- Trivial
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})=\sum_{\mathbf{t}}^{}p_{\boldsymbol{\mathsf{y}},\boldsymbol{\mathsf{t}}}(\mathbf{y},\mathbf{t};\mathbf{x})=p_{\boldsymbol{\mathsf{y}},\boldsymbol{\mathsf{t}}}(\mathbf{y},\mathbf{t}(\mathbf{y});\mathbf{x})=p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{t}}}(\mathbf{y}|\mathbf{t}(\mathbf{y});\mathbf{x})p_{\boldsymbol{\mathsf{t}}}(\mathbf{t}(\mathbf{y});\mathbf{x}).
> $$

Here is another characterization:

> [!theorem] (Neyman Factorization Theorem)
> A statistic $\mathbf{t}(\bullet)$ is sufficient with respect to the model family $\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x}) : \mathbf{x}\in \mathcal{X} \}$ if and only if there exist functions $a(\bullet,\bullet)$ and $b(\bullet)$ such that
> $$
> p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})=a(\mathbf{t}(\mathbf{y}),\mathbf{x})b(\mathbf{y})
> $$
> for all $\mathbf{x}\in \mathcal{X}$ and $\mathbf{y}\in \mathcal{Y}$.

> [!proof] Trivial

Alright it's about time for some examples.

> [!example] ([[Single-Parameter Exponential Families#^2a98de|Natural statistic]] of an exponential family)
> This is the only part of $\mathbf{y}$ that interacts with $\mathbf{x}$. 

> [!example] (Samples from uniform)
> Given i.i.d. samples $Y_{1},\dots,Y_{n}\sim \mathtt{U}([0,x])$, the $n$-th order statistic $T(Y_{1},\dots,Y_{n})=\max_{i}Y_{i}$ is a sufficient statistic for inferences about $x$.

> [!example] (Samples from $n$-dimensional Gaussian)
> Given i.i.d. samples $Y_{1},\dots,Y_{n}\sim \mathtt{N}(x,1)$, the sample mean $T(Y_{1},\dots,Y_{n})=\frac{1}{n}\sum_{i=1}^{n}Y_{i}$ is a sufficient statistic for inferences about $x$.

## Minimality

Given a sufficient statistic $\mathbf{t}$, we could augment it with any additional information and it would still be sufficient. Hell, even $\mathbf{y}$ itself is sufficient!

> [!definition] (Minimal sufficient statistic)
> A sufficient statistic $\mathbf{s}$ is ==minimal== if for any other sufficient statistic $\mathbf{t}$, there exists a function $\mathbf{g}(\bullet)$ such that $\mathbf{s}=\mathbf{g}(\mathbf{t})$.

These are not unique; consider any injective function from a minimal statistic. 

> [!example] (Likelihood ratio for binary hypothesis testing)
> For a model $p_{\boldsymbol{\mathsf{y}}}(\bullet;x)$ where the parameter $x \in \mathcal{X}=\{ H_{0},H_{1} \}$, the familiar [[The Likelihood Ratio Test#^48ae30|likelihood ratio]] 
> $$
> L(\mathbf{y})=\frac{p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};H_{1})}{p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};H_{0})}
> $$
> is a minimal sufficient statistic.

There is no systematic method of determining whether a sufficient statistic is minimal. However, the notion of completeness implies minimality (but not the other way around).

> [!definition]
> A sufficient statistic $\mathbf{t}$ is ==complete== if any function $\phi(\bullet):\mathbf{t}(\mathcal{Y})\to \mathbb{R}$ satisfying
> $$
> \mathbb{E}[\phi(\mathbf{t}(\boldsymbol{\mathsf{y}}))]=0
> $$
> for all $\mathbf{x}\in \mathcal{X}$ satisfies $\phi(\mathbf{t}(\boldsymbol{\mathsf{y}}))=0$ almost surely.

> [!theorem]
> A sufficient statistic $\mathbf{t}$ is minimal if it is complete.

> [!proof]-
> Suppose $\mathbf{t}$ is complete and $\mathbf{s}$ is minimal. Then, we can write $\mathbf{s}=\mathbf{g}(\mathbf{t})$, so
> $$
> \mathbb{E}[\boldsymbol{\mathsf{t}}|\boldsymbol{\mathsf{s}}]=f(\boldsymbol{\mathsf{s}}). 
> $$
> Therefore, if we define $\phi(\mathbf{t})=\mathbf{t}-\mathbb{E}[\boldsymbol{\mathsf{t}}|\boldsymbol{\mathsf{s}}=\mathbf{s}]$, then its expected value is zero which means that by the completeness of $\mathbf{t}$, $\mathbf{t}=\mathbb{E}[\boldsymbol{\mathsf{t}}|\boldsymbol{\mathsf{s}}=\mathbf{s}]=f(\mathbf{s})$ almost surely. Yet $\mathbf{s}$ is minimal, so $\mathbf{t}$ is as well.

---

**Next:** [[Some Useful Inequalities]]