We [[Alternating Projections|just]] interpreted ML estimation as empirical distribution-fitting. In particular, ML parameter estimation corresponds to M-projection of the empirical distribution onto a set of candidate models.

In this section, we extend to the case where the set of candidate models forms an exponential family; this also allows us to handle continuous-valued data.

## Log-Likelihood for Exponential Family

Suppose we are working with the canonical exponential family
$$
\mathcal{P}=\left\{ p_{\mathsf{y}}(y;\mathbf{x}), \mathbf{x} \in \mathcal{X} \right\},
$$
where $\mathcal{Y}$ is an arbitrary alphabet, $\mathcal{X}=\mathbb{R}^{K}$, and
$$
p_{\mathsf{y}}(y;\mathbf{x})=\exp \left\{ \sum_{k=1}^{K}x_{k}t_{k}(y)-\alpha(\mathbf{x})+\beta(y) \right\}. 
$$
Then, the log-likelihood is literally
$$
\tilde{\ell}(\mathbf{x};\mathbf{y})=\frac{1}{N}\sum_{n=1}^{N}\ln p_{\mathsf{y}}(y_{n};\mathbf{x})=\frac{1}{N}\sum_{n=1}^{N}\sum_{k=1}^{K}x_{k}t_{k}(y_{n})-\alpha(\mathbf{x})+\frac{1}{N}\sum_{n=1}^{N}\beta(y_{n}),
$$
and setting to zero the gradient of this with respect to $\mathbf{x}$ gives
$$
\frac{1}{N}\sum_{n=1}^{N}t_{k}(y_{n})=\mathbb{E}_{p_{\mathsf{y}}(\bullet;\mathbf{x})}\left[ t_{k}(\mathsf{y}) \right],
$$
where we use the [[Single-Parameter Exponential Families#^42919e|moments of the log-partition function]]. You can also check that the Hessian is negative semidefinite, so this is the maximum.

> [!idea]
> The ML estimate is the set of parameter values so that the empirical average of natural statistics matches their population average.

When the data are continuous-valued and the natural statistics simple powers, we refer to this result as ==moment-matching==.

> [!example] (Gaussian mean matching)
> If we have samples $y_{i}\sim \mathtt{N}(x,1)$ for $x \in \mathbb{R}$, then the ML estimate is just the sample mean.

---

**Return:** [[⛺Information Homepage]]