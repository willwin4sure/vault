> Assume that nature is adversarial, and responds to your selected decision rule with the worst-case prior. You want your decision rule to be unexploitable/balanced. This is often a bit too conservative in practice.

You have the costs, but not the prior (as we've mentioned, it might be hard to specify). For any decision rule, we will evaluate it by the **worst-case** prior that it could face.

## The Minimax Criterion

Our goal is to find a randomized decision rule characterized by
$$
r_{M}(\bullet)=\mathop{\text{argmin}}_{r:\mathcal{Y}\to[0,1]}\ \max_{p \in[0,1]}\varphi(p,r),
$$
where $\varphi(p,r)$ is the Bayes' risk if the prior is $p=\mathbb{P}(\mathsf{H}=H_{1})$ and our rule is $r(\bullet)$.

## Mismatch of Bayesian Decision Rules

We know from [[Randomized Decision Rules|previous sections]] that all decision rules on the efficient frontier take the form of a randomized likelihood ratio test
$$
r_{B}(\mathbf{y};p,\lambda):=
\begin{cases}
1 & L(\mathbf{y})>\eta \\
\lambda & L(\mathbf{y})=\eta \\
0 & L(\mathbf{y})<\eta
\end{cases},\quad \text{where} \quad
\eta:=\left( \frac{1-p}{p} \right) \left( \frac{C_{10}-C_{00}}{C_{01}-C_{11}} \right),
$$
which is a Bayesian decision rule $\hat{H}_{B}$ matched to a prior $p \in [0,1]$, where $\lambda \in[0,1]$ is the randomization parameter. This gives the associated false-alarm and detection probabilities of
$$
P_{F}^{B}(p,\lambda)=\mathbb{P}(L(\boldsymbol{\mathsf{y}})>\eta|\mathsf{H}=H_{0})+\lambda \mathbb{P}(L(\boldsymbol{\mathsf{y}})=\eta|\mathsf{H}=H_{0})
$$
and
$$
P_{D}^{B}(p,\lambda)=\mathbb{P}(L(\boldsymbol{\mathsf{y}})>\eta|\mathsf{H}=H_{1})+\lambda \mathbb{P}(L(\boldsymbol{\mathsf{y}})=\eta|\mathsf{H}=H_{1}).
$$

> [!definition] (Mismatch Bayes' risk)
> We define the ==mismatch Bayes' risk== as 
> $$
> \varphi_{B}(p,q,\lambda):=\varphi(p,r_{B}(\bullet;q,\lambda)).
> $$
> This is the expected cost of assuming the prior is $q$ when the prior is actually $p$.

By contrast, we call $\varphi_{B}^{*}:=\varphi_{B}(p,p,\lambda)$ the ==matched Bayes' risk==.

> [!claim] (Some obvious properties)
> 1. $\varphi_{B}(\bullet,q,\lambda)$ is a linear function.
> 2. $\varphi_{B}(p,q,\lambda)$ is lower bounded by $\varphi_{B}^{*}(p)$ with equality if $q=p$.
> 3. $\varphi_{B}^{*}(\bullet)$ is concave and continuous on $[0,1]$.
> 4. $\varphi_{B}^{*}(0)=C_{00}$ and $\varphi_{B}^{*}(1)=C_{11}$.

Here is a picture:

![[minimaxbayesrisk.png|center|384]]

All curves are functions of $p$. The blue curve is the Bayes' assuming optimal design against a particular prior $p$. The red and green lines are what happens when you fix a $q_{1}$ or $q_{2}$ as a design for your decision rule, and then vary the true prior $p$ against it.

> [!idea]
> Our exploitability for any decision rule is the maximum height of the line (e.g. red or green). This will always occur when $p=0$ or $p=1$. Basically, because nature moves second, it can respond deterministically.

Points of non-differentiability correspond to places where the optimum Bayes decision rule is achieved at a non-unique point on the efficient frontier. Then, there are various randomization parameters $\lambda$ we could choose to vary the slope. This doesn't affect the matched Bayes' risk, but it changes our exploitability.

How should we be the least exploitable? Just be flat at the max of $\varphi_{B}^{*}(p)$! This should give a minimax risk of $\max_{p}\varphi_{B}^{*}(p)$, as you will see below. 

## The Minimax Decision Rule

> [!theorem] (Minimax Hypothesis Testing)
> Given data models $p_{\boldsymbol{\mathsf{y}}|\mathsf{H}}(\bullet|H_{i})$ for $i\in \{ 0,1 \}$, and valid costs $C_{ij}$ for $i,j\in \{ 0,1 \}$, a minimax decision rule is
> $$
> r_{M}(\bullet)=r_{*}(\bullet):=r_{B}(\bullet;p_{*},\lambda_{*})
> $$
> for some $(p_{*},\lambda_{*})\in[0,1]^{2}$, where $p_{*}$ is the corresponding minimax prior, i.e.
> $$
> \min_{r:\mathcal{Y}\to[0,1]}\max_{p \in[0,1]}\varphi(p,r)=\varphi(p_{*},r_{*})=\varphi_{B}^{*}(p_{*}).
> $$
> When there exists $P_{F}^{*}\in[0,1]$ such that
> $$
> \zeta_{NP}(P_{F}^{*})=g_{M}(P_{F}^{*})
> $$
> with $g_{M}$ denoting the linear function
> $$
> g_{M}(P_{F}):=\left( \frac{C_{01}-C_{00}}{C_{01}-C_{11}} \right) - \left( \frac{C_{10}-C_{00}}{C_{01}-C_{11}} \right) P_{F},
> $$
> then it corresponds to an optimizing pair $(p_{*},\lambda_{*})$, i.e. $P_{F}^{B}(p_{*},\lambda_{*})=P_{F}^{*}$ and $P_{D}^{B}(p_{*},\lambda_{*})=\zeta_{NP}(P_{F}^{*})$. Otherwise,
> $$
> p_{*}=\begin{cases}
> 0 & \text{if }\zeta_{NP}(P_{F})>g_{M}(P_{F})\text{ for all }P_{F}\in[0,1] \\
> 1 & \text{if }\zeta_{NP}(P_{F})<g_{M}(P_{F})\text{ for all }P_{F}\in[0,1]
> \end{cases}.
> $$
> 

Okay, sure, that's a ton of waffles. Now let me explain what's going on. Here's a picture of the decision rule:

![[minimaxdecision.png|center|384]]

In particular, the decision rule is wherever the Neyman-Pearson function intersects the depicted red linear function. For example, in the case where $C_{ij}$ is $0$-$1$ loss, the red line would just have slope $-1$ from $(0,1)$ to $(1,0)$. 

> [!idea]
> The red line is the locus of decision rules where nature is indifferent between all selections of priors.

In particular, nature is indifferent if the expected Bayes' risk from the world always being $H_{0}$ or $H_{1}$ are the same. This occurs if and only if
$$
P_{F}C_{10}+(1-P_{F})C_{00} = P_{D}C_{11}+(1-P_{D})C_{01},
$$
which simplifies to
$$
P_{F}(C_{10}-C_{00})+P_{D}(C_{01}-C_{11})=C_{01}-C_{00}.
$$
In particular, this matches the linear function $P_{D}=g_{M}(P_{F})$ in the theorem above.

The extra stuff at the end of the theorem just accounts for cases where the red line lies entirely above or below the blue curve: then, just go to one of the extremes of $\{ 0,1 \}$ to optimize.

> [!claim] (Risk function exhibits a saddle point)
> $$
> \min_{r:\mathcal{Y}\to[0,1]}\max_{p \in[0,1]}\varphi(p,r)=\max_{p \in[0,1]}\min_{r:\mathcal{Y}\to[0,1]}\varphi(p,r).
> $$

Generically, the left hand side should be at least than the right hand side (the inner max/min always has dominance since it can depend on the outer one). 

However, we can write that
$$
\min_{r:\mathcal{Y}\to[0,1]}\max_{p \in[0,1]}\varphi(p,r)=\max_{p \in[0,1]}\varphi(p,r_{*}).
$$
Generically, we know that $\varphi(p,r_{*})\geq \min_{r:\mathcal{Y}\to[0,1]}\varphi(p,r)$. However, we know that nature is indifferent at equilibrium, which means that $\varphi(p,r_{*})$ is constant over $p$. Further, we know that there exists some prior $p_{*}$ which gives $\varphi(p_{*},r_{*})=\min_{r:\mathcal{Y}\to[0,1]}\varphi(p_{*},r)$, which shows that equality holds.

---

**Next:** [[⛺Decision Theory Homepage]]