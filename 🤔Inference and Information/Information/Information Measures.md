In the [[Generalized Bayesian Decision Theory|last section]], we defined a cost function for generalized Bayesian decision theory. We will focus on the log-loss criterion $C(x,q)=-\log q(x)$: we took $A=1$ and $B(\bullet)=0$. This logarithm can either be taken base $2$ or base $e$, depending on if we want to measure the cost in bits or nats.

## Entropy

You've seen this before, but it actually falls out of looking at the minimum expected cost under no observations. In this case, $p_{\mathsf{x}|\mathsf{y}}(\bullet|\varnothing)=p_{\mathsf{x}}(\bullet)$. Since the log-loss cost criterion is proper, we know that the prior $p_{\mathsf{x}}$ achieves the minimum. 

Therefore, the prior expected cost is
$$
\mathbb{E}[C(\mathsf{x},p_{\mathsf{x}})]=-\mathbb{E}[\log p_{\mathsf{x}}(\mathsf{x})]=-\sum_{a}^{}p_{\mathsf{x}}(a)\log p_{\mathsf{x}}(a).
$$

> [!definition] (Entropy)
> The ==entropy==, or ==self-information== of the random variable $\mathsf{x}$ is defined as
> $$
> H(\mathsf{x})=-\sum_{a}^{}p_{\mathsf{x}}(a)\log p_{\mathsf{x}}(a),
> $$
> where we use the convention that $0\cdot \log 0 = 0$.

> [!idea]
> This is a measure of the average uncertainty in $\mathsf{x}$. For example, if $\mathsf{x}$ is deterministic, the entropy is $0$. If $\mathsf{x}$ is uniform, then the entropy is maximized.
> 

> [!claim]
> Let $\mathsf{x}$ be a discrete random variable defined over an alphabet $\mathcal{X}$. Then,
> $$
> 0\leq H(\mathsf{x})\leq \log|\mathcal{X}|,
> $$
> where the lower bound is tight if and only if $\mathsf{x}$ is deterministic, and the upper bound is tight when $\mathsf{x}$ is uniformly distributed over $\mathcal{X}$.

For some more intuition, here is the entropy of a Bernoulli random variable $\mathtt{B}(p)$ as a function of $p$:

![[entropy_bernoulli.png|center|256]]

## Conditional Entropy

Alright, what happens when we observe $\mathsf{y}=y$? Well, the expected log-loss is still minimized at the posterior $p_{\mathsf{x}|\mathsf{y}}(\bullet|y)$, so we can write it as
$$
\mathbb{E}[C(x,p_{\mathsf{x}|\mathsf{y}})|\mathsf{y}=y]=-\mathbb{E}[\log p_{\mathsf{x}|\mathsf{y}}(\mathsf{x}|\mathsf{y})|\mathsf{y}=y]=-\sum_{a}^{}p_{\mathsf{x}|\mathsf{y}}(a|y)\log p_{\mathsf{x}|\mathsf{y}}(a|y).
$$
> [!definition] (Conditional entropy on a particular observation)
> We define the ==conditional entropy== of $\mathsf{x}$ given a *particular* observation $\mathsf{y}=y$ as
> $$
> H(\mathsf{x}|\mathsf{y}=y)=-\sum_{a}^{}p_{\mathsf{x}|\mathsf{y}}(a|y)\log p_{\mathsf{x}|\mathsf{y}}(a|y).
> $$

Now, we can consider averaging this over all possible observations of $\mathsf{y}$. This gives
$$
\begin{align}
\mathbb{E}[C(\mathsf{x},p_{\mathsf{x}|\mathsf{y}})]
&=\mathbb{E}\left[ \mathbb{E}[C(\mathsf{x},p_{\mathsf{x}|\mathsf{y}}(\mathsf{x}|\mathsf{y}))|\mathsf{y}] \right] \\
&=\sum_{y}^{}p_{\mathsf{y}}(y)H(\mathsf{x}|\mathsf{y}=y) \\
&=-\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{x}|\mathsf{y}}(a|b).
\end{align}
$$
> [!definition] (Conditional entropy)
> We define the ==conditional entropy== of $\mathsf{x}$ given $\mathsf{y}$ as
> $$
> H(\mathsf{x}|\mathsf{y})=-\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{x}|\mathsf{y}}(a|b).
> $$

Note that this value lies between $0$ and $H(\mathsf{x})$. Equality for the lower bound occurs when $\mathsf{x}$ is a deterministic function of $\mathsf{y}$, while equality for the upper bound occurs when $\mathsf{x}$ and $\mathsf{y}$ are independent.

> [!idea]
> When you learn more, on average your uncertainty never increases.

In particular, we could have that $H(\mathsf{x}|\mathsf{y}=y)>H(\mathsf{x})$ for a particular observation $y$. Yet if we average over $y$ then it is not true.

> [!claim] (Chain rule of entropy)
> If $\mathsf{z}=(\mathsf{x},\mathsf{y})$, then $H(\mathsf{z})=H(\mathsf{x}|\mathsf{y})+H(\mathsf{y})$.

> [!proof]-
> Just compute
> $$
> \begin{align}
> -\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{x},\mathsf{y}}(a|b)-\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{y}}(b)
> &= -\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{x}}(a) \\
> &= -\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log p_{\mathsf{x},\mathsf{y}}(a,b),
> \end{align}
> $$
> as desired. 

## Mutual Information

The average cost reduction achieved by making an observation is
$$
\Delta \mathbb{E}[C(\mathsf{x},q)]=H(\mathsf{x})-H(\mathsf{x}|\mathsf{y}).
$$

> [!definition] (Mutual information)
> We define the ==mutual information== of $\mathsf{x}$ and $\mathsf{y}$ as
> $$
> I(\mathsf{x};\mathsf{y})=\sum_{a,b}^{}p_{\mathsf{x},\mathsf{y}}(a,b)\log \frac{p_{\mathsf{x},\mathsf{y}}(a,b)}{p_{\mathsf{x}}(a)p_{\mathsf{y}}(b)}.
> $$

Note that this is symmetric in $\mathsf{x}$ and $\mathsf{y}$, and it is equal to the quantity above.

> [!idea]
> In summary:
> 
> * Entropy is optimal expected cost of $\mathsf{x}$ under no observations.
> * Conditional entropy is optimal expected cost of $\mathsf{x}$ under observing $\mathsf{y}$.
> * Mutual information is the expected cost reduction by observing $\mathsf{y}$.

Here is a Venn diagram that suggests a set theoretic analogue for these concepts:

![[entropy_venn_diagram.png|center|384]]

> [!proposition] (Basic properties of mutual information)
> 1. $I(\mathsf{x},\mathsf{y})\geq 0$.
> 2. $I(\mathsf{x};\mathsf{y})=0$ if and only if $\mathsf{x}$ is independent of $\mathsf{y}$.
> 3. $I(\mathsf{x};\mathsf{y})=I(\mathsf{y};\mathsf{x})$.
> 4. $I(\mathsf{x};\mathsf{y},\mathsf{z})=I(\mathsf{x};z)+I(\mathsf{x};\mathsf{y}|\mathsf{z})$. 

Note that $I(\mathsf{x};\mathsf{y}|\mathsf{z})=H(\mathsf{x}|\mathsf{z})-H(\mathsf{x}|\mathsf{y},\mathsf{z})$ is our definition of conditional mutual information.

---

**Next:** [[The Data Processing Inequality]]