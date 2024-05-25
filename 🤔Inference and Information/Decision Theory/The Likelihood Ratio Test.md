> Compare the ratio of likelihoods to a threshold that is a function of priors and costs.
## Binary Classification

We specialize to the special case of binary classification. $H_{0}$ is called the ==null hypothesis== and $H_{1}$ is called the ==alternative hypothesis==.

> [!definition] (Decision rules and regions)
> A ==decision rule== is a function $\hat{H}:\mathcal{Y}\to \mathcal{H}$ that maps every possible observation $\mathbf{y}$ to one of the two hypotheses. This can be viewed as partitioning $\mathcal{Y}$ into two disjoint ==decision regions== $\mathcal{Y}_{0}$ and $\mathcal{Y}_{1}$.

> [!example]
> All classifiers are decision rules. Some are less interpretable, such as neural networks.

How do we quantify what is the "best" action?

> [!definition] (Cost function)
> We define $C(H_{j},H_{i})=:C_{ij}$ as the cost of deciding that the hypothesis is $\hat{H}=H_{i}$, when the correct hypothesis is actually $\mathsf{H}=H_{j}$. This is ==valid== if the cost of a correct decision is lower than the cost of an incorrect decision.

We only think about valid cost functions.

> [!example] ($0$-$1$ loss)
> A basic symmetric loss function is $C_{ij}=\mathbf{1}_{i\neq j}$.

Then, the optimal decision rule minimizes the expected cost.

> [!definition] (Bayes' risk)
> We refer to the expected cost of a decision rule $f$ as the ==Bayes' risk==
> $$
> \varphi(f):=\mathbb{E}\left[ C(\mathsf{H},f(\boldsymbol{\mathsf{y}})) \right].
> $$
> The expectation is over sampling a hypothesis from a prior and then generating some data $\boldsymbol{\mathsf{y}}=\mathbf{y}$.

> [!theorem] (Likelihood ratio test)
> Given a priori probabilities $P_{i}$, data $\mathbf{y}$, observation models $p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\bullet|H_{i})$, and valid costs $C_{ij}$, the Bayesian decision rule takes the form
> $$
> L(\mathbf{y})
> :=
> \frac{p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{1})}
> {p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0})}
> \mathop{\gtreqless}_{\hat{H}(\mathbf{y})=H_{0}}^{\hat{H}(\mathbf{y})=H_{1}}
> \frac{P_{0}(C_{10}-C_{00})}{P_{1}(C_{01}-C_{11})}
> =:
> \eta,
> $$
> i.e. the decision is $H_{1}$ if $L(\mathbf{y})>\eta$ and $H_{0}$ if $L(\mathbf{y})<\eta$, and arbitrary if equal.

^ae30e6

> [!proof]-
> In order to minimize $\varphi(f)$, we should just minimize pointwise by $\mathbf{y}$, i.e. given any observation, figure out what to predict that minimizes the expected cost
> $$
> \tilde{\varphi}(H,\mathbf{y}):=\mathbb{E}\left[ C(\mathsf{H},H)|\boldsymbol{\mathsf{y}}=\mathbf{y} \right].
> $$
> Yet for any observation we can literally compare our choices:
> $$
> \begin{align*}
> \tilde{\varphi}(H_{0},\mathbf{y})
> &=C_{00}p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{0}|\mathbf{y})
> +C_{01}p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{1}|\mathbf{y}), \\
> \tilde{\varphi}(H_{1},\mathbf{y})
> &=C_{10}p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{0}|\mathbf{y})
> +C_{11}p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{1}|\mathbf{y}).
> \end{align*}
> $$
> This comes from multiplying costs by our a posteriori probabilities. Picking the smaller one gives the decision rule
> $$
> \frac{p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{1}|\mathbf{y})}
> {p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H_{0}|\mathbf{y})}
> \mathop{\gtreqless}_{H_{0}}^{H_{1}}
> \frac{C_{10}-C_{00}}{C_{01}-C_{11}}.
> $$
> However, posteriors are proportional to priors scaled by likelihoods, so this is equivalent to
> $$
> \frac{P_{1}\cdot p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(y|H_{1})}
> {P_{0}\cdot p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(y|H_{0})}
> \mathop{\gtreqless}_{H_{0}}^{H_{1}}
> \frac{C_{10}-C_{00}}{C_{01}-C_{11}},
> $$
> which rearranges to the desired.

The left hand side $L(\mathbf{y})$ is referred to as the ==likelihood ratio==. In the case that one of the values in the fraction is $0$, decide in the obvious way. (If they're both zero, something went wrong.) ^48ae30

> [!idea]
> If we use $0$-$1$ loss, then LRT corresponds to directly comparing a posteriori probabilities. Nontrivial losses just reweight the prior by how much it hurts to misclassify that thing (e.g. if false negatives really hurt, it is equivalent to $P_{1}$ being higher).

## LRT Interpretation

The decision rule is remarkably structured. The left hand side $L(\mathbf{y})$ is computed exclusively from the observation models and data, while the right hand side $\eta$ is a precomputable threshold determined exclusively from prior probabilities and costs.

> [!definition] (Statistic)
> A ==statistic== is a real-valued function of the data.

^8317c7

Other names are ==features== or ==embeddings==. More remarkably, $L(\mathbf{y})$ contains *all the information we need* from the models and data in order to make the optimal decision. This is what we refer to as a [[Sufficient Statistics|sufficient statistic]]. We could forget everything else!

> [!remark]
> Any invertible function of $L(\mathbf{y})$ would also be a sufficient statistic.

A useful choice is $\log(\bullet)$, which allows us to rewrite the LRT as
$$
\log(L(\mathbf{y}))\mathop{\gtreqless}_{H_{0}}^{H_{1}}\log(\eta),
$$
as log-likelihoods are easier to compute with.

## MAP and ML Decision Rules

In the case that we are using a $0$-$1$ loss function, the Bayes' risk is equivalent to 
$$
\varphi(\hat{H})=\mathbb{P}(\hat{H}(\boldsymbol{\mathsf{y}})=H_{0},\mathsf{H}=H_{1})
+ \mathbb{P}(\hat{H}(\boldsymbol{\mathsf{y}})=H_{1},\mathsf{H}=H_{0}),
$$
i.e. the likelihood of error. This allows us to specialize the LRT:

> [!claim] (MAP minimizes error probability)
> The minimum probability-of-error decision rule takes the form
> $$
> \hat{H}(\mathbf{y})=\mathop{\text{argmax}}_{H\in \{ H_{0},H_{1} \}}p_{\mathsf{H}|\boldsymbol{\mathsf{y}}}(H|\mathbf{y}),
> $$
> i.e. the ==maximum a posteriori (MAP)== decision rule.

If we further assume that the prior probabilities are equal, this is equivalent to the ==maximum likelihood (ML)== decision rule, which just takes the argmax of the likelihood function.

## Multi-way Classification

Everything generalizes, we just need a larger cost matrix $C_{ij}$; our goal is still to minimize Bayes' risk. It turns out a sufficient statistic consists of the $M-1$ features
$$
L_{m}(\mathbf{y})
:=\frac{p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{m})}
{p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0})},\quad
m=1,\dots,M-1.
$$
After all, this gives all ratios between the likelihoods.

---

**Next:** [[OC-LRT]]