We just developed [[Maximally Ignorant Priors|maximally ignorant priors]], which are more easily computable than least informative priors. These are still challenging computationally since they do not support efficient belief updates, e.g. if as more data is gradually acquired.

In this section, we develop ==conjugate prior families==, which are particularly amenable to both design and implementation. 

## Permutation-Invariant Models

We impose some additional structure on the models that we consider, which will allow us to develop some important results.

> [!definition] (Conditionally-i.i.d. models)
> Consider observations of the form $\boldsymbol{\mathsf{y}}=[\mathsf{y}_{1},\dots,\mathsf{y}_{N}]^{T}$ over the alphabet $\mathcal{Y}^{N}$, and a parameter $\mathsf{x}$ over alphabet $\mathcal{X}$. Then, the associated model $p_{\boldsymbol{\mathsf{y}}|\mathsf{x}}$ is ==conditionally-i.i.d.== if for every $x \in \mathcal{X}$ and every $y_{1},\dots,y_{N} \in \mathcal{Y}^{N}$, we have
> $$
> p_{\mathsf{y}_{1},\dots,\mathsf{y}_{N}|\mathsf{x}}(y_{1},\dots,y_{N}|x)=\prod_{n=1}^{N}p_{\mathsf{y}|\mathsf{x}}(y_{n}|x)
> $$
> for some $p_{\mathsf{y}|\mathsf{x}}$.

This is exactly what you expect: you have some parameter $x$ and then all our samples come i.i.d. from the same distribution. These have the ==permutation-invariance== property: the order of the data does not affect the probability that it is generated.

> [!definition] (Exchangeability)
> A sequence of random variables $\mathsf{y}_{1},\dots \mathsf{y}_{N}$ is ==exchangeable== if for every permutation $\sigma \in S^{N}$ of the indices and for every $y_{1},\dots,y_{N} \in \mathcal{Y}^{N}$,
> $$
> p_{\mathsf{y}_{1},\dots,\mathsf{y}_{N}}(y_{1},\dots,y_{N})=p_{\mathsf{y}_{\sigma(1)},\dots y_{\sigma(N)}}(y_{1},\dots,y_{N}),
> $$
> i.e. the joint distribution of $\mathsf{y}_{1},\dots,\mathsf{y}_{N}$ is permutation-invariant. Moreover, we say a sequence of random variables $\mathsf{y}_{1},\mathsf{y}_{2},\dots$ is ==infinitely exchangeable== if it is exchangeable for every finite $N$.

Obviously all i.i.d. random variables are exchangeable. Clearly all observations from a conditionally-i.i.d. model are also exchangeable, since they are a mixture of i.i.d. random variables. 

It turns out we can also go backwards: any infinitely exchangeable sequence of random variables is expressible as the observations of a conditionally-i.i.d. model.

> [!theorem] (Characterization of infinitely exchangeable sequences)
> The sequence of random variables $\mathsf{y}_{1},\mathsf{y}_{2},\dots$ over $\mathcal{Y}^{N}$ is infinitely exchangeable if and only if for every $N$ there exists an alphabet $\mathcal{X}$, distribution $p_{\mathsf{x}}$, and model $p_{\mathsf{y}_{1},\dots \mathsf{y}_{N}|\mathsf{x}}$ that is conditionally i.i.d. such that
> $$
> p_{\mathsf{y}_{1},\dots \mathsf{y}_{N}}(y_{1},\dots,y_{N})=\int p_{\mathsf{y}_{1},\dots,\mathsf{y}_{N}}(y_{1},\dots,y_{N}|x)p_{\mathsf{x}}(x) \, dx,
> $$
> i.e. it is a mixture model with appropriate prior.

## Conjugate Prior Families

> [!idea]
> We want to support efficient belief updates.

In general, Bayesian updates are computationally infeasible to perform since we need to rescale the entire prior by the likelihoods and then renormalize (potentially a difficult numerical integration).

Some setup: given a prior belief $p_{\mathsf{x}}(\bullet)$, when data $\boldsymbol{\mathsf{y}}=\mathbf{y}$ becomes available, we update it via Bayes' rule into $p_{\mathsf{x}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y})=T_{\mathbf{y}}(p_{\mathsf{x}}(\bullet))$. Here, we use $T_{\mathbf{y}}$ to denote our belief update operator.

> [!definition] (Conjugate prior family)
> Let
> $$
> \mathcal{Q}=\{ q(\bullet;\boldsymbol{\theta}) : \boldsymbol{\theta} \in \Theta \subseteq \mathbb{R}^{K} \}
> $$
> denote a family of distributions specified by some dimension $K$, region $\Theta \subseteq \mathbb{R}^{K}$, and parameterized form $q$, where the mapping from $\Theta$ to $\mathcal{Q}$ defined by $q$ is homeomorphic. Then, $\mathcal{Q}$ is a ==conjugate prior family== for the conditionally i.i.d. model
> $$
> p_{\boldsymbol{\mathsf{y}}|\mathsf{x}}(\mathbf{y}|x)=\prod_{i=1}^{N}p_{\mathsf{y}|\mathsf{x}}(y_{i}|x)
> $$
> if for every $y\in \mathcal{Y}$ we have that $p_{\mathsf{x}|\mathsf{y}}(\bullet|y)\in \mathcal{Q}$ whenever $p_{\mathsf{x}}(\bullet)\in \mathcal{Q}$. 

This says exactly what you want: if your belief is already inside the family $\mathcal{Q}$, then after an update of the model, it still lies inside the family $\mathcal{Q}$.

> [!notation]
> If $\boldsymbol{\theta}_{0}\in\Theta$ denotes the parameter specifying $p_{\mathsf{x}}$ in $\mathcal{Q}$, then the parameter specifying $p_{\mathsf{x}|\mathsf{y}}(\bullet|y)$ based on an observation $y$ we denote using $\boldsymbol{\theta}(y;\boldsymbol{\theta}_{0})$.

The prior we *start* with, before any observations are made, is governed by the parameter $\boldsymbol{\theta}_{0}$. We call this a ==hyperparameter==, and it arises out of domain knowledge in our problem.

> [!claim] (Characterization of conjugate prior families)
> A collection $\mathcal{Q}$ is a conjugate prior family if and only if $p_{\mathsf{x}|\mathsf{y}_{1},\dots,\mathsf{y}_{N}}(\bullet|y_{1},\dots,y_{N})\in \mathcal{Q}$ whenever $p_{\mathsf{x}}(\bullet)\in \mathcal{Q}$ for every $N\geq 1$ and $y_{1},\dots,y_{N}\in \mathcal{Y}$.

> [!proof] Obvious

---

**Next:** [[Conjugate Priors and Sufficient Statistics]]