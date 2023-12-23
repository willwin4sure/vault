The work from [[Utility and Budget]] allows us to solve for [[Supply and Demand Curves#^f766a5|demand curves]], which are the relationship between the price and the quantity demanded by consumers.

## Drawing Demand Curves

Here's a picture of what happens when we vary the price of good $X$:

![[deriving_demand_curves.png | center | 512]]

On the left, we see how the budget constraint varies as the price of $X$ changes.

* The further out the red line, the cheaper $X$ is.
* The green lines indicate the indifference curves that are tangent to each budget constraint.
* The blue dots indicate the optimal bundles for each price of $X$. Their locus forms the <span style="color:#0088ff">price-consumption curve</span>.

In this example, we have a flat price-consumption curve, in light of a [[Utility and Budget#^5f0f0d|remark from the previous section]], where we noted that the price of goods does not affect the proportion of budget spent on each good.

On the right, we see the relationship between the price of $X$ and the quantity demanded by this consumer. Aggregating over all consumers and their various preferences, this gives the full demand curve for the market.

## Price Elasticity of Demand

Intuitively, price elasticity of demand measures how much price changes impact the quantity demanded.

> [!definition] Definition (Price elasticity of demand)
> The <span style="color:#0088ff">price elasticity of demand</span>
> $$
> \varepsilon=\frac{\Delta Q_{d}/Q_{d}}{\Delta P/P}
> $$
> is the percent change in quantity demanded over the percent change in price.

> [!example]
> **Insulin** is perfectly inelastic: there is no plausible substitute. Some individuals have no choice but to buy this good.
> 
> **Fast food options** are rather elastic: there are nearly perfect substitutes. Demand will drop to zero if the price moves at all from the initial equilibrium.

> [!note]
> Importantly, elasticity changes along the curve! This is especially true if we assume that the curve is linear.
## Shifting Demand Curves

In this section, we explore how changes in income affect quantity demanded:

![[engel_curves.png|center|512]]

This time, we parallel shift the budget constraints outwards as the income increases. We can also draw the <span style="color:#0088ff">Engel curve</span>, which shows the relationship between the budget $B$ and consumption of the good $X$.
## Income Elasticity of Demand

This one measures how much income changes impact the quantity demanded.

> [!definition] Definition (Income elasticity of demand)
> The <span style="color:#0088ff">income elasticity of demand</span>
> $$
> \gamma=\frac{\Delta Q_{d}/Q_{d}}{\Delta B/B}
> $$
> is the percent change in quantity demanded over the percent change in income.

^51dd84

> [!definition] Definition (Normal and inferior goods; luxuries and necessities)
> If the [[Properties of Demand Curves#^51dd84|income elasticity of demand]] $\gamma$ is positive, then we call the good <span style="color:#0088ff">normal</span>; otherwise, we call it <span style="color:#0088ff">inferior</span>. Further, if $\gamma<1$, then we call the good a <span style="color:#0088ff">necessity</span>; otherwise, we call it a <span style="color:#0088ff">luxury</span>.

> [!example]
> Most goods are normal. One example of an inferior good is potatoes: you substitute away from them so much as you income goes up that consumption falls.

> [!example]
> Food is an example of a necessity: this is not saying that you buy less food as your income rises, but rather that you spend a smaller fraction of your income on food. Meanwhile, cars and jewelry would be examples of luxuries, as you spend a larger share of your income on them as income rises. 

This distinctions are complicated, of course, and also local: clothing is probably a luxury over some income ranges, but a necessity over others.
## Substitution and Income Effects

In the first section, we discussed how changes in price cause changes in quantity demanded. In this section, we explore one way of subdividing this effect. 

> [!definition] Definition (Substitution effect)
> The <span style="color:#0088ff">substitution effect</span> is the change in quantity demanded of a good due to a change in that good's price, *holding utility constant*.

> [!definition] Definition (Income effect)
> The <span style="color:#0088ff">income effect</span> is the change in quantity demanded of a good due to a change in the consumer's effective income, *holding prices constant*.

The total change in quantity demanded is the sum of the substitution effect and the income effect.

Here's a picture of what's going on.

![[substitution_income_effects.png|center|384]]

Suppose the price of good $X$ increases. This shifts the budget constraint from the outer red line to the inner red line, moving the optimal bundle. We can decompose the change in quantity demanded of $X$ into two shifts.

The substitution effect is computed by considering an imaginary budget constraint, **parallel to the new budget constraint**, that is tangent to the original indifference curve. This effect is *always* opposite the price change; since the price of $X$ increased, we substitute *away* from $X$.

The income effect is computed by considering the change from this imaginary budget constraint to the true new budget constraint, which just corresponds to a decrease in income. This effect is in the same direction as the substitution effect for normal goods, and the opposite for inferior goods.

| Type of Good | Price Change | Substitution Effect | Income Effect | Total Effect |
| ------------ | ------------ | ------------------- | ------------- | ------------ |
| Normal       | <font style="color:lime">Price Rises</font>  | <font style="color:red">Negative</font>            | <font style="color:red">Negative</font>      | <font style="color:red">Negative</font>    |
| Normal       | <font style="color:red">Price Falls</font>  | <font style="color:lime">Positive</font>            | <font style="color:lime">Positive</font>      | <font style="color:lime">Positive</font>    |
| Inferior     | <font style="color:lime">Price Rises</font>  | <font style="color:red">Negative</font>            | <font style="color:lime">Positive</font>      | <font style="color:yellow">Unknown</font>     |
| Inferior     | <font style="color:red">Price Falls</font>  | <font style="color:lime">Positive</font>            | <font style="color:red">Negative</font>      | <font style="color:yellow">Unknown</font>     |

In the case of an inferior good where the income effect overcomes the substitution effect, we get a <span style="color:#0088ff">Giffen good</span>, which has an upwards-sloping demand curve! There is some convincing evidence that these may actually exist in specific circumstances.

---

**Next:** [[Production and Costs]]
