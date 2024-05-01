> Sample from some other distribution and reweight by importance or reject with appropriate probability.

There are a few core computational tasks that arise in inference:

1. Compute the partition function of a distribution.
2. Compute marginals of a joint distribution.
3. Compute a conditional distribution.
4. Compute expectations of functions with respect to a distribution.
5. Generate samples from a distribution.

All of these tasks are actually closely related: a good method for one of them can be converted into a method for any other of them.

## Sampling from a Distribution

Suppose you want to compute the expectation of a function $f$ on a random variable with distribution $p$, i.e.
$$
\mu_{f}=\mathbb{E}_{p}\left[ f(\mathsf{x}) \right],
$$
where $\mathsf{x}\sim p$. One way of estimating this is to generate i.i.d. $\mathsf{x}_{1},\dots,\mathsf{x}_{N}\sim p$ and compute
$$
\hat{\mu}_{f}(\boldsymbol{\mathsf{x}})=\frac{1}{N}\sum_{n=1}^{N}f(\mathsf{x}_{n}).
$$
This is an unbiased estimate whose variance falls with $N^{-1}$. However, we need to be able to generate samples from $p$, which might be infeasible.

> [!idea]
> Generate samples from a simpler distribution $q$, and then modify either the samples or how they are used.

Alright, say we have a target distribution $p \in \mathcal{P}^{\mathcal{X}}$ over a large alphabet $\mathcal{X}$, such that
$$
p(x)=\frac{p_{\circ}(x)}{Z_{p}}.
$$
Suppose we are in the common case where it is feasible to compute $p_{\circ}(x)$ for any particular $x \in \mathcal{X}$, but we cannot get $p(x)$ since the normalization constant $Z_{p}$ is infeasible to compute.

## Importance Sampling

We just need to be able to *sample* from some distribution $q$, and also have access to some $q_{\circ}$ such that $q(x)\propto q_{\circ}(x)$.

> [!example] (Importance sampling)
> Given i.i.d. $\mathsf{x}_{1},\dots,\mathsf{x}_{N}\sim q$,  ==importance sampling== constructs the estimate
> $$
> \hat{\mu}_{f}(\boldsymbol{\mathsf{x}})= \frac{\frac{1}{N}\sum_{n=1}^{N}w(\mathsf{x}_{n})f(\mathsf{x}_{n})}{\frac{1}{N}\sum_{n=1}^{N}w(\mathsf{x}_{n})},
> $$
> where the ==importance weights== are given by
> $$
> w(x)=\frac{p_{\circ}(x)}{q_{\circ}(x)}.
> $$

Why does this work? Well,
$$
\mathbb{E}_{q}\left[ w(\mathsf{x})f(\mathsf{x}) \right]=\sum_{x \in \mathcal{X}}^{} \frac{p_{\circ}(x)}{q_{\circ}(x)}f(x)q(x)=\frac{Z_{p}}{Z_{q}}\mu_{f}.
$$
The strong law of large numbers and continuous mapping theorem then ensures that the fraction $\hat{\mu}_{f}(\boldsymbol{\mathsf{x}})$ converges a.s. to $\mu_{f}$, as desired.

> [!remark]
> This showed up in Graphics when we discussed ray tracing algorithms. It also appears in Sensorimotor Learning in the [[Trust Region Policy Optimization|TRPO]] algorithm.

## Rejection Sampling

This aims to directly generate samples from the correct distribution $p$. Once again, suppose we can sample from some distribution $q$, and have access to a proportional $q_{\circ}(x)$. Further, we need to know a constant $c$ such that
$$
cq_{\circ}(x)>p_{\circ}(x)
$$
for all $x \in \mathcal{X}$.

> [!example] (Rejection sampling)
> To construct a set of i.i.d. samples from the distribution $p$, we first sample $x$ from $q$, and then only keep it if $u\sim \mathtt{U}([0,cq_{\circ}(x)])$ is at most $p_{\circ}(x)$. Otherwise, it gets rejected.

Note that the total rejection rate is related to the area $cZ_{q}-Z_{p}$ between the two curves, and grows with $c$.

---

**Next:** [[Markov Chain Monte Carlo (MCMC)]]