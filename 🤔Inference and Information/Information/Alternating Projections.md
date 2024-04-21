Here, we provide an information geometric viewpoint on the [[The Expectation Maximization (EM) Algorithm|EM algorithm]]. In particular, it is equivalent to an alternating projections algorithm on the probability simplex.

## Maximum Likelihood as a Reverse I-Projection

We will focus on the case of i.i.d. discrete data and a nonrandom parameter. Our observations (the incomplete data) comprise the sequence
$$
\boldsymbol{\mathsf{y}}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N}).
$$
Let $p_{\mathsf{y}}(\bullet;x)$ be our model for each $\mathsf{y}_{n}$, so we can write the distribution for $\boldsymbol{\mathsf{y}}$ as
$$
p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\prod_{n=1}^{N}p_{\mathsf{y}}(y_{n};x).
$$
Our goal is to produce the ML estimate of $x$, which is based on the log-likelihood
$$
\tilde{\ell}(x;\mathbf{y})=\frac{1}{N}\log p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};x)=\frac{1}{N}\sum_{n=1}^{N}\log p_{\mathsf{y}}(y_{n};x).
$$
Now, in such i.i.d. models, the *ordering* of our data carries no information about $x$. Therefore, only the frequency array of the outputs matters.

> [!definition]
> The ==empirical distribution== or ==type== of the data is defined via
> $$
> \hat{p}_{\mathsf{y}}(b;\mathbf{y})=\frac{1}{N}\sum_{n=1}^{N}\mathbf{1}_{b=y_{n}},\quad b\in \mathcal{Y}.
> $$
> This is just writing $\hat{p}_{\boldsymbol{\mathsf{y}}}(b;\mathbf{y})$ as the fraction of times the symbol $b$ appears in the sequence $\mathbf{y}$.

This is a sufficient statistic for our data.

Remember when we introduced the [[Information Projection and Pythagoras' Theorem#^936604|I-projection]] we [[Information Projection and Pythagoras' Theorem#^fafb8a|remarked]] that it was the "morally incorrect" thing to do for inference. In particular, it took a distribution $q$ and projected it to $p^{*}\in \mathcal{P}$ by considering $p^{*}=\arg\min_{p \in \mathcal{P}}D(p\parallel q)$, which is **treating $q$ as our model and seeing which $p$ we'd be least surprised to see**.

However, in inference, we see some data $q$ and want to attribute it to some element in the set of models $\mathcal{P}$ which would make us least surprised to observe $q$, i.e. the maximum likelihood estimator.

> [!claim] (M-projection)
> Let the set of models be
> $$
> \mathcal{P}=\{ p_{\mathsf{y}}(\bullet;x) : x \in \mathcal{X} \}.
> $$
> Then, the ML estimate can be written in the form
> $$
> \hat{x}_{ML}(\mathbf{y})=\arg\min_{p \in \mathcal{P}}D(\hat{p}_{\mathsf{y}}(\bullet;\mathbf{y})\parallel p)=\arg\min_{a}D(\hat{p}_{\mathsf{y}}(\bullet;\mathbf{y})\parallel p_{\mathsf{y}}(\bullet;a)),
> $$
> which we call a reverse I-projection, or a ==M-projection==.

## Expectation-Maximization as Alternating Projections

Recall that the EM algorithm has the following steps for $\ell=1,2,\dots$

1. **E-step:** Compute $U(x,\hat{x}^{(\ell-1)})$, where
$$
U(x,x')=\sum_{\mathbf{z}\in \mathcal{Z}^{N}}^{}p_{\boldsymbol{\mathsf{z}}|\boldsymbol{\mathsf{y}}}(\mathbf{z}|\mathbf{y};x')\log p_{\boldsymbol{\mathsf{z}}}(\mathbf{z};x).
$$
2. **M-step:** Compute
$$
\hat{x}^{(\ell)}=\arg\max_{x}U(x,\hat{x}^{(\ell-1)}).
$$

