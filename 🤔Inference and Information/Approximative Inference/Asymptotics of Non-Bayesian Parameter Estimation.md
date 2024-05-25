We just explored the [[Asymptotics of Hypothesis Testing|asymptotics of hypothesis testing]]. Now it is time for [[⛺Estimation Homepage|parameter estimation]]. We start with the non-Bayesian case.

## Definitions

This section will feel a lot like intro statistics class. The setup is that we have some parameterized family of distributions
$$
\left\{ p_{\mathsf{y}}(\bullet;a)\in \mathcal{P}^{\mathcal{Y}} : a \in \mathcal{X} \right\}
$$
and some data $\mathsf{y}^{N}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})$ generated i.i.d. according to one of them. Then, $\hat{x}_{N}(\mathsf{y}^{N})$ generates some estimate of the underlying parameter $x$ based on the data.

Here is a basic requirement for this problem to be interesting:

> [!definition] (Identifiability)
> A parameter $x \in \mathcal{X}$ in family $\left\{ p_{\mathsf{y}}(\bullet;a)\in \mathcal{P}^{\mathcal{Y}} : a \in \mathcal{X} \right\}$ is called ==identifiable== if $D(p_{\mathsf{y}}(\bullet;x)\parallel p_{\mathsf{y}}(\bullet;x'))\neq 0$ whenever $x'\neq x$.

Here is a fairly minimal requirement on the strength of our estimator:

> [!definition] (Consistency)
> Let $\hat{x}(\mathsf{y}^{N})$ denote a sequence of estimates of a parameter $x \in \mathcal{X}$. Then, the sequence is ==weakly consistent== if
> $$
> \hat{x}_{N}(\mathsf{y}^{N})\xrightarrow[N\to \infty]{\mathbb{P}} x.
> $$
> If the convergence is almost surely, then we call it ==strongly consistent==.

A stronger requirement is that the distribution of the parameter follows a narrowing Gaussian distribution as $N$ increases.

> [!definition] (Asymptotic normality)
> A sequence of estimates $\hat{x}_{N}(\mathsf{y}^{N})$ is ==asymptotically normal== if
> $$
> \sqrt{ N }\left( \hat{\mathsf{x}}_{N} - \mathsf{x} \right) \xrightarrow[N\to \infty]{d} \mathtt{N}\left( 0, \frac{1}{\sigma^{2}(x)} \right) 
> $$
> for some $\sigma^{2}(x)$. 

An even stronger requirement requires the performance to match that of the [[The Cramér–Rao Bound|Cramér–Rao bound]]. Indeed, we call such a sequence ==asymptotically efficient== if it is asymptotically normal with $\sigma^{2}(x)=J_{\mathsf{y}}(x)$, analogous to [[Efficient Estimators|efficiency]] in the non-asymptotic case.

## ML Estimates

We turn to analyzing our estimator of choice. Note that the MLE minimizes the empirical divergence
$$
\hat{D}_{\boldsymbol{\mathsf{y}}}^{N}(a):=\frac{1}{N}\sum_{n=1}^{N}\log \frac{p_{\mathsf{y}}(\mathsf{y}_{n};x)}{p_{\mathsf{y}}(\mathsf{y}_{n};a)}.
$$
Now, note that the LLN implies that
$$
\hat{D}_{\boldsymbol{\mathsf{y}}}^{N}(a)\xrightarrow[N\to \infty]{\text{a.s.}} D\left( p_{\mathsf{y}}(\bullet;x)\parallel p_{\mathsf{y}}(\bullet;a) \right).
$$
This order of the divergence is in agreement with the [[Alternating Projections#^c8c715|M-projection]], since we are considering the MLE. Because of this, we should expect the MLE to be close to $x$, but it might not due to certain nonuniform convergence phenomena (e.g. in the case of parameters over infinite alphabets).

### Finite Alphabet Case

> [!theorem]
> Consider a model family with $|\mathcal{Y}|<\infty$ for a parameter set $\mathcal{X}$ such that $|\mathcal{X}|<\infty$, and let $\mathsf{y}_{1},\dots,\mathsf{y}_{N}$ be generated i.i.d. according to the model $p_{\mathsf{y}}(\bullet;x)$ for some identifiable $x \in \mathcal{X}$. Further, suppose that $p_{\mathsf{y}}(y;a)>0$ for all $y \in \mathcal{Y}$ and $a \in \mathcal{X}$. Then the MLE $\hat{x}_{N}(\mathsf{y}^{N})$ is weakly consistent.

Strong consistency also holds!

### General Case

For canonical exponential model families, it turns out that strong consistency and asymptotic efficiency both hold!

For the general case, Wald covers some sufficient conditions for strong consistency, while Cramér covers some sufficient conditions for asymptotic efficiency.

> [!theorem] (Wald)
> Consider a model family where $\mathcal{X}\subseteq \mathbb{R}$ and let $\mathsf{y}^{N}$ be generated i.i.d. according to the model $p_{\mathsf{y}}(\bullet;x)$ for some $x \in \mathcal{X}$. Then, if:
> 1. $\mathcal{X}$ is compact;
> 2. $p_{\mathsf{y}}(y;a)$ is continuous in $a \in \mathcal{X}$ for all $y \in \mathcal{Y}$;
> 3. $\hat{D}_{y}(a)$ is dominated by some integrable $D_{y}^{+}$ uniformly over $a \in \mathcal{X}$;
> 4. the parameter $x$ is identifiable;
>    
> then the MLE is strongly consistent.

> [!theorem] (Cramér)
> For an alphabet $\mathcal{Y}$, consider a model family $\left\{ p_{\mathsf{y}}(\bullet;a) \in \mathcal{P}^{\mathcal{Y}} : a \in \mathcal{X} \right\}$ for a parameter set $\mathcal{X} \subseteq \mathbb{R}$, and let $\mathsf{y}_{1},\dots,\mathsf{y}_{N}$ be i.i.d. according to the model $p_{\mathsf{y}}(\bullet;x)$ for some $x \in \mathcal{X}$. If
> 1. $\mathcal{X}$ is open;
> 2. $\frac{ \partial^{2} p_{\mathsf{y}}(y;a) }{ \partial a^{2} }$ exists and is continuous in $a \in \mathcal{X}$ for all $y \in \mathcal{Y}$, and we have the usual regularity condition
> $$
> \int_{\mathcal{Y}}\frac{ \partial  }{ \partial a } p_{\mathsf{y}}(y;a) \, dy = 0;
> $$
> 3. the second derivative of $\log p_{\mathsf{y}}(y;a)$ with respect to $a$ is dominated by some integrable $\ddot{\ell}_{y}^{+}$ uniformly over $a$ in a neighborhood of $x$;
> 4. $J_{\mathsf{y}}(x)>0$;
> 5. the MLE is strongly consistent;
> 
> then the MLE is asymptotically efficient.

### Mismatched Case

Here is a result for the case where the true model lies outside of the model family.

> [!theorem]
> Consider a model family $\mathcal{P}=\left\{ p_{\mathsf{y}}(\bullet;x)\in \mathcal{P}^{\mathcal{Y}} : x \in \mathcal{X} \right\}$ with $|\mathcal{Y}|<\infty$ for a parameter set $\mathcal{X}$ such that $|\mathcal{X}|<\infty$, and let $\mathsf{y}_{1},\dots,\mathsf{y_N}$ be i.i.d. according to $q_{\mathsf{y}}\not\in \mathcal{P}$. Further suppose that $q_{\mathsf{y}}(y)>0$ and $p_{\mathsf{y}}(y;x)>0$ for all $y \in \mathcal{Y}$ and $x \in \mathcal{X}$, and that
> $$
> x_{*}=\arg\min_{x \in \mathcal{X}}D\left( q_{\mathsf{y}} \parallel p_{\mathsf{y}|\mathsf{x}}(\bullet | x) \right) 
> $$
> is unique. Then the MLE converges in probability to $x_{*}$.

It is what you expect: we converge to the M-projection of $q$ onto $\mathcal{P}$.

> [!warning] (Hodges' estimator)

---

**Next:** [[Asymptotics of Bayesian Parameter Estimation]]