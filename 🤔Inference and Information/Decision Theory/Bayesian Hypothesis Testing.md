> You have a prior over hypotheses and then reweight it based on likelihoods.
## Problem Setup

> [!definition] (Hypothesis testing)
> We have a set of ==hypotheses== $\mathcal{H}=\{ H_{0},H_{1},\dots,H_{M-1} \}$ and want to decide which model generated the observed data $\mathbf{y}\in \mathcal{Y}$.

In the **Bayesian approach**, a full description of our models also contains the ==a priori== probabilities
$$
p_{\mathsf{H}}(H_{m}),\quad m=0,1,\dots,M-1,
$$
together with a characterization of what the observed data should look like under each hypothesis: these are conditional probability distributions
$$
p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\bullet|H_{m}),\quad m=0,1,\dots,M-1.
$$
The function $p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|\bullet)$ is referred to as the ==likelihood function== (given a hypothesis, it tells you how likely the data was to be observed).

After receiving observations, we can construct our ==a posteriori== probabilities
$$
p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{m}|\mathbf{y}),\quad m=0,1,\dots,M-1.
$$

> [!claim] (Bayes' rule)
> The belief update is computed via ==Bayes' rule==, which states that
> $$
> p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{m}|\mathbf{y})
> = \frac{p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{m})p_{\mathsf{H}}(H_{m})}
> {\sum_{m'}^{}p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{m'})p_{\mathsf{H}}(H_{m'})}.
> $$
> This says what you think it does: scale the prior by the likelihoods, then renormalize.

To actually make a decision requires a bit more, however: we also need a measure of how "good" any given decision is. We will do this by defining a cost function.

---

**Next:** [[The Likelihood Ratio Test]]