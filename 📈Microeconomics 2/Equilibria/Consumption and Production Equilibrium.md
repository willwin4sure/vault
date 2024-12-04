We extend on the [[Consumption Equilibrium#Setup|setup]] from before.
## Extended Setup

* Firms $1, 2, \dots, r$.
* Goods $1, 2, \dots, n_{c}$ that are "consumer goods" (outputs of firms, e.g. cars).
* Goods $n_{c}+1, \dots, n$ that are "factors" (inputs of firms, e.g. leisure/labor).
* Consumers have utility over both types of goods.
* Each firm $i'$ can produce good $k\leq n_{c}$ with production function
$$
f_{k}^{i'}:\mathbb{R}_{+}^{n-n_{c}}\to \mathbb{R}.
$$
* Firm $i'$ has production plan $\mathbf{x}^{i'}=\left\{ x_{kl}^{i'}(\mathbf{p}) \right\}_{k=1,\dots,n_{c}; l=n_{c}+1,\dots,n}$.
* Consumer $i$ owns share $\theta^{i,i'}$ of firm $i'$, and $\sum_{i=1}^{m}\theta^{i,i'}=1$ for each $i'$.
* Firm $i'$ chooses $\mathbf{x}^{i'}(\mathbf{p})$ to maximize profits (price of outputs minus price of inputs)
$$
\Pi^{i'}=\sum_{k=1}^{n_{c}}\left( p_{k}f_{k}^{i'}\left( \{ x_{kl}^{i'}(\mathbf{p}) \}_{l=n_{c}+1,n} \right) - \sum_{l=n_{c}+1}^{n}p_{l}x_{kl}^{i'} \right). 
$$
* Consumer $i$'s income is given by (endowed value plus share of firm profits)
$$
I^{i}=\sum_{j=1}^{n}p_{j}\overline{x}_{j}^{i}+\sum_{i'=1}^{r}\theta^{i,i'}\Pi^{i'},
$$
## Excess Demand and Equilibrium

Now, the excess demand for a factor $l$ is given by the sum of the consumer and producer demands less the endowed amounts.
$$
z_{l}(\mathbf{p})=\sum_{i=1}^{m}x_{l}^{i}(\mathbf{p},I^{i})+\sum_{i'=1}^{r}\sum_{k=1}^{n_{c}}x_{kl}^{i'}(\mathbf{p})-\sum_{i=1}^{m}\overline{x}_{l}^{i}
$$
Meanwhile, the excess demand for a consumer good $k$ is the sum of the consumer and producer demands less the endowed amounts.
$$
z_{k}(\mathbf{p})=\sum_{i=1}^{m}x_{k}^{i}(\mathbf{p},I^{i})-\sum_{i'=1}^{r}f_{k}^{i'}\left( \{ x_{kl}^{i'}(\mathbf{p}) \}_{l=n_{c}+1,n} \right) - \sum_{i=1}^{m}\overline{x}_{k}^{i}.
$$
We have the [[Consumption Equilibrium#^e5784e|same definition]] of equilibrium as a price vector $\mathbf{p}^{*}$ that solves $\mathbf{z}(\mathbf{p}^{*})=\mathbf{0}$, i.e. there is no excess demand.

## Efficiency Conditions

1. **Exchange:** the marginal rate of substitution between any pair of goods $k$ and $l$ is the same for all consumers. Corresponds to equal slopes of isoutilities in the [[Edgeworth Box|Edgeworth box]] picture.
2. **Production:** the marginal rate of technical substitution between any pair of inputs $l$ and $l'$ is the same for any firm producing any good. Corresponds to equal slopes of isoquants in the [[Production Edgeworth Box|production Edgeworth box]] picture.
3. **Combination:** the marginal physical product of input $l$ in the production of output $k$ equals the marginal rate of substitution between input $l$ and consumer good $k$ in consumption for any consumer $i$. Corresponds to the equal slope of isoutility and marginal product of labor in the [[Robinson Crusoe Economy|Robinson Crusoe]] picture.


---

**Return:** [[⛺Equilibria Homepage]]