Even if we have a cost function and priors, it may be infeasible to implement Bayesian parameter estimation directly, especially when $\boldsymbol{\mathsf{x}}$ and $\boldsymbol{\mathsf{y}}$ are high-dimensional. 

Therefore, we usually try to approximate the estimator by parameterizing it by $\boldsymbol{\theta}$. Then, we optimize
$$
\hat{\boldsymbol{\theta}}=\mathop{\arg\min}_{\boldsymbol{\theta}}\ \mathbb{E}[C(\boldsymbol{\mathsf{x}},\mathbf{f}_{\boldsymbol{\theta}}(\boldsymbol{\mathsf{y}}))]
$$
to generate the estimator
$$
\hat{\mathbf{x}}(\mathbf{y})=\mathbf{f}_{\hat{\boldsymbol{\theta}}}(\mathbf{y}).
$$

Some popular parameterizations are linear architectures and neural networks. In modern deep neural networks, there can be billions of parameters.

---

**Next:** [[BLS Covariance Inequality]]