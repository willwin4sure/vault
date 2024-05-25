> The EM algorithm consists of alternating I- and M-projections.

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

^3b50bd

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

^c8c715

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
> [!idea]
> The E-step is saying: use our best guess for the parameters $x$, and take the distribution of $\boldsymbol{\mathsf{z}}$ conditioned on the observation $\boldsymbol{\mathsf{y}}=\mathbf{y}$. Then, compute the log-likelihood of the distribution of $\boldsymbol{\mathsf{z}}$ for some $x$.
> 
> The M-step is just saying to pick an $x$ that maximizes this log-likelihood. 

We will assume our complete data is also i.i.d. and discrete-valued, i.e.
$$
\boldsymbol{\mathsf{z}}=(\mathsf{z}_{1},\mathsf{z}_{2},\dots,\mathsf{z}_{N})
$$
has elements that are drawn from the alphabet $\mathcal{Z}$ according to $p_{\mathsf{z}}(\bullet;x)$. Then, the actual observed data is obtained from the complete data via some deterministic mapping $y_{n}=g(z_{n})$.

Now, we can specialize the EM algorithm. In particular, we can write
$$
U(x,x')=\sum_{n=1}^{N}\sum_{c \in \mathcal{Z}}^{}p_{\mathsf{z}|\mathsf{y}}(c|y_{n};x')\log p_{\mathsf{z}}(c;x),\quad p_{\mathsf{z}|\mathsf{y}}(z|y;x)=\frac{p_{\mathsf{z}}(z;x)}{p_{\mathsf{y}}(y;x)}\mathbf{1}_{z \in g^{-1}(y)}.
$$
We also have that
$$
p_{\mathsf{y}}(y;x)=\sum_{z \in g^{-1}(y)}^{}p_{\mathsf{z}}(z;x).
$$
The ultimate goal of the EM algorithm is to find the M-projection
$$
\arg\min_{x}D(\hat{p}_{\mathsf{y}}(\bullet;\mathbf{y})\parallel p_{\mathsf{y}}(\bullet;x)),
$$
i.e. the MLE for the empirical distribution. However, we assume that this is a difficult optimization problem, but we can construct a complete data so that
$$
\arg\min_{x}D(\hat{p}_{\mathsf{z}}(\bullet;\mathbf{z})\parallel p_{\mathsf{z}}(\bullet;x))
$$
is a tractable problem. Of course, the realization of the complete data is fictitious, so the set of empirical distributions over $\mathcal{Z}$ is
$$
\hat{\mathcal{P}}^{\mathcal{Z}}(\mathbf{y})=\left\{ \hat{p}_{\mathsf{z}}(\bullet) : \sum_{c \in g^{-1}(b)}^{}\hat{p}_{\mathsf{z}}(c)=\hat{p}_{\mathsf{y}}(b;\mathbf{y})\text{ for all }b \in \mathcal{Y} \right\}, 
$$
i.e. empirical distributions that collapse under $g$ to the correct empirical distribution for $\mathbf{y}$.

> [!claim]
> The empirical distribution
> $$
> \hat{p}^{*}_{\mathsf{z}}(\bullet;x)=\arg\min_{\hat{p}_{\mathsf{z}}\in \hat{\mathcal{P}}^{\mathcal{Z}}(\mathbf{y})} D(\hat{p}_{\mathsf{z}}(\bullet)\parallel p_{\mathsf{z}}(\bullet;x))
> $$
> can be expressed in the form
> $$
> \hat{p}_{\mathsf{z}}^{*}(z;x)=\frac{p_{\mathsf{z}}(z;x)\hat{p}_{\mathsf{y}}(g(z))}{p_{\mathsf{y}}(g(z);x)}.
> $$

The upshot of this characterization is that
$$
\frac{1}{N}U(x,x')=-D(\hat{p}_{\mathsf{z}}^{*}(\bullet;x')\parallel p_{\mathsf{z}}(\bullet;x))-H\left( \hat{p}_{\mathsf{z}}^{*}(z;x') \right), 
$$
which allows us to rewrite the EM algorithm in the following form:

> [!example] (EM algorithm as alternating projections)
> 1. **E-step:** Set
> $$
> \hat{p}_{\mathsf{z}}^{*}(\bullet;\hat{x}^{(\ell-1)})=\arg\min_{\hat{p}_{\mathsf{z}}\in \hat{\mathcal{P}}^{\mathcal{Z}}(\mathbf{y})}D\left( \hat{p}_{\mathsf{z}}(\bullet)\parallel p_{\mathsf{z}}(\bullet;\hat{x}^{(\ell-1)}) \right).
> $$
> 2. **M-step**: Set
> $$
> \hat{x}^{(\ell)}=\arg\min_{x}D\left( \hat{p}_{\mathsf{z}}^{*}(\bullet;\hat{x}^{(\ell-1)})\parallel p_{\mathsf{z}}(\bullet;x) \right).
> $$
> 
> In other words, start with an arbitrary distribution in in the model family $\mathcal{P}$, and then repeat I-projecting it onto $\hat{\mathcal{P}}^{\mathcal{Z}}$ and M-projecting it back onto $\mathcal{P}$.

We know that $\hat{\mathcal{P}}^{\mathcal{Z}}$ is convex, so as long as the set of models $\mathcal{P}$ is also convex, the EM algorithm will converge to the MLE if the initial parameter estimate results in a strictly positive distribution.

The claim we left unproven above is a directly consequence of the following lemma:

> [!claim] (Data Processing Inequality—Decision Form)
> Let $\mathcal{Y}$ and $\mathcal{Z}$ be two alphabets, and let $g:\mathcal{Z}\to \mathcal{Y}$ be an arbitrary mapping, and let $p_{\mathsf{y}},q_{\mathsf{y}}\in \mathcal{P}^{\mathcal{Y}}$ be the (respective) distributions induced by arbitrary distributions $p_{\mathsf{z}},q_{\mathsf{z}}\in \mathcal{P}^{\mathcal{Z}}$ induced by the mapping $g$. Then,
> $$
> D(p_{\mathsf{z}}\parallel q_{\mathsf{z}})\geq D(p_{\mathsf{y}}\parallel q_{\mathsf{y}}),
> $$
> with equality if and only if
> $$
> \frac{p_{\mathsf{z}}(z)}{q_{\mathsf{z}}(z)}=\frac{p_{\mathsf{y}}(g(z))}{q_{\mathsf{y}}(g(z))}.
> $$

---

**Next:** [[Moment Matching]]
