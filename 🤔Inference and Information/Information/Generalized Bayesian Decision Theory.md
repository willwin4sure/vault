> Output a distribution instead of a decision. NLL loss is canonical.

In our initial formulation of [[Bayesian Hypothesis Testing|Bayesian hypothesis testing]], we took an observation model $p_{\mathsf{y}|\mathsf{x}}(\bullet|\bullet)$, some data $\mathsf{y}$, as well as a prior $p_{\mathsf{x}}(\bullet)$ and cost criterion $C(\bullet,\bullet)$ in order to produce a "hard" decision $\hat{x}(y)$.

Now, we will consider the more general case where you are allowed to output a "soft" decision in the form of a probability distribution $q(\bullet)$ over $\mathcal{X}$. This is analogous to your classification neural network outputting some softmax-ed value.

Note that in this section, we restrict to cases where $\mathcal{X}$ and $\mathcal{Y}$ are both discrete alphabets.

## Cost Functions

One approach would be to just use an MSE loss.

> [!definition] (Quadratic cost criterion)
> The ==quadratic cost criterion== is given by
> $$
> C(x,q)=A\sum_{a}^{}(q(a)-\mathbf{1}_{a=x})^{2}+B(x),
> $$
> where $A>0$ is constant and $B(\bullet)$ is arbitrary.

Here, we are using the Euclidean distance between $\mathbf{q}$ and the unit vector along dimension $x$.

A more sophisticated approach is the log-loss criterion, also from intro ML.

> [!definition] (Log-loss criterion)
> The ==log-loss criterion== is given by
> $$
> C(x,q)=-A\log q(x)+B(x),
> $$
> where $A>0$ is constant and $B(\bullet)$ is arbitrary. 

## Properties

> [!definition] (Proper cost functions)
> A cost function $C(\bullet,\bullet)$ is ==proper== if
> $$
> p_{\mathsf{x}|\mathsf{y}}(\bullet|y)=\mathop{\arg\min}_{\left\{  q(\bullet)\geq 0: \sum_{a}^{}q(a)=1 \right\}}\mathbb{E}[C(x,q)|\mathsf{y}=y]
> $$
> for all $y\in \mathcal{Y}$. This is saying that the true Bayesian belief $p_{\mathsf{x}|\mathsf{y}}$ is being reported if the cost is minimized.

> [!claim]
> The log-loss cost criterion is proper.

^eb0347

> [!proof]- Gibbs'
> The expected cost is
> $$
> \mathbb{E}_{p_{\mathsf{x}|\mathsf{y}}(\bullet|y)}\left[ -A\log q(\mathsf{x})+B(\mathsf{x}) \right].
> $$
> We can only minimize the first term, which is equivalent to maximizing $\mathbb{E}_{p_{\mathsf{x}|\mathsf{y}}(\bullet|y)}\left[ \log q(\mathsf{x}) \right]$, yet by [[Some Useful Inequalities#^e52776|Gibbs' inequality]] this occurs when $q=p_{\mathsf{x}|\mathsf{y}}$.

> [!definition] (Local cost functions)
> A cost function $C(\bullet,\bullet)$ is ==local== if there exists a function $\phi:\mathcal{X}\times \mathbb{R}\to \mathbb{R}$ such that $C(x,q)=\phi(x,q(x))$.

This just says that the cost function only depends on the value of the outputted distribution $q(\bullet)$ on the true answer.

> [!claim]
> The log-loss cost criterion is local.

Other costs are proper, and other costs are local. Yet the log-loss is the only smooth one that is both.

> [!theorem]
> When the alphabet $\mathcal{X}$ consists of at least three values, then the log-loss is the only smooth, local, proper cost function.

> [!proof]-
> If the cost function is local, we just want to find values of $q_{\ell}$ such that
> $$
> (p_{1},\dots,p_{L})=\mathop{\arg\min}_{\mathbf{q}}\sum_{\ell=1}^{L}p_{\ell}\phi_{\ell}(q_{\ell}).
> $$
> Here, $p_{\ell}$ form the posterior distribution while $q_{\ell}$ are our predictions; the cost function is local so we can express it in that form. However, by Lagrange multipliers, optimizing this under the constraint that $\sum_{\ell=1}^{L}q_{\ell}=1$ gives
> $$
> p_{\ell}\frac{ \partial \phi_{\ell} }{ \partial q_{\ell} } (p_{\ell})=-\lambda\implies \phi_{\ell}(p_{\ell})=-\lambda \log p_{\ell}+c_{\ell},
> $$
> which finishes.

---

**Next:** [[Information Measures]]