> Marshallian demand functions fix an income and optimize utility.

We've already seen most of this in [[Utility and Budget|14.01]]: people have a utility function $\mathcal{U}(\mathbf{x})$ in terms of the bundle of goods, and they optimize it based on their budget. This can be solved with [[Optimization#Lagrange's Method|Lagrange's method]] from last section.

## Marshallian Demand Functions

> [!definition] (Marshallian demand)
> The ==Marshallian demand function== is given by
> $$
> \mathbf{x}^{*}(\mathbf{p},I) = \arg\max_{\mathbf{x},\ \mathbf{p}\cdot \mathbf{x} \leq I}\mathcal{U}(\mathbf{x}),
> $$
> where $\mathbf{p}$ is the price vector for goods and $I$ is your income. The budget constraint will be ==binding== (i.e. equality holds) if $\mathcal{U}$ satisfies non-satiation.

> [!example] (Cobb-Douglas utility)
> For utility functions $\mathcal{U}(x,y)=x^{\alpha}y^{1-\alpha}$, you spend $\alpha$ of your income on $x$ and $1-\alpha$ of your income on $y$, resulting in Marshallian demand functions
> $$
> x^{*}(p_{x},p_{y},I)=\frac{\alpha I}{p_{x}},\quad
> y^{*}(p_{x},p_{y},I)=\frac{(1-\alpha) I}{p_{y}}.
> $$

Here are some obvious properties.

> [!claim] (Properties of Marshallian demand)
> 1. Adding up if binding: $\mathbf{p}\cdot \mathbf{x}^{*}(\mathbf{p},I)=I$.
> 2. Homogeneity of degree zero: $\mathbf{x}^{*}(k\mathbf{p},kI)=\mathbf{x}^{*}(\mathbf{p},I)$, i.e. no illusion of money.
> 3. Doesn't depend on cardinal utility, i.e. any transformation of utility by an increasing function $\phi$ doesn't impact $\mathbf{x}^{*}$.

You should also remember the [[Properties of Demand Curves#^006e5a|price-consumption curve]] picture from 14.01.

We also still have [[Properties of Demand Curves#Price Elasticity of Demand|various]] [[Properties of Demand Curves#Income Elasticity of Demand|elasticities]] of demand:

> [!definition] (Elasticities of demand)
> ==Elasticity of demand== is the percent change in demand per percent change in some quantity $Q$, and equals
> $$
> \frac{\Delta x^{*}/x^{*}}{\Delta Q/Q} = \frac{Q}{x^{*}} \cdot \frac{ \partial x^{*} }{ \partial Q }.
> $$
> For example, $Q$ can be income $I$, own price $p_{x}$, or cross price $p_{y}$.

We also have [[Properties of Demand Curves#^4b3618|various]] [[Properties of Demand Curves#^f07f2e|names]] for categories of elasticities:

> [!definition] (Local properties of demand)
> For income elasticity: if $<1$, a ==necessity==; if $>1$, a ==luxury==. Among necessities, if $<0$ it is ==inferior==, else it is ==normal==.
> 
> For own price elasticity: if $>0$, called a ==Giffen good==.
> 
> For cross price elasticity: if $>0$, considered ==gross substitutes==; if $<0$, considered ==gross complements==.

---

**Next:** [[Indirect Utility and Expenditure]]

