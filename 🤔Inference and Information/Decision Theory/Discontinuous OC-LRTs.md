> If $L(\boldsymbol{\mathsf{y}})$ has a point mass at a point $\eta$, this results in a discontinuity in the OC-LRT.

In this section, we investigate cases where the OC-LRT is discontinuous, and how we can still optimize the Neyman-Pearson criterion by time-sharing between adjacent decision rules.
## Examples of Discontinuities

> [!example] (Binary data)
> Suppose $y\in \mathcal{Y}=\{ 0,1 \}$, where the probability mass functions are
> $$
> p_{\mathsf{y}|\mathsf{H}}(y|H_{0})=
> \begin{cases}
> \frac{1}{2} & y = 0 \\
> \frac{1}{2} & y = 1
> \end{cases},\quad
> p_{\mathsf{y}|\mathsf{H}}(y|H_{1})=
> \begin{cases}
> \frac{2}{3} & y = 0 \\
> \frac{1}{3} & y = 1
> \end{cases}.
> $$

This results in the following OC-LRT (you take neither, only $y=0$, or both):

![[binaryoclrt.png|center|256]]

> [!example] (Poisson of different rates)
> A neuroscience motivation for this example is examining the firing pattern of neurons in the visual cortex of the brain in response to the presence or absence of some stimulus. In the null hypothesis, firing patterns obey a Poisson process with rate $\mu_{0}$, while in the alternate hypothesis, firing patterns obey a Poisson process with rate $\mu_{1}>\mu_{0}$. We need to make a decision based on the number of observations $y$ in a unit of time.
> 
> The LRT can be computed to be
> $$
> L(y)=\left( \frac{\mu_{1}}{\mu_{0}} \right)^{y} e^{-(\mu_{1}-\mu_{0})}\mathop{\gtreqless}_{H_{0}}^{H_{1}}\eta, 
> $$
> which further simplifies to
> $$
> y\mathop{\gtreqless}_{H_{0}}^{H_{1}} \frac{\ln \eta+(\mu_{1}-\mu_{0})}{\ln(\frac{\mu_{1}}{\mu_{0}})}=:\gamma.
> $$
> 

^79744b

This results in the following OC-LRT:

![[poissonoclrt.png|center|256]]

Note that each point actually corresponds to a range of thresholds $\gamma$ (just like the previous example).

> [!example] (Non-discrete discontinuity)
> Discontinuities in the OC-LRT can show up even if data is continuous instead of discrete. Consider the hypotheses for some data $y\in \mathcal{Y}=[0,1]$, where $p_{\mathsf{y}|\mathsf{H}}(y|H_{0})$ is uniform and
> $$
> p_{\mathsf{y}|\mathsf{H}}(y|H_{1})=\begin{cases}
> 3y & 0\leq y<1/3 \\
> 1 & 1/3\leq y<2/3 \\
> 3y-1 & 2/3\leq y<1
> \end{cases}.
> $$

![[gapoclrt.png|center|256]]

> [!idea]
> The intuition with OC-LRTs is that as $\eta$ decreases, you grab chunks of $\mathbf{y}$ in decreasing order of $L(\mathbf{y})$ to include in your positive decision region $\mathcal{Y}_{1}$. This generally leads to a small increase in both $P_{D}$ and $P_{F}$, corresponding to the measures of the chunks of $\mathbf{y}$ under the respective hypotheses.
> 
> However, when $L(\mathbf{y})$ has a point mass, we grab a nontrivial chunk of $\mathbf{y}$ values, leading to a large jump in both $P_{D}$ and $P_{F}$, causing a discontinuity.

## Bayesian and Neyman-Pearson Criterion

In the Bayesian case, we don't care about these discontinuities. They correspond to cases in the likelihood ratio test where equality $L(\mathbf{y})=\eta$ occurs with nonzero probability. In the OC-LRT, this means that there are at least two $(P_{F},P_{D})$ operating points corresponding to a Bayesian optimal rule.

In the Neyman-Pearson case, we are rather sad: given some restriction $P_{F}<\alpha$ where $\alpha$ is within in the discontinuity, we are forced to round down to the next point on the OC-LRT, leading to a rather low $P_{D}$.

In the next section, we will show that randomization can actually interpolate between the points, solving our issue!

---

**Next:** [[Randomized Decision Rules]]