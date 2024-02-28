> Randomization linearly interpolates in the PF-PD plane. The upper convex hull of the OC-LRT is called the Neyman-Pearson function and consists of the optimal decision rules.

## Time-Sharing

Consider a pair of LRTs $\hat{H}'(\bullet)$ and $\hat{H}''(\bullet)$ corresponding to thresholds $\eta'$ and $\eta''$, with $\eta''>\eta'$. For any $p \in[0,1]$, we can ==time-share== between them with
$$
\hat{\mathsf{H}}(\mathbf{y})=\begin{cases}
\hat{H}'(\mathbf{y}) & \text{with probability }p \\
\hat{H}''(\mathbf{y}) & \text{with probability }1-p
\end{cases},
$$
which gives us operating values of
$$
\begin{align*}
P_{D}&=pP_{D}(\eta')+(1-p)P_{D}(\eta'')\\
P_{F}&=pP_{F}(\eta')+(1-p)P_{F}(\eta'')
\end{align*}
$$
which linearly interpolates in the PF-PD plane.

This allows us to achieve the upper convex hull of the OC-LRT. Here is a picture of the interpolated version of one of the [[Discontinuous OC-LRTs#^79744b|examples from last section]]:

![[interpolatepoissonoclrt.png|center|256]]

Now, we will develop a more general theory of randomized tests. However, we will show that nothing more complicated than the above is needed.

> [!idea] Time sharing is optimal.

## Randomized Tests

In general, a randomized decision rule can be characterized by a conditional distribution
$$
q(\mathbf{y}):=p_{\hat{\mathsf{H}}|\boldsymbol{\mathsf{y}}}(H_{1}|\mathbf{y}).
$$
This is assigning a number to each data point $\mathbf{y}$ giving the probability that we return $H_{1}$ (e.g. like in logistic regression). In particular, deterministic rules are a special case of the form
$$
q(\mathbf{y})=\mathbf{1}_{y\in \mathcal{Y}_{1}}.
$$
Now, consider the randomization from before. Take the limit $\eta''\to \eta'$ from the right; this leads to the ==point-mass splitting form== of the randomized LRT as
$$
p_{\hat{\mathsf{H}}|\boldsymbol{\mathsf{y}}}(H_{1}|\mathbf{y})
=\mathbb{P}(\hat{\mathsf{H}}(\boldsymbol{\mathsf{y}})=H_{1}|\boldsymbol{\mathsf{y}}=\mathbf{y})
=\begin{cases}
0 & L(\mathbf{y})<\eta' \\
p & L(\mathbf{y})=\eta' \\
1 & L(\mathbf{y})>\eta'
\end{cases}.
$$

## Optimality

> [!claim] (Randomization cannot help Bayes)
> A randomized test cannot achieve a lower Bayes' risk than the optimal LRT in binary Bayesian hypothesis testing.

> [!proof]- Duh
> Just pick whichever one is better. Pictorially, the highest tangent line does not get any higher when you add in the entire upper convex hull.

Next, the full version of [[Neyman-Pearson Hypothesis Testing#^125f6d|a previous lemma]]. 

> [!theorem] (Neyman-Pearson Lemma)
> Given hypotheses $p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0})$ and $p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{1})$ and any $\alpha \in[0,1]$, for some $\eta$ and $p \in[0,1]$ there exists a decision rule of the form
> $$
> q_{*}(\mathbf{y})=
> \begin{cases}
> 0 & L(\mathbf{y})<\eta \\
> p & L(\mathbf{y})=\eta \\
> 1 & L(\mathbf{y})>\eta
> \end{cases}
> $$
> such that $P_{F}(q_{*})=\alpha$ and $P_{D}(q_*)\geq P_{D}(q)$ for any decision rule $q(\bullet)$ satisfying $P_{F}(q)\leq\alpha$. 

> [!proof]- Omitted
> Probably also obvious.

> [!definition] (Neyman-Pearson function)
> The interpolated OC-LRT is called the ==Neyman-Pearson function==, denoted $\zeta_{NP}(\bullet)$.

The Neyman-Pearson function traces out the ==efficient frontier==, and the operating points on the curve are called ==Pareto-optimal==. This and its reflection across $\left( \frac{1}{2}, \frac{1}{2} \right)$ surround the region of achievable operating points (you can always just negate your prediction to send $(P_{F},P_{D})\mapsto(1-P_{F},1-P_{D})$).

Alright, now here are some trivial properties.

> [!proposition] (Properties of the Neyman-Pearson function)
> 1. It contains the point $(1,1)$.
> 2. It lies above $P_{F}=P_{D}$.
> 3. It is concave.
> 4. The slope of the Neyman-Pearson function is equal to the threshold of the corresponding LRT.

One-line proofs:

1. Always guess 1.
2. Always guess $1$ with probability $p$.
3. Linearly interpolate.
4. I guess this is not obvious, but it's intuitively clear from OC-LRT discussions.

---

**Next:** [[Minimax Hypothesis Testing]]

