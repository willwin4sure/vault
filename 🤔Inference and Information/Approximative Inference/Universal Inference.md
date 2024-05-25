Now we move on to asymptotics of inference. The concept of universal inference is that once we have observed enough data from an i.i.d. stream, we can perform inference and make predictions just as well as if we knew the true underlying distribution.

## Universal Prediction

> [!definition] (Universal predictor)
> For $N\geq 1$ and finite $\mathcal{Y}$, let
> $$
> \mathcal{S}_{N}=\left\{ p_{\mathsf{y}^{N}}(\bullet;x) \in \mathcal{P}^{\mathcal{Y}^{N}}, x \in \mathcal{X} \right\}
> $$
> denote a family of models for data $y^{N}=(y_{1},\dots,y_{N})\in \mathcal{Y}^{N}$. Let $q_{1},q_{2},\dots,q_{N}\in \mathcal{P}^{\mathcal{Y}}$ be a sequence of predictors, where $q_{n}(\bullet;y^{n-1})$ is an estimate of the distribution for $\mathsf{y}_{n}$ based on data $y^{n-1}=(y_{1},\dots,y_{n-1})$. Then this sequence of predictors is ==universal== if
> $$
> \lim_{ N \to \infty } \max_{x \in \mathcal{X}}\bar{\rho}_{N}(x)=0
> $$
> where
> $$
> \bar{\rho}_{N}(x):= \frac{1}{N}\sum_{n=1}^{N}\rho_{n}(x),\quad
> \rho_{n}(x):=\mathbb{E}_{p_{\mathsf{y}^{N}}(\bullet;x)}\left[ \log \frac{p_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(\mathsf{y_{n}}|\mathsf{y}_{n-1};x)}{q_{n}(\mathsf{y}_{n};\mathsf{y}^{n-1})} \right], 
> $$
> with
> $$
> p_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(y_{n}|y^{n-1};x)= \frac{p_{\mathsf{y}^{n}}(y^{n};x)}{p_{\mathsf{y}^{n-1}}(y^{n-1};x)}.
> $$

A perhaps more natural representation of $\rho_{n}(x)$ is
$$
\rho_{n}(x)=\mathbb{E}_{p_{\mathsf{y}^{n-1}}(\bullet;x)}\left[ D\left(p_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(\bullet|\mathsf{y}^{n-1};x) \parallel q_{n}(\bullet;\mathsf{y}^{n-1})\right)  \right],
$$
i.e. the average over all rollouts of the first $n-1$ numbers of the divergence between the true posterior for the next element and the prediction of the next element.

> [!remark]
> Provided that $\lim_{ N \to \infty }D_{N}$ exists, the fact that $\lim_{ N \to \infty } \frac{1}{N}\sum_{n=1}^{N}D_{n}=0$ is equivalent to $\lim_{ N \to \infty }D_{N}=0$ and $D_{N}<\infty$ for all $N$.

This is because the initial transient behavior of $D_{N}$ doesn't matter at all.

> [!remark]
> The concept of a universal predictor is stronger than $\lim_{ N \to \infty }\bar{\rho}_{N}(x)=0$ for all $x \in \mathcal{X}$, which is called a ==weakly universal predictor==. 

> [!claim]
> The average prediction loss can be expressed in the form
> $$
> \bar{\rho}_{N}(x)=\frac{1}{N}D\left( p_{\mathsf{y}^{N}}(\bullet;x) \parallel q_{\mathsf{y}^{N}}(\bullet) \right),
> $$
> where
> $$
> q_{\mathsf{y}^{N}}(y^{N}):=\prod_{n=1}^{N}q_{n}(y_{n};y^{n-1}).
> $$

Alright, time for the real stuff.

> [!theorem] (Universal Prediction Theorem)
> Let the model capacity of the family $\mathcal{S}_{N}$ be $C_{N}$. Then for $\mathsf{y}^{N}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})$ with distribution $p \in \mathcal{S}_{N}$, universal inference is possible if and only if
> $$
> \lim_{ N \to \infty } \frac{C_{N}}{N}=0.
> $$

