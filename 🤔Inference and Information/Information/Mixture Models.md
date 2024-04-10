Our central approach will be to model $\mathsf{y}$ as a *mixture* of the candidate models, i.e.
$$
q_{w}(y)=\sum_{x \in \mathcal{X}}^{}w(x)p_{\mathsf{y}}(y;x),
$$
where the weights $w(x)$ specify a convex combination of the $p_{\mathsf{y}}(\bullet;x)$. A Bayesian viewpoint would interpret $w$ as a prior; indeed, $w \in \mathcal{P}^{\mathcal{X}}$.

It turns out that no model can do better than a mixture one.

> [!theorem] (Mixtures are Optimal)
> Let $\{ p_{\mathsf{y}}(\bullet;x) : x \in \mathcal{X} \}$ be a class of models, and let $q\in \mathcal{P}^{\mathcal{Y}}$ be an admissible distributions. Then, there exist weights $w(\bullet)$ in a mixture model
> $$
> q_{w}(\bullet)=\sum_{x \in \mathcal{X}}^{}w(x)p_{\mathsf{y}}(\bullet;x)
> $$
> such that
> $$
> D(p_{\mathsf{y}}(\bullet;x)\parallel q_{w})\leq D(p_{\mathsf{y}}(\bullet;x)\parallel q)
> $$
> for all $x \in \mathcal{X}$. 
> 

> [!proof]- Pythagorean theorem
> Let $\mathcal{P}$ be the set of mixture models; this is convex and closed. For any $q\not\in \mathcal{P}$, consider
> $$
> q_{w}=\arg\min_{p \in \mathcal{P}}D(p\parallel q),
> $$
> the I-projection of $q$ onto $\mathcal{P}$. Now, by Pythagoras,
> $$
> D(p\parallel q)\geq D(p \parallel q_{w})+D(q_{w}\parallel q),
> $$
> so $q_{w}$ is better.

## Minimax Optimization

One natural framework for choosing $q$ is via a minimax criterion, as the outcome a zero-sum game between us and an adversarial nature. 

> [!definition] (Minimax redundancy)
> We define the ==minimax redundancy== in $q$ as
> $$
> R^{+}:=\min_{q \in \mathcal{P}^{\mathcal{Y}}}\max_{x \in \mathcal{X}} D(p_{\mathsf{y}}(\bullet;x)\parallel q).
> $$

In other words, we pick a model, and then nature responds by picking the worst-case true answer. We want to be balanced so we don't lose badly.

> [!claim]
> For any $q \in \mathcal{P}^{\mathcal{Y}}$, we have
> $$
> \max_{x \in \mathcal{X}}D(p_{\mathsf{y}}(\bullet;x)\parallel q)=\max_{w \in \mathcal{P}^{\mathcal{X}}}\sum_{x}^{}w(x) D(p_{\mathsf{y}}(\bullet;x)\parallel q).
> $$

In other words, even if nature is allowed to pick a mixture, they're going to assign all the weight to exactly one distribution. This is obvious.

However, this relaxation is important for us to state the following saddle-point theorem:

> [!theorem] (Redundancy-Capacity)
> We have that
> $$
> R^{+}=\min_{q \in \mathcal{P}^{\mathcal{Y}}} \max_{w \in \mathcal{P}^{\mathcal{X}}}\sum_{x \in \mathcal{X}}^{}w(x)D(p_{\mathsf{y}}(\bullet;x)\parallel q)=\max_{w \in \mathcal{P}^{\mathcal{X}}}\min_{q \in \mathcal{P}^{\mathcal{Y}}}\sum_{x \in \mathcal{X}}^{}w(x)D(p_{\mathsf{y}}(\bullet;x)\parallel q)=R^{-}.
> $$

