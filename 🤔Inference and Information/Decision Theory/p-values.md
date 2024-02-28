> Many tests have a significance level of 0.05. If the p-value of the data is at most 0.05, we mark it as statistically significant and reject the null.

We will restrict our attention to the case where the data $\boldsymbol{\mathsf{y}}$ is governed by bounded PDFs under each hypothesis, and where the likelihood ratio $L(\boldsymbol{\mathsf{y}})$ is a purely continuous random variable (no discrete components) under each hypothesis.

In this case, the [[OC-LRT#Classification Performance Measures|measures]] $P_{F}(\eta)$ and $P_{D}(\eta)$ for the LRT are continuous, decreasing functions of the threshold $\eta$.

Therefore, we can rewrite the LRT as 
$$
p_{*}(\mathbf{y}):=P_{F}(L(\mathbf{y}))\mathop{\gtreqless}_{H_{1}}^{H_{0}}P_{F}(\eta)=:\alpha.
$$

> [!definition] ($p$-value, significance level)
> The feature $p_{*}(\mathbf{y})\in[0,1]$ is referred to as the ==$p$-value== of the data, while $\alpha$ is referred to as the ==significance level== of the test.

> [!idea]
> The $p$-value is just the probability under the null hypothesis that the likelihood ratio is at least as extreme as observed, i.e. $\mathbb{P}(L(\boldsymbol{\mathsf{y}})\geq L(\mathbf{y})|\mathsf{H}=H_{0})$.

To run significance testing, first assign the level $\alpha$ (e.g. 0.05 is a common choice), and then compute the $p$-value for the data. Check if this is less than $\alpha$.

> [!claim]
> The $p$-value is uniformly distributed under $H_{0}$.

> [!proof]-
> It's just the percentile of the data. We can check that
> $$
> \mathbb{P}(p_{*}(\boldsymbol{\mathsf{y}}\leq\alpha|\mathsf{H}=H_{0}))
> =\mathbb{P}(L(\boldsymbol{\mathsf{y}})\geq \eta|\mathsf{H}=H_{0})=\alpha.
> $$

---

**Next:** [[Neyman-Pearson Hypothesis Testing]]