It turns out that the universal predictor can be constructed as the mixture predictor with weights corresponding to the [[Mixture Models#^1ed9df|least informative prior]].

## Universal Mixture Predictors

It turns out that you can replace the horizon-dependent least informative prior $p_{\mathsf{x}}^{N}$ with an asymptotic least informative prior.

> [!definition] (Asymptotic least informative prior)
> A distribution $p_{\mathsf{x}}^{\infty}\in \mathcal{P}^{\mathcal{X}}$ is an ==asymptotic least informative prior== for a family with model capacity $C_{N}$ if the mutual information $I(\mathsf{x};\mathsf{y}^{N})$ associated with the joint distribution $p_{\mathsf{y}^{N}}(y^{N};x)p_{\mathsf{x}}^{\infty}(x)$ satisfies
> $$
> C_{N}-I(\mathsf{x};\mathsf{y}^{N})=o(1)
> $$
> as $N\to \infty$.

Accordingly, consider the mixture distribution
$$
q_{\mathsf{y}^{N}}(y^{N})=\int_{\mathcal{X}}p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{N}}(y^{N};a) \, da 
$$
and use it to construct the associated predictor. We can construct the predictor via
$$
\begin{align}
q_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(y_{n}|y^{n-1})
&= \frac{q_{\mathsf{y}^{n}}(y^{n})}{q_{\mathsf{y}^{n-1}}(y^{n-1})} \\
&= \frac{\int_{\mathcal{X}} p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{n-1}}(y^{n-1};a)p_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(y_{n}|y^{n-1};a) \, da }{\int_{\mathcal{X}} p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{n-1}}(y^{n-1};a) \, da} \\
&= \int \left[ \frac{p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{n-1}}(y^{n-1};a)}{\int _{\mathcal{X}}p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{n-1}}(y^{n-1};a) \, da } \right] p_{\mathsf{y}_{n}|\mathsf{y}^{n-1}}(y_{n}|y^{n-1};a) \, da, 
\end{align}
$$
is some weighted average of the candidate predictors using
$$
w_{n|n-1}(a|y^{n-1})\propto p_{\mathsf{x}}^{\infty}(a)p_{\mathsf{y}^{n-1}}(y^{n-1};a).
$$
This is rather intuitive.

Weights can also be obtained iteratively via "tilt-and-normalize" updates
$$
w_{n+1|n}(a|y^{n})\propto w_{n|n-1}(a|y^{n-1})p_{\mathsf{y}}(y_{n};a),
$$
where we start at $w_{1|0}(\bullet|y^{0})=p_{\mathsf{x}}^{\infty}$.

## Asymptotics of Model Capacity

We develop asymptotics of model capacity for i.i.d. data to establish when universal prediction is possible. The model capacity and associated least informative prior are
$$
C_{N}=\max_{p_{\mathsf{x}}}I(\mathsf{x};\mathsf{y}^{N}),\quad
p_{\mathsf{x}}^{N}=\arg\max_{p_{\mathsf{x}}}I(\mathsf{x};\mathsf{y}^{N}).
$$
Recall that
$$
I(\mathsf{x};\mathsf{y}^{N})=\mathbb{E}\left[ \log \frac{p_{\mathsf{x},\mathsf{y}^{N}}(\mathsf{x},\mathsf{y}^{N})}{p_{\mathsf{x}}(x)p_{\mathsf{y}^{N}}(\mathsf{y}^{N})} \right]
=\mathbb{E}\left[ \log \frac{p_{\mathsf{y}^{N}|\mathsf{x}}(\mathsf{y}^{N}|\mathsf{x})}{p_{\mathsf{y}^{N}}(\mathsf{y}^{N})} \right]
=\mathbb{E}\left[ D(p_{\mathsf{y}^{N}|\mathsf{x}}(\bullet|x) \parallel p_{\mathsf{y}^{N}}) \right].
$$
Note that this final expectation is only over $x$.

### Finite Case

> [!theorem]
> Consider the model family $\mathcal{S}$ with $|\mathcal{X}|<\infty$, and let $\mathsf{y}^{N}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})$ be generated i.i.d. according to $p_{\mathsf{y}|\mathsf{x}}(\bullet|x)$ where $x \in \mathcal{X}$ is generated from the positive distribution $p_{\mathsf{x}}$. Moreover, suppose all $x \in \mathcal{X}$ are identifiable and $p_{\mathsf{y}|\mathsf{x}}(\bullet|a)$ is positive for all $a \in \mathcal{X}$. Then,
> $$
> D(p_{\mathsf{y}^{N}|\mathsf{x}}(\bullet|x)\parallel p_{\mathsf{y}^{N}})=-\log p_{\mathsf{x}}(x)+o(1),\quad N\to \infty
> $$
> and
> $$
> I(\mathsf{x};\mathsf{y}^{N})=H(p_{\mathsf{x}})+o(1),\quad N\to \infty.
> $$
> Moreover, the model capacity is
> $$
> C_{N}=\log|\mathcal{X}|+o(1),\quad N\to \infty,
> $$
> achieved by the uniform prior.

This immediately implies that universal inference is possible since $\frac{C_{N}}{N}=\mathcal{O}\left( \frac{1}{N} \right)$. 

### Continuous Case

When $\mathsf{x}$ is a continuous parameter the analysis of the model capacity is more involved. We will consider the case of canonical exponential family models
$$
p_{\mathsf{y}}(y;a)=\exp \left\{ at(y)-\alpha(a)+\beta(y) \right\}.
$$

