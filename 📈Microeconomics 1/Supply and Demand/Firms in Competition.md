In the last section, we discussed how firms determine the cost of producing a certain target $q$ in both the short and long run. In this section, we will discuss how firms choose the target $q$ in order to maximize profits.

## Perfect Competition

The way that a firm maximizes profits depends on the particular market conditions they are confronted with.

> [!definition] Definition (Perfect competition)
> We say that a market is <span style="color:#0088ff">perfectly competitive</span> if the firms in the market are all <span style="color:#0088ff">price takers</span> on both input and output sides. That is, no action that a given firm takes can affect the price $P$ of the goods that they sell or the price at which they buy their inputs ($w$ and $r$).

^dd3137

In other words, $P$, $w$, and $r$ are all handed to us by the markets, and we need to optimize accordingly. In future sections on [[⛺Monopoly and Oligopoly Homepage|different market structures]] and [[⛺Factor Markets Homepage|input markets]], we will explore what happens when you don't make such assumptions.

One example of an unchanging $P$ occurs when firm demand is *perfectly elastic*. This could happen if:

1. Consumers believe that firms sell identical products.
2. Consumers know prices charged by all firms in the market.
3. There are very low transaction costs in searching across possible purchase opportunities.

Note that firm demand elasticity is different from market demand elasticity. Firm demand elasticity measures how easily consumers can substitute between different firms selling the same product. Market demand elasticity measures how easily consumers can substitute to other products entirely.

## Profit Maximization

The goal of producers is to maximize their profits, which is their revenue less their costs:
$$
\pi(q)=R(q)-C(q).
$$
By taking a derivative, this is equivalent to setting $q$ so that $MR(q)=MC(q)$.

Well, in a competitive environment, $MR(q)=P$, the competitive market price (after all, the revenue is just $R(q)=qP$). 

> [!idea]
> In perfectly competitive markets, firm maximize profits by setting $MC(q)=P$.

In other words, only keep producing if the marginal revenue $P$ you are making per unit exceeds the marginal cost $MC(q)$ you are losing per unit.

> [!definition] Definition (Shutdown)
> A firm will undergo <span style="color:#0088ff">shutdown</span> if its maximal profit is lower than the amount they would lose from fixed costs alone. In other words, a firm shuts down if $R(q)<VC(q)\Longleftrightarrow P<AVC(q)$ at the optimal $q$.

You have already paid off the fixed costs (since in the short run, you cannot just pack up and leave). The question is just whether revenues from producing the next unit covers the variable cost of producing the next unit. If it never does, don't produce.

Note that in the long run, there are no fixed costs. Therefore, comparing to variable costs $VC(q)$ is equivalent to comparing to total costs $C(q)$. 

In particular, you may be willing to accept short run losses from fixed costs, and not shutdown. This is because you may be able to reoptimize capital in the long run. However, long run losses are never sensible, just go home.

## Firm Supply Curves

> [!example] Example (Short run firm supply curve)
> Suppose we are working with the cost function $C(q)=10+5q^{2}$. Is there a price $P$ at which the firm shuts down?

> [!proof]- Solution
> For profit maximization, we set $MC(q)=P$, so $P=10q$. However, the average variable cost is $5q$, which is always less. Therefore, the firm never shuts down.

Therefore, the firm's supply curve is literally just its marginal cost curve, under the condition that quantity drops to zero if ever marginal costs are lower than average variable costs (the firm would shut down).

What happens if there are more identical firms in the market? Well, they would also produce the same good at the same quantity, flattening the supply curve. As more and more firms join, supply of the good becomes more elastic (any change in price leads to more change in production).

![[flattening_supply_curve.jpg|center|256]]

What happens in the long run, if we allow firms to enter and exit the market? Well, a firm will exit the market whenever it cannot make a positive profit, and a firm will enter the market whenever it can make a positive profit.

> [!idea]
> In a perfectly competitive market with free entry and exit of identical firms, all firms will make zero profits in the long run!

## Complications

Obviously, firms in the real world do not make zero profits in the long run. What's going on?

Well, in reality, long run supply is likely to be somewhat upwards sloping, for three important reasons:

1. Entry might be limited (e.g. barriers to entry, such as med school).
2. Firms might differ (some may be more efficient than others).
3. Input prices might rise with output (as you try to produce more, the [[⛺Factor Markets Homepage|input markets]] will adjust; for example, you will need to pay a higher wage, which leads to higher marginal cost).

---

**Next:** [[Surpluses and Welfare Economics]]