Let's examine the inner minimum in the definition of $R^{-}$. After expansion, [[Some Useful Inequalities#^e52776|Gibbs' inequality]] tells us that this is optimized when
$$
q^{*}(\bullet)=q_{w}(\bullet):=\sum_{x}^{}w(x)p_{\mathsf{y}}(\bullet;x).
$$
This is not very surprising. This allows us to rewrite
$$
R^{-}=\max_{w \in \mathcal{P}^{\mathcal{X}}}\sum_{x}^{}w(x)D(p_{\mathsf{y}}(\bullet;x)\parallel q_{w})=\sum_{x}^{}\sum_{y}^{}w(x)p_{\mathsf{y}}(y;x)\log \frac{p_{\mathsf{y}}(y;x)}{\sum_{x'}^{}w(x')p_{\mathsf{y}}(y;x')}.
$$
Useful intuition will be gained by returning to a Bayesian point of view.

## The Least Informative Prior and Model Capacity

Interpret $w(\bullet)=p_{\mathsf{x}}(\bullet)$ as a prior on $x$, and let our class of models be expressed as a conditional distribution $p_{\mathsf{y}}(y;x)=p_{\mathsf{y}|\mathsf{x}}(y|x)$. Then, we can interpret the mixture as $p_{\mathsf{y}}(\bullet)$, via
$$
q_{w}(y)=p_{\mathsf{y}}(y)=\sum_{x}^{}p_{\mathsf{x}}(x)p_{\mathsf{y}|\mathsf{x}}(y|x).
$$
Then, the term we are optimizing for $R^{-}$ is
$$
\sum_{x}^{}p_{\mathsf{x}}(x)D(p_{\mathsf{y}|\mathsf{x}}(\bullet|x)\parallel p_{\mathsf{y}})=\sum_{x,y}^{}p_{\mathsf{x},\mathsf{y}}(x,y)\log \frac{p_{\mathsf{x},\mathsf{y}}(x,y)}{p_{\mathsf{x}}(x)p_{\mathsf{y}}(y)}=I(\mathsf{x};\mathsf{y}).
$$
Therefore,
$$
R^{-}=\max_{p_{\mathsf{x}}}I(\mathsf{x};\mathsf{y}).
$$
We have a word for this.

> [!definition] (Least informative prior and model capacity)
> Let $p_{\mathsf{y}|\mathsf{x}}$ be a model. The ==least informative prior== $p^{*}_{\mathsf{x}}$ for $p_{\mathsf{y}|\mathsf{x}}$ is given by
> $$
> p_{\mathsf{x}}^{*}=\arg\max_{p_{\mathsf{x}}} I(\mathsf{x};\mathsf{y}).
> $$
> Meanwhile, the ==capacity== $C$ of the model is the average cost reduction associated with the least informative prior, i.e.
> $$
> C=\max_{p_{\mathsf{x}}} I(\mathsf{x};\mathsf{y}).
> $$

The model capacity is a fundamental measure of the richness of the model family. Do note that $0\leq C\leq \log|\mathcal{X}|$.

The lower bound is tight when an observation $\mathsf{y}=y$ gives *no* information about $x$. The upper bound is achieved when there is no entropy left in $\mathsf{x}$ conditioned on $\mathsf{y}$, if $H(\mathsf{x}|\mathsf{y})=0$. 

> [!theorem] (Equidistance property)
> The optimum mixture model $q^{*}$, for which the optimum weights are $w^{*}$, is such that
> $$
> D(p_{\mathsf{y}}(\bullet;x)\parallel q^{*})\leq C
> $$
> for all $x \in \mathcal{X}$, with equality for all $x$ such that $w^{*}(x)\geq 0$.

The redundancy-capacity theorem follows from this. I've omitted the proofs.

## Inference with Mixture Models

In this section, we illustrate how to use optimized mixture models for inference. Suppose there is a set of variables $\mathsf{y}_{-}$ that we observe, and a set $\mathsf{y}_{+}$ that we do not observe but want to make inferences about. In particular, we want to find some $q_{\mathsf{y}_{+}|\mathsf{y}_{-}}(\bullet|y_{-})$ that is as close as possible in the minimax redundancy sense to
$$
p_{\mathsf{y}_{+}|\mathsf{y}_{-}}(\bullet|y_{-};x)= \frac{p_{\boldsymbol{\mathsf{y}}}(\bullet,y_{-};x)}{\sum_{b}^{}p_{\boldsymbol{\mathsf{y}}}(b,y_{-};x)}
$$
for all values $x$ of the unknown parameter, where $\boldsymbol{\mathsf{y}}=(\mathsf{y}_{+},\mathsf{y}_{-})$.

TODO

---

**Next:** [[Continuous Information Theory]]


 