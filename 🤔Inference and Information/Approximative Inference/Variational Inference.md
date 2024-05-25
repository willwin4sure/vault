Variational methods turn the task of evaluating a quantity into an optimization problem, and many fundamental variational methods arise out of considering the problem of evaluating a partition function.

## Variational Characterization of Partition Function

Consider finite $\mathcal{Y}$. Suppose we have some distribution $p \in \mathcal{P}^{\mathcal{Y}}$ but we only know it up to normalization $p\propto \tilde{p}$, where $p(\mathbf{y})=\frac{1}{Z}\tilde{p}(\mathbf{y})$.

> [!claim] (Variational characterization, partition function)
> Given $\mathcal{Y}$ and $p \in \mathcal{P}^{\mathcal{Y}}$ known up to $\tilde{p}$, the partition function satisfies
> $$
> \ln Z=\max_{q \in \mathcal{P}^{\mathcal{Y}}}\left\{ \log(e)H(q)+\mathbb{E}_{q}[\ln \tilde{p}(\boldsymbol{\mathsf{y}})] \right\}.
> $$

When $\boldsymbol{\mathsf{y}}$ is a continuous random variable in $\mathcal{Y}\subseteq \mathbb{R}^{N}$, an analogous result holds with the differential entropy $h(q)$ instead.

If we restrict to some more regular class $\mathcal{Q} \subseteq \mathcal{P}^{\mathcal{Y}}$, then this provides a way of lower bounding the log-partition function via
$$
\ln Z \geq \max_{q \in \mathcal{Q}}\left[ \log(e) H(q) + \mathbb{E}_{q}[\ln \tilde{p}(\boldsymbol{\mathsf{y}})] \right] 
$$
This is an example of a ==variational lower bound==. 

## Parameter Estimation in Exponential Families

Suppose we have a large finite alphabet $\mathcal{Y}$ and our collection of models is the $K$-dimensional exponential family
$$
\left\{ p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})\in \mathcal{P}^{\mathcal{Y}} : p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) = \exp \left\{ \mathbf{x}^{T}\mathbf{t}(\mathbf{y})-\alpha(\mathbf{x})+\beta(\mathbf{y}), \mathbf{y}\in \mathcal{Y},\mathbf{x}\in \mathcal{X} \right\}  \right\}.
$$
This involves evaluating the log-partition function $\alpha(\mathbf{x})$ which is
$$
\alpha(\mathbf{x})=\ln \sum_{\mathbf{y}' \in \mathcal{Y}}^{} \exp \left\{ \mathbf{x}^{T}\mathbf{t}(\mathbf{y}')+\beta(\mathbf{y}') \right\},
$$
which is computationally infeasible for large $|\mathcal{Y}|$.

One idea would be to pick some rich class $\mathcal{Q}$ that is easy to evaluate and approximate each $p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})$ with the M-projection
$$
q_{*}(\bullet;\mathbf{x})=\arg\min_{q \in \mathcal{Q}} D(p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x}) \parallel q).
$$
However, this optimization is infeasible: it involves an expectation with respect to $p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})$ which requires the log-partition function!

So let's return to the idea of using a computable bound on $\alpha(\mathbf{x})$. Pick some simple $\mathcal{Q}\subseteq \mathcal{P}^{\mathcal{Y}}$ and consider some $q \in \mathcal{Q}$. Then take a lower bound on $\alpha(\mathbf{x})$:
$$
\tilde{\alpha}_{q}(\mathbf{x})=\alpha(\mathbf{x})-\frac{1}{\log(e)}D(q\parallel p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})),
$$
where we refer to the second term as $\Delta_{q}(\mathbf{x})$. All this is doing is swapping the M-projection with an I-projection. This gives
$$
\begin{align}
\tilde{\alpha}_{q}(\mathbf{x})
&= -\sum_{\mathbf{y}'\in \mathcal{Y}}^{}q(\mathbf{y}')\ln \frac{q(\mathbf{y}')}{e^{\alpha(\mathbf{x})}p_{\boldsymbol{\mathsf{y}}}(\mathbf{y}';\mathbf{x})} \\
&=\frac{1}{\log(e)}H(q)+\mathbb{E}_{q}\left[ \mathbf{x}^{T}\mathbf{t}(\boldsymbol{\mathsf{y}})+\beta(\boldsymbol{\mathsf{y}}) \right]  \\
 & = \frac{1}{\log(e)}H(q)+\mathbf{x}^{T}\mathbb{E}_{q}[\mathbf{t}(\boldsymbol{\mathsf{y}})]+\mathbb{E}_{q}[\beta(\boldsymbol{\mathsf{y}})]
\end{align}
$$
which is computable if $\mathcal{Q}$ is chosen suitably. This gives some optimal distribution
$$
q_{*}(\bullet;\mathbf{x})=\arg\max_{q \in \mathcal{Q}}\tilde{\alpha}_{q}(\mathbf{x})=\arg\min_{q \in \mathcal{Q}}\Delta_{q}(\mathbf{x}).
$$
It may be tempting to use this as an approximation for $\alpha(\mathbf{x})$ via $\tilde{\alpha}_{q_{*}(\bullet;\boldsymbol{\mathsf{x}})}(\mathbf{x})$, but the distribution is not properly normalized.

> [!idea]
> It turns out that the distribution $q_{*}(\bullet;\mathbf{x})$ is a useful approximation for $p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})$.

This is what we call the variational approximation. Then, when fitting such a model to data, we replace the intractable ML estimate
$$
\hat{\mathbf{x}}_{ML}(\mathbf{y})=\arg\max_{\mathbf{x}}p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})
$$
with the variational approximation
$$
\hat{\mathbf{x}}_{*}(\mathbf{y})=\arg\max_{\mathbf{x}}q_{*}(\mathbf{y};\mathbf{x}).
$$
## Latent Variable Models

TODO

## Model Architecture Selection

TODO