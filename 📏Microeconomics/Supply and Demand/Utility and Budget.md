In this section, we build a basic model of how consumers make choices, which will determine a [[Supply and Demand Curves#^f766a5|demand curve]].
## Axioms of Preference

Suppose that consumers can choose to spend their money on a variety of goods. The particular goods they end up purchasing forms a bundle. In this section, we make three assumptions on how consumers weigh different bundles.

1. **Completeness:** when comparing two bundles, you either prefer one, prefer the other, or are indifferent. There is no such thing as "I'm not sure".
2. **Transitivity:** If bundle A is preferable to bundle B, and bundle B is preferable to bundle C, then bundle A is preferable to bundle C.
3. **Non-satiation:** a bundle with strictly more of every good is always better.

## Utility Functions

> [!definition] Definition (Indifference curves)
> <span style="color:#0088ff">Indifference curves</span> consists of groups of bundles where the individual is indifferent between them all.

> [!claim]
> The following properties hold. As an exercise, derive them from the axioms.
> 1. Consumers prefer higher indifference curves.
> 2. Indifference curves are downward-sloping.
> 3. Indifference curves never cross, so there is one indifference curve through each bundle.

We can represent indifference curves as the level curves of utility functions:

> [!definition] Definition (Utility function)
> Suppose we have two goods $X$ and $Y$, and you buy $x$ units of one and $y$ units of the other. A <span style="color:#0088ff">utility function</span> $\mathcal{U}:\mathbb{R}\times \mathbb{R}\to \mathbb{R}$ maps every bundle to some number of <span style="color:#0088ff">utils</span>. This encodes preferences as an ordinal ranking: if some bundle has more utils, it is preferred.

> [!example]
> If I bought $S$ slices of pizza and $C$ cookies, my utility function could be $\mathcal{U}(S,C)=\sqrt{ SC }$.

In general, we will assume **diminishing marginal utility of consumption**: the amount of utility you gain from the fifth cookie you consume is lower than the amount of utility you gain from the first cookie you consume. In the above example, this is represented by the fact that the marginal utility
$$
\text{MU}_{S}=\frac{ \partial \mathcal{U} }{ \partial S }=\frac{\sqrt{ C }}{2\sqrt{ S }}
$$
is decreasing in $S$.
## Budget Constraints

As a simplifying example, we will assume that a consumer's budget $B$ is equal to their income. This is true for many Americans, who live paycheck to paycheck.

> [!definition] Definition (Budget constraint)
> If the price of good $X$ is $p_X$, and the price of good $Y$ is $p_{Y}$, then our <span style="color:#0088ff">budget constraint</span> is
> $$
> x\cdot p_{X}+y\cdot p_{Y}=B.
> $$
> Note the equality sign (instead of $\leq$), since we assume that consumers spend all their budget.

In general, budget constraints can look weirder, for example if the government gives you a certain amount of free food credits, or if you receive a coupon that discounts purchases of a good above a certain quantity. In general, all budget constraints in this class will be piecewise linear, and you can just casework on each section.
## Solving Constrained Choice

This is now a standard Lagrange multipliers problem: we want to maximize the utility $\mathcal{U}(x,y)$ subject to the budget constraint $x\cdot p_{X}+y\cdot p_{Y}=B$. This happens exactly when the slopes match.

Economists have some funny names for these slopes, so I may as well introduce them.

> [!definition] Definition (MRS and MRT)
> The <span style="color:#0088ff">marginal rate of substitution</span> is the slope of the utility curve, and is equal to $-\frac{\text{MU}_{X}}{\text{MU}_{Y}}$. The <span style="color:#0088ff">marginal rate of transformation</span> is the slope of the budget constraint, and is equal to $-\frac{p_{X}}{p_{Y}}$. 

To solve for constrained choice, you set the MRS and MRT equal.

> [!example]
> Suppose we have the utility function $\mathcal{U}(x,y)=x^{1/3}y^{2/3}$, the prices are $p_{X}=1$ and $p_{Y}=2$, and your budget is $B=1$. Solve for the optimal bundle $(x^{*},y^{*})$.

> [!proof]- Solution
> Setting the MRS and MRT equal gives $3xp_{X}=2yp_{Y}$. Plugging into the budget constraint gives 
> $$
> xp_{X}=\frac{2}{5}B\quad\text{and}\quad yp_{Y}=\frac{3}{5}B.
> $$
> Therefore, the optimal bundle is $(x^{*},y^{*})=\left( \frac{2}{5}, \frac{3}{10} \right)$.

> [!remark]
> This is an example of a common feature of utility functions of this shape: the optimal bundle will involve the consumer spending some fraction of their budget on each good, regardless of the price (it only depends on their preference).

^5f0f0d

This optimal bundle represents how much of each good the consumer is going to buy. This will allow us to derive demand curves.

---

**Next:** [[Properties of Demand Curves]]