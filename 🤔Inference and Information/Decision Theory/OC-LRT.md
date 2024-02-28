> You can bias towards more detections, but this will cause more false alarms.
## Classification Performance Measures

The performance of any decision rule $\hat{H}(\bullet)$ may be fully specified by two quantities.

> [!definition] (False-alarm and detection)
> The ==false-alarm== and ==detection== probabilities are respectively defined as
> $$
> \begin{align*}
> P_{F}:=\mathbb{P}(\hat{H}(\boldsymbol{\mathsf{y}})=H_{1}|\mathsf{H}=H_{0}), \\
> P_{D}:=\mathbb{P}(\hat{H}(\boldsymbol{\mathsf{y}})=H_{1}|\mathsf{H}=H_{1}).
> \end{align*}
> $$

This terminology originated from the radar community. High $P_{D}$ is good, high $P_{F}$ is bad.

> [!notation]
> In the statistics community, we may use ==Type I== or ==Type II== error rates $P_{E}^{1}=P_{F}$ and $P_{E}^{2}=1-P_{D}$. These are also called false positives and false negatives, respectively. 
> 
> In some machine learning communities, $P_{D}$ is referred to as the ==recall== or ==sensitivity==, while the quantity
> $$
> P_{P}:=\mathbb{P}(\mathsf{H}=H_{1}|\hat{H}(\boldsymbol{\mathsf{y}})=H_{1})
> $$
> is referred to as the ==precision== or ==positive predictive value==. Unlike the other measures above, this one depends on a choice of priors.

## Operating Characteristic

Consider the family of LRT decision rules
$$
\{ \hat{H}(\bullet)=H_{\mathbf{1}_{L(\bullet)\geq \eta}},\eta \in \mathbb{R} \}
$$
by varying our threshold $\eta$. Each $\eta$ specifies a decision rule associated with an operating point $(P_{F}(\eta),P_{D}(\eta))$ in the $(P_{F},P_{D})$ plane.

> [!definition] (OC-LRT)
> The collection of points $(P_{F}(\eta),P_{D}(\eta))$ is referred to as the ==operating characteristic== of the LRT (==OC-LRT==).

Note that $\lim_{ \eta \to 0 }(P_{F}(\eta),P_{D}(\eta))=(1,1)$ and $\lim_{ \eta \to \infty }(P_{F}(\eta),P_{D}(\eta))=(0,0)$. Here is a representative diagram:

![[oclrt.png|center|256]]

The best decision rules are in the top-left, while the worst are in the bottom-right. The OC-LRT specifies a tradeoff.

> [!claim]
> The Bayes' optimal $\eta$ corresponds to the point on the OC-LRT where the slope is equal to 
> $$
> \frac{(C_{10}-C_{00})P_{0}}{(C_{01}-C_{11})P_{1}}.
> $$

> [!proof]- Lagrange multipliers
> The Bayes' risk is
> $$
> \varphi(f)=\sum_{i,j}^{}C_{ij}\mathbb{P}(\hat{H}(\boldsymbol{\mathsf{y}})=H_{i}|\mathsf{H}=H_{j})P_{j},
> $$
> which can be rewritten as a linear function $\varphi(f)=\alpha P_{F}-\beta P_{D}+\gamma$, where
> $$
> \alpha=(C_{10}-C_{00})P_{0},\quad \beta=(C_{01}-C_{11})P_{1},\quad \gamma=(C_{00}P_{0}+C_{11}P_{1}).
> $$
> Lagrange multipliers tells us to match the slopes to $\frac{\alpha}{\beta}$.

For example, if we want to minimize the probability of error, take the point where the slope is $1$. Also, if either the prior that the answer is $0$ is high or the cost of a false positive is high, we'd prefer a conservative threshold where both detection and false-positive probabilities are rather low.

> [!claim]
> The OC-LRT is monotonically nondecreasing.

---

**Next:** [[Neyman-Pearson Hypothesis Testing]]