## Regularization

In practice, ML estimation is used with *regularization*, e.g. in the learning of the parameters of a neural network.

> [!definition] (Regularized MLE)
> Given a parameterized model $p_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{x})$ for $\boldsymbol{\mathsf{y}}\in \mathcal{Y}$ and $\mathbf{x}\in \mathcal{X}$, the ML estimate can be replaced with the regularized ML estimate
> $$
> \hat{\mathbf{x}}_{RML}(\mathbf{y})=\mathop{\arg\max}_{\mathbf{x}\in \mathcal{X}}\left[ \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) + \rho(\mathbf{x}) \right],
> $$
> where $\rho(\bullet)$ is referred to as the ==regularizer==.

This regularizer serves to bias the procedure towards estimates that reflect good properties, e.g. you might want to constrain $\| \mathbf{x} \|_{2}$ to be small.

> [!idea]
> ML estimation with a particular regularizer is often equivalent to Bayesian estimation with a particular prior. These are both ways of incorporating domain knowledge.

For example, the MAP estimate in a Bayesian setting is
$$
\hat{\mathbf{x}}_{MAP}(\mathbf{y})=\mathop{\arg\max}_{\mathbf{x}\in \mathcal{X}}\left[ \ln p_{\boldsymbol{\mathsf{y}}|\boldsymbol{\mathsf{x}}}(\mathbf{y}|\mathbf{x}) + \ln p_{\boldsymbol{\mathsf{x}}}(\mathbf{x}) \right]. 
$$

> [!example] (Tikhonov regularization)
> If we have a Gaussian prior $p_{\boldsymbol{\mathsf{x}}}=\mathtt{N}(\mathbf{0},\boldsymbol{\Lambda}_{0})$, then the regularizer is of the form
> $$
> \rho(\mathbf{x})=-\frac{1}{2}\| \boldsymbol{\Lambda}_{0}^{-1/2}\mathbf{x} \|_{2}^{2}.
> $$
> In the particular case where $\boldsymbol{\Lambda}_{0}=\sigma_{0}^{2}\cdot\mathbf{I}$, this becomes
> $$
> \rho(\mathbf{x})=-\lambda \| \mathbf{x} \|_{2}^{2},
> $$
> where $\lambda=\frac{1}{2\sigma_{0}^{2}}$, a direct penalization of the $L^{2}$-norm of $\mathbf{x}$.

^bc61fe

A related example is when the $\mathsf{x}_{i}$ are i.i.d. zero-mean Laplace random variables, leading to a regularizer of the form $-\lambda \| \mathbf{x} \|_{1}$. This $L^{1}$-norm penalization promotes ==sparsity==, since it is sharper.

## Constraints

Regularizers impose a "soft" constraint on what the parameters should be. Sometimes, domain knowledge takes the form of "hard" constraints, however.

> [!remark]
> This shows up in the [[Actor-Critic Step Size|actor-critic step size derivation]] from Sensorimotor Learning. 
> 

TODO

---

**Next:** [[The Gauss-Markov Theorem]]