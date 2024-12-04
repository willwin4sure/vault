This story was mostly told in [[Production and Costs|14.01]] as well.

From an economic view, firms can be modeled as black boxes with two inputs capital and labor and an output of goods via a production function $q=f(k,l)$.

> [!warning]
> Production functions are very similar in analysis to utility functions, but the *cardinality has physical significance*! 

One example of a production function is the Cobb-Douglas $f(k,l)=k^{\alpha}l^{\beta}$. Here is another one:

> [!example] (Constant elasticity of substitution)
> $$
> f(k,l)=\left[ ak^{\beta}+bl^{\beta} \right]^{\gamma/\beta}.
> $$

I'll give some names for properties you've seen in 14.01.

> [!definition] (Marginal and average product)
> * The ==marginal product of labor== is $\frac{ \partial f }{ \partial l }$. This is diminishing if $\frac{ \partial^{2}f }{ \partial l^{2} }<0$.
> * The ==average product of labor== is $\frac{f}{l}$. This is diminishing if it goes down as $l$ increases.

> [!definition] (Returns to scale)
> Compare $f(sk,sl)$ to $sf(k,l)$ to get whether the ==returns to scale== are increasing (greater), constant (equal), or decreasing (lesser).

^21aad1

> [!definition] (Marginal rate of technical substitution)
> This is the rate at which you substitute capital for labor along isoquants.
> $$
> MRTS = \frac{\partial f / \partial l}{\partial f / \partial k}.
> $$

Here is something new:

> [!definition] (Elasticity of substitution)
> This is equal to the percent change in $\frac{k}{l}$ over the percent change in $MRTS$.

> [!example]
> * $f(k,l)=\min(ak,bl)$ has zero elasticity of substitution.
> * $f(k,l)=k^{a}l^{b}$ has unit elasticity of substitution.
> * $f(k,l)=ak+bl$ has infinite elasticity of substitution.

---

**Next:** [[Cost Minimization]]