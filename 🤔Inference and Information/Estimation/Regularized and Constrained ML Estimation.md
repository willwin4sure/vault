> Ridge and lasso regression.

There are two general ways of incorporating domain knowledge into your estimation to avoid degeneracy/instability.
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

For example, suppose we want to enforce that $\rho(\hat{\mathbf{x}})\geq0$; then, we can express this constrained ML estimate in the form
$$
\hat{\mathbf{x}}_{CML}(\mathbf{y})=\mathop{\arg\max}_{\mathbf{x}}\min_{\lambda \geq 0}\left[ \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) + \lambda \rho(\mathbf{x}) \right].
$$
In some cases, the solution to the optimization above is a saddle point, in which case
$$
\max_{\mathbf{x}}\min_{\lambda \geq 0}\left[ \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) + \lambda \rho(\mathbf{x}) \right] = \min_{\lambda \geq 0}\max_{\mathbf{x}}\left[ \ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x}) + \lambda \rho(\mathbf{x}) \right].
$$
The conditions to do this are referred to as the [Karush-Kuhn-Tucker conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions): generally, we want that both $\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\bullet)$ and $\rho(\bullet)$ are concave. Then, we can follow a proof similar to the [[Minimax Hypothesis Testing#^48bd56|proof that Bayes' risk exhibits a saddle point]] in the minimax formulation of hypothesis testing.

Suppose there is some optimal $\hat{\mathbf{x}}^{*}$ on the left. In the case where $\rho(\hat{\mathbf{x}}^{*})>0$, then we can just pick $\lambda=0$ on the other side and win. Otherwise, we have that $\rho(\hat{\mathbf{x}}^{*})=0$. At the optimum, by Lagrange multipliers, $\nabla_{\mathbf{x}}\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})=-\lambda\nabla_{\mathbf{x}}\rho(\mathbf{x})$. In particular, this means that the gradient of $\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})+\lambda \rho(\mathbf{x})$ with respect to $\mathbf{x}$, evaluated at $\hat{\mathbf{x}}^{*}$, is $0$, so it is a local optimum. Concavity ensures that it is also the global optimum; therefore, we can just pick the necessary $\lambda$ to win.

> [!remark]
> This shows up in the [[Actor-Critic Step Size|actor-critic step size derivation]] from Sensorimotor Learning. 
> 

Using this method, we could add a sparsity constraint of the form $\| \hat{\mathbf{x}} \|_{1}\leq\delta$.

---

**Next:** [[The Gauss-Markov Theorem]]