[[The Goods Market|Last]], we discussed why fiscal policy is a powerful tool for governments to jumpstart economies, by injecting money into a consumer feedback loop. In the next couple of classes, we will discuss ==monetary policy==, actions that the central bank can take to achieve sustainable economic growth.

This is a great front-line system to protect the economy: it is much more nimble than fiscal policy.

## Federal Reserve System

The U.S. Central Bank consists of a board of 7 governors in DC appointed by the president and confirmed by the Senate, as well as 12 regional banks (only 4 of these bank presidents vote with the governors, one fixed as New York).

## Demand for Money

For simplicity, we will reduce investors' portfolio problem to choosing between only two assets:

* ==money==, which is used for transactions but accrues no interest.
* ==bonds==, which pay a positive interest rate $i$, but cannot be used for transactions.

There is a trade-off here! Money allows you to do more transactions, but you give up earning the interest rate on it. At an aggregate level,
$$
M^{d} = \$Y\cdot L(i),
$$
where $L(i)$ is the liquidity, a decreasing function in $i$.

In our simple model, the central bank decides to supply a certain amount of money $M^{s}$, and at equilibrium, this is set equal to $M^{d}$. Here's a picture:

![[money_supply_demand.png|center|256]]

## Open Market Operations

If the central bank changes the money supply, the interest rate moves accordingly. In modern finance, we think of the central bank as choosing an interest rate, not a money supply. 

> [!definition] (Open market operations)
> This is the way that the central bank changes the money supply. ==Expansionary open market operations== involve printing money to buy bonds. ==Contractionary open market operations== involves taking money by selling bonds.

On the balance sheet of the central bank, it has bonds in its assets and money (currency) in its liabilities. Expansionary open market operations add to both these quantities.

## Bond Prices

Suppose a bond promises to pay $100 a year from now. If the price of the bond today is $\$P_{B}$, then the interest rate is
$$
i=\frac{100-P_{B}}{P_{B}},
$$
since that is the percent return you received on the bond.

> [!idea]
> This means that when the central bank buys bonds, they are driving bond prices *up*. This means that interest rates *decrease*, as desired.

## The Liquidity Trap

This is the issue of the ==zero lower bound== on the interest rate $i$. Generically, suppose a country is in a recession and is trying to use monetary policy to relieve the economy. The idea is that you should decrease interest rate by raising money supply, which then should boost the economy. 

However, when the recession is bad enough, you've already hit $i=0$ and cannot decrease any further! You technically can go slightly negative (charge to hold peoples' money, since it's safer than being under peoples' mattresses), but not by much.

This is why there are new techniques like ==quantitative easing==, where they start buying all sorts of other financial instruments.

## Intermediaries

Our model above is massively simplified, since we have removed financial intermediaries, such as banks. 

The liabilities of a bank consist of money, in the form of ==checkable deposits==. Banks keep some ==reserves== (deposits held at the central bank) as a proportion of the funds they receive. The liabilities of the central bank are the money in circulation plus the store d portion of commercial bank reserves within the central bank, called ==high power money==. 

Here's a more realistic balance sheet:

![[balance_sheet.png|center|384]]

Then, we have that the demand for high-power money is given by $H^{d}=\theta M^{d}$, where $\theta$ is called the ==reserve ratio==. If $H^{s}$ is the supply of central bank money, then the equilibrium condition is $H^{s}=\theta\cdot\$Y\cdot L(i)$.

---

**Next:** [[The IS-LM Model]]