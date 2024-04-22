In this section, we investigate when conjugate prior families exist.

When $\mathcal{X}$ is finite, it turns out that a conjugate prior family always exists. You can just take the entire simplex $\mathcal{P}^{\mathcal{X}}$ as a $|\mathcal{X}|-1$ parameter family! Continuous alphabets $\mathcal{X}$ are more interesting. One of these is special:

> [!definition] (Natural conjugate prior family)
> A conjugate prior family is ==natural== if for every $y \in \mathcal{Y}$ there exists a parameter value $\boldsymbol{\theta}(y)\in \Theta$ such that
> $$
> q(\bullet;\boldsymbol{\theta}(y))\propto p_{\mathsf{y}|\mathsf{x}}(y|\bullet),
> $$
> where the constant of proportionality in general depends on $y$.

^7d4845

Now, we will establish when conjugate prior families exist.

> [!theorem]
> For a model such that $\mathcal{Y}\subseteq \mathbb{R}$ is a region and $p_{\mathsf{y}|\mathsf{x}}(\bullet|x)$ is a continuous function, if a conjugate prior family exists, then for every $N\geq 1$ there exists a continuous function $t_{N}(\bullet, \dots, \bullet)$ with finite dimension independent of $N$ such that $t_{N}(y_{1},\dots,y_{N})$ is a sufficient statistic for inferences about $\mathsf{x}$.

The sufficient statistic is precisely $\boldsymbol{\theta}(y_{1},\dots,y_{N};\boldsymbol{\theta}_{0})$.

## Sufficiency Characterization of Exponential Families

Well, another reason why exponential families are beautiful and fundamental. It turns out they are the *only* families of distributions with finite-dimensional sufficient statistics, meaning also that they are the only models with conjugate priors.

> [!theorem] (Exponential family, sufficiency characterization)
> Let $p_{\mathsf{y}}(\bullet;x)$ be a family of models over alphabet $\mathcal{Y}$ that is a region of $\mathbb{R}$ with parameter set $\mathcal{X}$, such that $p_{\mathsf{y}}(y;x)>0$ for all $y \in \mathcal{Y}$ and $x \in \mathcal{X}$, and such that $p_{\mathsf{y}}(\bullet;x)$ is differentiable for each $x \in \mathcal{X}$. Moreover, let $\mathsf{y}_{1},\dots,\mathsf{y}_{N}$ be i.i.d. according to this model for a given $x \in \mathcal{X}$. Then, there is a differentiable, scalar-valued sufficient statistic $t(y_{1},\dots,y_{N})$ if and only if $p_{\mathsf{y}}(\bullet;x)$ is a one-parameter exponential family with a differentiable natural statistic.

> [!proof]-
> The "if" direction is obvious, since the sufficient statistic would be
> $$
> t_{N}(y_{1},\dots,y_{N})=\sum_{i=1}^{N}t(y_{i}).
> $$
> We omit the "only if" direction.

Milder conditions suffice. For example, differentiability can be replaced with continuity, but the analysis is more involved.

This can be extended naturally to a sequence of multidimensional observations $\mathbf{y}_{1},\dots,\mathbf{y}_{N}$ and multidimensional sufficient statistics $\mathbf{t}$. 

---

**Next:** [[Conjugate Prior Families for Exponential Families]]