We continue our discussion of asymptotic approximation for high-dimensional inference, starting with the case of [[⛺Decision Theory Homepage|binary hypothesis testing]] and leveraging the tools we have just developed.

## LRT Setup

Suppose we receive i.i.d. samples $\mathbf{y}=(y_{1},\dots,y_{N})$, where the underlying distribution over $\mathcal{Y}$ is $p_{0}$ under the null and $p_{1}$ under the alternative hypothesis. For any constant $\gamma$, the log-likelihood ratio test takes the form
$$
\frac{1}{N}\sum_{n=1}^{N}\log \frac{p_{1}(y_{n})}{p_{0}(y_{n})}\mathop{\gtreqless}_{\hat{H}(\mathbf{y})=H_{0}}^{\hat{H}(\mathbf{y})=H_{1}}\gamma.
$$
This induces the decision regions $\mathcal{Y}^{N}=\mathcal{R}_{0} \cup \mathcal{R}_{1}$, where we include the equality case in both regions for simplicity of analysis.

We can construct corresponding subsets of the probability simplex for these decision regions:
$$
\mathcal{S}_{0}=\left\{ p \in \mathcal{P}^{\mathcal{Y}} : \mathbb{E}_{p}\left[ \log \frac{p_{1}(\mathsf{y})}{p_{0}(\mathsf{y})} \right] \leq \gamma  \right\},\quad \mathcal{S}_{1}=\left\{ p \in \mathcal{P}^{\mathcal{Y}} : \mathbb{E}_{p}\left[ \log \frac{p_{1}(\mathsf{y})}{p_{0}(\mathsf{y})} \right] \geq \gamma  \right\}. 
$$
Note that the relationship is that
$$
\mathcal{R}_{i}=\left\{ \mathbf{y}\in \mathcal{Y}^{N} : \hat{p}(\bullet;\mathbf{y}) \in \mathcal{S}_{i} \cap \mathcal{P}_{N}^{\mathcal{Y}} \right\},
$$
i.e. the empirical distribution satisfies the corresponding inequality.

We consider the case where $p_{0}\in \mathcal{S}_{0}$ and $p_{1}\in \mathcal{S}_{1}$, which corresponds to the bound
$$
-D(p_{0}\parallel p_{1})\leq\gamma \leq D(p_{1}\parallel p_{0}).
$$
## Sanov on Performance Measures

Now, let's apply [[Sanov's Theorem|Sanov's theorem]] to the [[OC-LRT#Classification Performance Measures|classification performance measures]] of false-alarm and miss probabilities. In particular,
$$
P_{F}=\mathbb{P}\left( \hat{H}(\mathbf{y})=H_{1} | \mathsf{H}=H_{0} \right) = P_{0}\left\{ \mathcal{R}_{1} \right\} = P_{0}\{ \mathcal{S}_{1}\cap \mathcal{P}_{N}^{\mathcal{Y}} \}\doteq 2^{-ND(p_{0}^{*}\parallel p_{0})},
$$
where $p_{0}^{*}$ is the I-projection of $p_{0}$ onto $\mathcal{S}_{1}$. Further,
$$
P_{M}=1-P_{D}=\mathbb{P}\left( \hat{H}(\mathbf{y})=H_{0} | \mathsf{H}=H_{1} \right)=P_{1}\{ \mathcal{R}_{0} \}=P_{1}\{ \mathcal{S}_{0}\cap \mathcal{P}^{\mathcal{Y}}_{N} \}\doteq 2^{-ND(p_{1}^{*}\parallel p_{1})},
$$
where $p_{1}^{*}$ is the I-projection of $p_{1}$ onto $\mathcal{S}_{0}$.

> [!claim]
> The projections are the same, i.e. $p_{0}^{*}=p_{1}^{*}$.

This is due to the form of the likelihood ratio test, which establishes a boundary between the two region $\mathcal{S}_{0}$ and $\mathcal{S}_{1}$ on the simplex that is the [[Linear Families#^cdaf8f|linear family]]
$$
\mathcal{L}=\left\{ p \in \mathcal{P}^{\mathcal{Y}} : \mathbb{E}_{p}\left[ \log \frac{p_{1}(\mathsf{y})}{p_{0}(\mathsf{y})} \right] =\gamma  \right\}, 
$$
and the [[Orthogonal Families|orthogonal exponential family]] to $\mathcal{L}$ passes through both $p_{0}$ and $p_{1}$.

The geometry is depicted below:

![[hypothesisgeometry.png|center|256]]

Finally, note that $p_{*}=p^{*}_{0}=p^{*}_{1}$ is some weighted geometric mixing of the two candidate distributions $p_{0}$ and $p_{1}$.

Here is a pretty picture of what happens when the threshold $\gamma$ shifts and we need to determine where $x_{*}$, the mixing parameter for $p_{*}$, is.

![[hypothesisdivergences.png|center|384]]

## Neyman-Pearson Formulation

In the [[Neyman-Pearson Hypothesis Testing#^d61b40|Neyman-Pearson formulation]], we place a constraint on $P_{F}$ while maximizing $P_{D}$.

> [!claim] (Stein's lemma)
> When $P_{F}\leq\alpha$, the fastest rate of decay for $P_{M}$ is $D(p_{0}\parallel p_{1})$.

Indeed, if we just want the miss probability to decay as quickly as possible, we should set the threshold all the way at $p_{0}$. Indeed, taking $\gamma=-D(p_{0}\parallel p_{1})$ gives $p_{*}=p_{0}$ and
$$
P_{M}\doteq 2^{-ND(p_{0}\parallel p_{1})},\quad P_{F}\doteq 1,
$$
so $P_{F}$ decays subexponentially while $P_{M}$ decays as quickly as possible with Chernoff exponent $D(p_{0}\parallel p_{1})$.

Here is a picture:

![[npdivergences.png|center|384]]

## Bayesian Formulation

Recall in the [[The Likelihood Ratio Test#^ae30e6|Bayesian formulation]] we choose the threshold $\gamma$ based on minimizing the expected cost. In this case, we just need to balance the exponential decay terms regardless of the costs and priors (which are just constant factors), giving the following picture:

Here is a picture:

![[bayesdivergences.png|center|384]]

This resultant $E_{C}$ is called the ==Chernoff information==.

## Extreme Thresholds

When $\gamma$ falls out of the specified range, both $p_{0}$ and $p_{1}$ are on the same side of $\mathcal{L}$, though they still both project onto the same point.

---

**Next:** [[Convergence of Random Sequences]]



