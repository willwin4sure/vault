> From any introductory ML or statistics class.

Now consider a binary classification setting where we are given some observations $\boldsymbol{\mathsf{y}}=(\boldsymbol{\mathsf{u}},\mathsf{v})$, where $\boldsymbol{\mathsf{u}}\in \mathcal{U}$ is our source variable, and $\mathsf{v}\in \{ 0,1 \}$ is our corresponding class label.

A widely used forward model for their relationship is the following model:

## Definition

> [!definition] (Logistic model)
> The ==logistic model== for the relationship between $\boldsymbol{\mathsf{u}}$ and $\mathsf{v}$ is given by
> $$
> p_{\mathsf{v}|\boldsymbol{\mathsf{u}}}(1|\mathbf{u};\mathbf{x})=\frac{1}{1+e^{-\mathbf{x}^{T}\mathbf{t}(\mathbf{u})}}\quad\text{and}\quad p_{\mathsf{v}|\boldsymbol{\mathsf{u}}}(0|\mathbf{u};\mathbf{x})=\frac{1}{1+e^{\mathbf{x}^{T}\mathbf{t}(\mathbf{u})}}.
> $$
> Note that these sum to one! The model is parameterized by some (unknown) vector $\mathbf{x}\in \mathbb{R}^{K}$, and our feature functions are
> $$
> \mathbf{t}(\mathbf{u})=\begin{bmatrix}
> 1 & t_{1}(\mathbf{u}) & \cdots & t_{K-1}(\mathbf{u})
> \end{bmatrix}^{T}.
> $$
> 

Suppose $\mathbf{x}$ were known, then the MAP estimate is simply given by the decision rule
$$
\hat{x}^{T}\mathbf{t}(\mathbf{u})\mathop{\gtreqless}_{\hat{v}=0}^{\hat{v}=1}0.
$$

^6c1608

For example, when $K=2$, the decision boundary lies at $t_{1}(\mathbf{y})=-\frac{x_{0}}{x_{1}}$.

## ML Estimation

Suppose you are given a dataset of i.i.d. training pairs
$$
\mathcal{D}=\{ (\mathbf{u_{i}},v_{i}) \}_{i=1}^{N}
$$
that is generated from the model
$$
p_{\boldsymbol{\mathsf{u}},\mathsf{v}}(\mathbf{u},v;\mathbf{x})=p_{\mathsf{v}|\boldsymbol{\mathsf{u}}}(v|\mathbf{u};\mathbf{x})p_{\boldsymbol{\mathsf{u}}}(\mathbf{u}),
$$
where $p_{\boldsymbol{\mathsf{u}}}(\mathbf{u})$ is our source model (it will become irrelevant). Then, the maximum likelihood estimate of $\mathbf{x}$ is simply
$$
\begin{align*}
\hat{\mathbf{x}}_{ML}(\mathcal{D})
&=\mathop{\arg\max}_{\mathbf{x}}\left[ \frac{1}{N}\sum_{n=1}^{N}\ln p_{\mathsf{v}|\boldsymbol{\mathsf{u}}} (v_{n}|\mathbf{u}_{n};\mathbf{x})+\frac{1}{N}\sum_{n=1}^{N}\ln p_{\boldsymbol{\mathsf{u}}}(\mathbf{u}_{n}) \right] \\
&=\mathop{\arg\max}_{\mathbf{x}}\ \frac{1}{N}\sum_{n=1}^{N}\ln p_{\mathsf{v}|\boldsymbol{\mathsf{u}}}(v_{n}|\mathbf{u}_{n};\mathbf{x}),
\end{align*}
$$
where the term we are maximizing is simply the log-likelihood. This is simply minimizing the familiar negative log-likelihood loss function
$$
C(\mathbf{x})=-\frac{1}{N}\sum_{n=1}^{N}\left[ \mathbf{1}_{v_{n}=1}\cdot\ln \left( \frac{1}{1+e^{-\mathbf{x}^{T}\mathbf{t}(\mathbf{u}_{n})}} \right) + \mathbf{1}_{v_{n}=0}\cdot\ln \left( \frac{1}{1+e^{\mathbf{x}^{T}\mathbf{t}(\mathbf{u}_{n})}} \right) \right],
$$
which can be done via gradient descent (the function is convex).

Finally, once the parameters are estimated, we can construct a decision rule as in the [[Logistic Regression#^6c1608|previous section]]. 

---

**Return:** [[⛺Estimation Homepage]]