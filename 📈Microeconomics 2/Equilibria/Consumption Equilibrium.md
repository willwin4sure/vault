> Equilibrium among many consumers and many goods.

## Setup

Now you have many consumers $1,\dots,m$ and many goods $1,\dots,n$.

In this exchange economy, consumer $i$ is "endowed" with $\overline{x}_{j}^{i}$ units of good $j$. Write $\overline{\mathbf{x}}^{i}=(\overline{x}_{1}^{i},\dots,\overline{x}_{n}^{i})$ as the vector of endowments for this consumer.

Say that the prices for the goods are given by the vector $\mathbf{p}=(p_{1},\dots,p_{n})$. Then, consumer $i$'s income is given by $\mathbf{p}\cdot  \overline{\mathbf{x}}^{i}$, and we write their demand function as
$$
\mathbf{x}^{i}(\mathbf{p},\mathbf{p}\cdot\overline{\mathbf{x}}^{i}).
$$

## Excess Demand and Competitive Equilibrium

> [!definition] (Excess demand)
> The total ==excess demand== is given by
> $$
> \mathbf{z}(\mathbf{p})=\sum_{i=1}^{m}\mathbf{x}^{i}(\mathbf{p},\mathbf{p}\cdot  \overline{\mathbf{x}}^{i})-\sum_{i=1}^{m} \overline{\mathbf{x}}^{i}.
> $$

This is just the sum of the quantities demanded given the endowed incomes, minus the actual total quantity of that good.

> [!definition] (Competitive equilibrium)
> A ==competitive equilibrium== price vector
> $$
> \mathbf{p}^{*}=(p_{1}^{*},\dots,p_{n}^{*})
> $$
> solves the equation
> $$
> \mathbf{z}(\mathbf{p}^{*})=\mathbf{0},
> $$
> i.e. all markets are clear!

^e5784e

This is a problem with $n$ equations (no excess demand per good) and $n$ variables (the price points).

> [!claim] (Properties of excess demand)
> 1. Homogeneity of degree zero: $\mathbf{z}(c \mathbf{p})=\mathbf{z}(\mathbf{p})$ for all $c>0$.
> 2. ==Walras' law==: $\mathbf{p}\cdot \mathbf{z}(\mathbf{p})=0$.

The first is clear since only the relative prices matters. The second is clear because the total incomes are the same for demanded quantity and endowed quantity.

> [!theorem] (Existence of competitive equilibria)
> A competitive equilibrium always exists.

Consider the set
$$
X=\left\{ \mathbf{p}\in \mathbb{R}_{+}^{n} \bigg| \sum_{k=1}^{n}p_{k}=1 \right\} 
$$
of possible price vectors. Then, take the function $\mathbf{f}(\mathbf{p})$ with components
$$
f_{k}(\mathbf{p})=\frac{\max\left\{ 0,p_{k}+z_{k}(\mathbf{p}) \right\} }{\sum_{i=1}^{n}\max\left\{ 0,p_{\ell}+z_{\ell}(\mathbf{p}) \right\} }.
$$
Now, the set $X$ is closed, bounded, and convex and if the utility functions are strictly quasi-concave, then each $z_{k}$ is continuous and so $\mathbf{f}:X\to X$ is as well.

> [!idea]
> Applying $\mathbf{p}'=\mathbf{f}(\mathbf{p})$ results in a price vector where you increase the price of goods with excess demand.

In particular, any fixed point $\mathbf{p}=\mathbf{f}(\mathbf{p})$ has $\mathbf{z}(\mathbf{p})=0$, i.e. all markets clear. However, this exists by [[Brouwer's fixed point theorem]]. 

## Efficient Allocations

> [!definition] (Feasible allocations)
> An ==allocation== is a collection $\mathbf{x}=(\mathbf{x}^{1},\dots,\mathbf{x}^{m})$, where each $\mathbf{x}^{i}=(x_{1}^{i},\dots,x_{n}^{i})$. This is ==feasible== if
> $$
> \sum_{i=1}^{m}x_{k}^{i}=\sum_{i=1}^{m}\overline{x}_{k}^{i},
> $$
> i.e. the total quantity of each good remains the same.

> [!definition] (Efficient allocations)
> An allocation $\mathbf{x}$ is ==efficient== if it is feasible and there is no other feasible allocation that makes everyone better off (and at least one person strictly better off), in terms of utility.

> [!claim]
> An interior allocation is efficient if and only if marginal rates of substitution between any two goods are equalized between any two consumers.

In the Edgeworth box picture, this corresponds to the red and green isoutility curves having the same slope. If they weren't the same slope, you could trade goods while increasing the total utility.

## First Welfare Theorem

> [!theorem] (First welfare theorem)
> Competitive equilibria are efficient.

^4373e7

This is because at a competitive equilibrium, all marginal rates of substitution are just equal to the ratio of the price levels, which is constant across consumers.

Further, if you have any efficient allocation, you can make it a competitive equilibrium for a variety of initial endowments!

---

**Next:** [[Production Edgeworth Box]]