> [!claim] (Partition function lemma)
> Consider a canonical exponential model family $\mathcal{S}$ and let $\mathsf{y}^{N}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})$ be generated i.i.d. according to $p_{\mathsf{y}|\mathsf{x}}(\bullet|x)$ where $x \in \mathcal{X}$ is generated from $p_{\mathsf{x}}$, where $p_{\mathsf{x}}(\bullet)$ is positive and continuous. Moreover, suppose all $a \in \mathcal{X}$ are identifiable and $J_{\mathsf{y}}(a)>0$ for all $a \in \mathcal{X}$. Then, with $\tilde{\mathsf{x}}=\sqrt{ N }(\mathsf{x}-\hat{x}_{N}(y^{N}))$, we have that
> $$
> \frac{1}{\sqrt{ N }}p_{\mathsf{x}|\mathsf{y}^{N}}(\hat{x}_{N}(\mathsf{y}^{N}))=p_{\tilde{\mathsf{x}}|\mathsf{y}^{N}}(0|\mathsf{y}^{N})\xrightarrow[N\to \infty]{\text{a.s.}}\sqrt{ \frac{J_{\mathsf{y}}(x)}{2\pi} }. 
> $$

This gives rise to the following theorem.

> [!theorem] (Bernstein-von Mises Theorem, Strong Version)
> Consider the model family $\mathcal{S}$ and let $\mathsf{y}^{N}=(\mathsf{y}_{1},\dots,\mathsf{y}_{N})$ be generated i.i.d. according to $p_{\mathsf{y}|\mathsf{x}}(\bullet|x)$, where $x \in \mathcal{X}$ is generated from $p_{\mathsf{x}}$ which is positive and differentiable. Moreover, suppose all $x \in \mathcal{X}$ are identifiable and $\ddot{\alpha}(a)=J_{\mathsf{y}}(a)>0$ for all $a \in \mathcal{X}$. Then, with $\tilde{\mathsf{x}}=\sqrt{ N }\left( \mathsf{x}-\hat{x}_{N}(\mathsf{y}^{N}) \right)$,
> $$
> D\left( p_{\tilde{\mathsf{x}}|\mathsf{y}^{N}}(\bullet|y^{N}) \parallel \mathtt{N}\left( 0,1/J_{\mathsf{y}}(x) \right)  \right) \xrightarrow[N\to \infty]{\text{a.s.}} 0. 
> $$

So the term above from the partition function is actually the value of the pdf at $0$ of the normal distribution with mean $0$ and variance $1/J_{\mathsf{y}}(x)$. Intuitively,
$$
p_{\mathsf{x}|\mathsf{y}^{N}}(\bullet|\mathsf{y}^{N})\approx \mathtt{N}\left( \hat{x}_{N}(\mathsf{y}^{N}), \frac{1}{NJ_{\mathsf{y}}(x)} \right).
$$
We now develop an asymptotic approximation to model capacity. Indeed,
$$
\log p_{\mathsf{x}|\mathsf{y}^{N}}(x|\mathsf{y}^{N})\approx \frac{1}{2}\log \frac{NJ_{\mathsf{y}}(x)}{2\pi}-\frac{1}{2}\log(e)NJ_{\mathsf{y}}(x)\left( x-\hat{x}_{N}(\mathsf{y}^{N}) \right)^{2}.
$$
With further approximations, we can write
$$
\mathbb{E}_{p_{\mathsf{y}^{N}|\mathsf{x}}(\bullet|x)}\left[ \log p_{\mathsf{x}|\mathsf{y}^{N}}(x|\mathsf{y}^{N}) \right] 
\approx \frac{1}{2}\log \frac{NJ_{\mathsf{y}}(x)}{2\pi e},
$$
which allows us to write
$$
\begin{align}
I(\mathsf{x};\mathsf{y}^{N})
&=\mathbb{E}_{p_{\mathsf{x}}}\left[ \mathbb{E}_{p_{\mathsf{y}^{N}|\mathsf{x}}(\bullet|x)}\left[ \log \frac{p_{\mathsf{x}|\mathsf{y}^{N}}(\mathsf{x}|\mathsf{y}^{N})}{p_{\mathsf{x}}(x)} \right]  \right] \\
&\approx \frac{1}{2}\log \frac{N}{2\pi e}-D(p_{\mathsf{x}}\parallel p_{\mathsf{x}}^{*})+\log \int_{\mathcal{X}} \sqrt{ J_{\mathsf{y}}(a) } \, da,
\end{align}
$$
where we define the distribution
$$
p_{\mathsf{x}}^{*}(x):= \frac{\sqrt{ J_{\mathsf{y}}(x) }}{\int _{\mathcal{X}}\sqrt{ J_{\mathsf{y}}(a) } \, da },
$$
called the ==Jeffreys' prior==. This is then the least informative prior and the model capacity is
$$
C_{N}\approx \frac{1}{2}\log \frac{N}{2\pi e}+\log \int _{\mathcal{X}}\sqrt{ J_{\mathsf{y}}(a) } \, da. 
$$

more stuff TODO

---

**Next:** [[Variational Inference]]