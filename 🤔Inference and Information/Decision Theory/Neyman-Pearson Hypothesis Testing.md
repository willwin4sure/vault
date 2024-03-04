> Put an upper bound on the false-alarm rate and maximize the detection rate.

Sometimes it's hard to specify priors and costs to run the Bayesian approach.

An alternative idea is to maximize $P_{D}$ subject to some constraint on $P_{F}$, i.e.
$$
\text{argmax}_{\hat{H}(\bullet)}P_{D}\quad\text{subject to }P_{F}\leq \alpha.
$$
This is called the ==Neyman-Pearson criterion==.

> [!theorem] (Neyman-Pearson Lemma, Deterministic Version)
> Suppose the likelihood ratio $L(\boldsymbol{\mathsf{y}})$ is a purely continuous random variable (no discrete components) under each hypothesis. Then, a solution to the Neyman-Pearson criterion among deterministic rules is a [[The Likelihood Ratio Test|LRT]] of the form
> $$
> \hat{H}(y)=H_{\mathbf{1}_{L(\boldsymbol{\mathsf{y}})\geq \eta}},
> $$
> where $\eta$ is chosen so that
> $$
> P_{F}=\mathbb{P}(L(\boldsymbol{\mathsf{y}})\geq \eta|\mathsf{H}=H_{0})=\alpha.
> $$

^125f6d

> [!idea]
> This really isn't deep. For each observation $y$, we can decide whether or not to place it into our positive decision region $\mathcal{Y}_{1}=\{ y\in \mathcal{Y} : \hat{H}(y)=H_{1} \}$. Every time we place something into this decision region, it accrues some detection probability $dP_{D}$ from its likelihood under $H_{1}$, and some false-alarm probability $dP_{F}$ from its likelihood under $H_{0}$.
> 
> Therefore, if we want to maximize $P_{D}$ while limiting $P_{F}$, we want to pick observations $y$ with the highest likelihood ratio. 

> [!proof]- Rigor
> Let $\mathcal{Y}_{1}=\{ \mathbf{y}\in \mathcal{Y} : \hat{H}(\mathbf{y})=H_{1} \}$ denote the decision region of any decision rule $\hat{H}$, and suppose it has detection and false-alarm probabilities of $P_{D}$ and $P_{F}$, and let $\mathcal{Y}_{1}^{\eta}$ denote the decision region of $\hat{H}^{\eta}$ with performance measures $P_{D}^{\eta}$ and $P_{F}^{\eta}$. 
> 
> We will show that whenever $P_{F}\leq P_{F}^{\eta}$, we have that $P_{D}\leq P_{D}^{\eta}$ as well: for any rule with a lower false-alarm rate, it also has a lower detection rate. In particular, we will show that
> $$
> \int_{\mathcal{Y}_{1}^{\eta}} p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{1}) \, d\mathbf{y}
> -\int_{\mathcal{Y}_{1}}p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{1}) \, d\mathbf{y}
> \geq \eta \left[ 
> \int_{\mathcal{Y}_{1}^{\eta}}p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0}) \, d\mathbf{y}
> -\int_{\mathcal{Y}_{1}}p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0}) \, d\mathbf{y} 
> \right].
> $$
> However, this is equivalent to
> $$
> \int_{\mathcal{Y}}(\mathbf{1}_{L(\mathbf{y})\geq \eta}-\mathbf{1}_{\mathbf{y}\in \mathcal{Y}_{1}})(L(\mathbf{y})-\eta)p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\mathbf{y}|H_{0}) \, d\mathbf{y}\geq 0,
> $$
> which is true since the integrand is always nonnegative.

> [!idea]
> Just cut $P_{F}=\alpha$, and then read off the maximal $P_{D}$ along the OC-LRT.

There are a lot of practical scenarios where this is not useful. For example, if one of the distributions under a hypothesis is discrete. Further, simple LRTs are only optimal among deterministic rules, and randomized classifiers could do better. 

Some extra related reading:

1. [[p-values]]

---

**Next:** [[Discontinuous OC-LRTs]]