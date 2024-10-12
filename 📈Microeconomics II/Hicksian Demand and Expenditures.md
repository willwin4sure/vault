Last, we saw that [[Marshallian Demand and Indirect Utility#^6d28b9|Marshallian demand]] is a function of price and income. What if instead we look to achieve a certain utility?

## Definitions

Expenditure is the dual to [[Marshallian Demand and Indirect Utility#^d1a8b5|indirect utility]].

> [!definition] (Expenditure)
> The ==expenditure function== is $E(\mathbf{p},u)=\min\{ \mathbf{p}\cdot \mathbf{x} | U(\mathbf{x})\geq u \}$, which can be found by solving the indirect utility function backwards for income.

> [!definition] (Hicksian demand)
> The optimal quantities for this dual optimization problem are called the ==Hicksian demands== $\mathbf{x}^{c}(\mathbf{p}, u)$. These are also called ==compensated demands==, since you are compensated for a change in price by keeping your utility constant (your expenditure changes).

## Income and Substitution Effects

Recall from [[Properties of Demand Curves#Substitution and Income Effects|14.01]] that when the price of a good changes, the effect that has on the quantity demanded (for a person with fixed income) can be decomposed into a sum of an income and substitution effect.

The substitution effect is the change in quantity demanded if we held the utility constant (i.e. just from *substituting away* to cheaper goods, while being *compensated* for the increase in price), and the income effect is the residual change that can be attributed to whether you are effectively richer or poorer.

In this class, we will define these effects via local partial derivatives, rather than in terms of actual units of quantity from discrete changes.

> [!definition] (Own-price income and substitution effects)
> The ==own-price substitution effect== is $\frac{ \partial x^{c} }{ \partial p_{x} }$, i.e. how a good's compensated demand reacts to price change. This is negative. The ==own-price income effect== is $-x^{*}\frac{ \partial x^{*} }{ \partial I }$, i.e. how a good's uncompensated demand reacts to income change, times the income change induced per price change (which is precisely how much you are holding).

There are also cross-price variants:

> [!definition] (Cross-price income and substitution effects)
> The ==cross-price substitution effect== is $\frac{ \partial x^{c} }{ \partial p_{y} }$, and the ==cross-price income effect== is $-y^{*} \frac{ \partial x^{*} }{ \partial I }$.

## Some Properties

> [!claim] (Properties of expenditures)
> 1. Homogeneity of degree $1$ in $\mathbf{p}$: $E(kp_{x},kp_{y},u)=kE(p_{x},p_{y},u)$.
> 2. ==Shepherd's lemma==: $x^{c}(\mathbf{p},u)=\partial E / \partial p_x$.
> 3. $E$ is concave in $(p_{x},p_{y})$.

The latter two have pretty much identical proofs to the latter two [[Marshallian Demand and Indirect Utility#^af6f94|properties of indirect utility]].

> [!claim] (Properties of Hicksian demand)
> 1. The own-price substitution effect is negative:
> $$
> \frac{ \partial^2 E }{ \partial p_{x}^{2} } \bigg|_{\text{const }u}=\frac{ \partial x^{c} }{ \partial p_{x} } \leq 0
> $$
> 2. ==Young's theorem==, which states symmetry of cross-price substitution effects:
> $$
> \frac{ \partial y^{c} }{ \partial p_{x} } = \frac{ \partial^{2} E }{ \partial p_{x} \partial p_{y} } = \frac{ \partial^{2} E }{ \partial p_{y} \partial p_{x} } = \frac{ \partial x^{c} }{ \partial p_{y} }.
> $$

> [!definition] (Hicksian substitutes and complements)
> Goods are Hicksian substitutes if
> $$
> \frac{ \partial x^{c} }{ \partial p_{y} } > 0,
> $$
> and Hicksian complements if
> $$
> \frac{ \partial x^{c} }{ \partial p_{y} } < 0.
> $$

---

**Next:** [[Slutsky's Equation]]