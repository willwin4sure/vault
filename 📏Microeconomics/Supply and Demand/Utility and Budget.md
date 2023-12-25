In this section, we build a basic model of how consumers make choices, which will determine a [[Supply and Demand Curves#^f766a5|demand curve]].

The short story is this: consumers want to maximize their own <span style="color:#0088ff">utility</span>, subject to <span style="color:#0088ff">budget constraints</span>. Solving this constrained optimization problem amounts to a simple exercise in Lagrange multipliers.
## Axioms of Preference

Suppose that consumers can choose to spend their money on a variety of goods. The particular goods they end up purchasing forms a bundle. In this section, we make three assumptions on how consumers weigh different bundles.

1. **Completeness:** when comparing two bundles, you either prefer one, prefer the other, or are indifferent. There is no such thing as "I'm not sure".
2. **Transitivity:** If bundle A is preferable to bundle B, and bundle B is preferable to bundle C, then bundle A is preferable to bundle C.
3. **Non-satiation:** a bundle with strictly more of every good is always preferred.

## Utility Functions

> [!definition] Definition (Indifference curves)
> <span style="color:#0088ff">Indifference curves</span> consists of groups of bundles where the individual is indifferent between them all.

^b7bdc2

> [!claim]
> The following properties hold. As an exercise, derive them from the axioms.
> 1. Consumers prefer higher indifference curves.
> 2. Indifference curves are downward-sloping.
> 3. Indifference curves never cross, so there is one indifference curve through each bundle.

We can represent indifference curves as the level curves of utility functions. For simplicity, we assume the consumer is only distributing their budget across two goods.

> [!definition] Definition (Utility function)
> Suppose we have two goods $X$ and $Y$, and you buy $x$ units of one and $y$ units of the other. A <span style="color:#0088ff">utility function</span> $\mathcal{U}:\mathbb{R}\times \mathbb{R}\to \mathbb{R}$ maps every bundle to some number of <span style="color:#0088ff">utils</span>. This encodes preferences as an ordinal ranking: if some bundle has more utils, it is preferred.

> [!example]
> If I bought $S$ slices of pizza and $C$ cookies, my utility function could be $\mathcal{U}(S,C)=\sqrt{ SC }$.

> [!idea]
> In general, we assume there is **diminishing marginal utility of consumption**: the amount of utility you gain from the fifth cookie you consume is lower than the amount of utility you gain from the first cookie you consume. Another example is [[Real-World Examples#^950c70|how stores price by size]].

^937ed8

In the above example, this is represented by the fact that the <span style="color:#0088ff">marginal utility</span>
$$
MU_{S}=\frac{ \partial \mathcal{U} }{ \partial S }=\frac{\sqrt{ C }}{2\sqrt{ S }}
$$
is decreasing in $S$ (analogous for $C$).
## Budget Constraints

As a simplifying example, we will assume that a consumer's budget $B$ is equal to their income. This is true for many Americans, who live paycheck to paycheck.

> [!definition] Definition (Budget constraint)
> If the price of good $X$ is $p_X$, and the price of good $Y$ is $p_{Y}$, then our <span style="color:#0088ff">budget constraint</span> is
> $$
> x\cdot p_{X}+y\cdot p_{Y}=B.
> $$
> Note the equality sign (instead of $\leq$), since we assume that consumers spend all their budget.

^c6e391

In general, budget constraints can look weirder, for example if the government gives you a certain amount of free food credits, or if you receive a coupon that discounts purchases of a good above a certain quantity. In general, all budget constraints in this class will be piecewise linear, and you can just casework on each section.
## Solving Constrained Choice

This is now a standard Lagrange multipliers problem: we want to maximize the utility $\mathcal{U}(x,y)$ subject to the budget constraint $x\cdot p_{X}+y\cdot p_{Y}=B$. This happens exactly when the slopes match.

Here's a picture:

![[optimal_bundle.png|center|256]]


Economists have some funny names for these slopes, so I may as well introduce them.

> [!definition] Definition (MRS and MRT)
> The <span style="color:#0088ff">marginal rate of substitution</span> is the slope of the utility curve, and is equal to $-\frac{MU_{X}}{MU_{Y}}$ (check this!). The <span style="color:#0088ff">marginal rate of transformation</span> is the slope of the budget constraint, and is equal to $-\frac{p_{X}}{p_{Y}}$. 

To solve for constrained choice, you set the MRS and MRT equal.

> [!example]
> Suppose we have the utility function $\mathcal{U}(x,y)=x^{1/3}y^{2/3}$, the prices are $p_{X}=1$ and $p_{Y}=2$, and your budget is $B=1$. Solve for the optimal bundle $(x^{*},y^{*})$.

> [!proof]- Solution
> Setting the MRS and MRT equal gives $3xp_{X}=2yp_{Y}$. Plugging into the budget constraint gives 
> $$
> xp_{X}=\frac{2}{5}B\quad\text{and}\quad yp_{Y}=\frac{3}{5}B.
> $$
> Therefore, the optimal bundle is $(x^{*},y^{*})=\left( \frac{2}{5}, \frac{3}{10} \right)$.

This showcases a common feature of utility functions of this shape: the optimal bundle will involve the consumer spending some fraction of their budget on each good, regardless of the price (it only depends on their preferences).

^5f0f0d

For a real-world example with a kinked budget constraint, check out [[Real-World Examples#^442f37|SNAP]]. 

This optimal bundle represents how much of each good the consumer is going to buy. This will allow us to derive demand curves.

---

**Next:** [[Properties of Demand Curves]]