When we talked about firms, we assumed they were in a market with [[Firms in Competition#^dd3137|perfect competition]], meaning that they were price takers. In a monopoly, there is only a single firm, making them <span style="color:#0088ff">price makers</span>.

In general, this leads to artificial limiting of quantity to sell products at a higher price, leading to larger profits overall.
## Profit Maximization

Instead of receiving a market price $P$, the price is dependent on the quantity $Q$ produced, by the inverse demand curve $P(Q)$. Then, their goal is to maximize profits
$$
\pi(Q)=R(Q)-C(Q)=P(Q)\cdot Q-C(Q),
$$
where we derive the cost function as before.

This time, the marginal revenue is
$$
MR=\frac{dR}{dQ}=P+\frac{dP}{dQ}\cdot Q
$$
by the product rule. The second term is negative, and it is known as the <span style="color:#0088ff">poisoning effect</span>: in order to produce more units, the firm is forced to sell at a lower price along the demand curve, leading to less profits from previous customers.

Since we want to find $Q$ so that $MR(Q)=MC(Q)$, and the marginal revenue is lower than raw demand, this means that the quantity the monopoly produces is lower than in the perfectly competitive case (where $P=MC(Q)$).

> [!example] Example (Monopoly price maximization)
> Suppose a monopolistic firm faces a market demand of $Q_{d}=24-P$ and has a cost function of $C(Q)=12+Q^{2}$. How many units will the firm choose to sell? How does this compare to the perfectly competitive case?

> [!proof]- Solution
> The inverse demand curve is $P(Q)=24-Q$, and the monopolist needs to set
> $$
> MR(Q)=MC(Q)\implies 24-2Q=2Q.
> $$
> Therefore, the monopolist produces $Q=6$ units at a price of $P=18$. Meanwhile, in the competitive case, we would have set $24-Q=2Q$ yielding $Q=8$ and $P=16$.

Here is a picture:

![[monopoly_equilibrium.jpg|center|256]]

Since the monopolist equates $MC$ to $MR$ (which is lower than demand), the quantity is restricted and price increased.

> [!definition] Definition (Markup)
> We define the <span style="color:#0088ff">markup</span> as the percentage under the monopolistic price the actual marginal cost is, i.e. $\theta=\frac{P-MC}{P}$.

It turns out that the markup is related to the elasticity of demand:

> [!question]
> Show that $\theta=-\frac{1}{\varepsilon}$, where $\varepsilon$ is the price elasticity of demand.

> [!idea]
> Intuitively, this makes sense: the less elastic the market is (in terms of absolute value), the more the monopolist should be able mark up the price.

For example, if you are the only firm producing insulin, consumers will have to purchase from you no matter how high you set the price. However, in most markets, monopolists will not be able to charge infinite price, as there may be close substitutes for the product.

## Welfare Effects of Monopoly

It is clear from the image above that the monopolist introduces [[Surpluses and Welfare Economics#^43802d|deadweight loss]] (marked in black) versus the perfectly competitive case.

However, a monopolist that can perform perfect <span style="color:#0088ff">price discrimination</span> can actually recapture the entire social welfare, it just all comes in the form of producer surplus! All such a monopolist would have to do is sell to each person at exactly the price they are willing to pay, unlike in our current model where the monopolist must charge the same price for every customer.

In this case, the monopolist will keep selling until $P=MC(Q)$, once again the perfectly competitive equilibrium, maximizing social welfare.

> [!idea]
> An alternative solution is that the government solves this with policy! They can impose a price ceiling at the perfectly competitive price.

If they set the price ceiling too low, however, this may introduce even more deadweight loss than leaving the monopoly alone.
## How do Monopolies Arise?

An essential input (e.g. they own the only rock quarry) or huge fixed costs causing large barriers to entry (e.g. needing to lay down a large network of water pipes) are both examples of reasons.

Another reason is governmental action. A main one is providing <span style="color:#0088ff">patents</span>: exclusive rights to sell a product for a fixed period of time. Why would the government intentionally create deadweight loss from introducing a monopoly?

> [!idea]
> In perfect competition, firms make zero profits in the long run. **Patents promote innovation**; without them, people would not be incentivized to invent stuff, when other firms can just copy their products.

Even though patents appear to lower welfare by creating monopolies, they could actually raise welfare by increasing the value to consumers of the goods that are produced.

---

**Next:** [[Oligopoly and Cournot Equilibrium]]