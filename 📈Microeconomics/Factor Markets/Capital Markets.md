Capital is a harder concept than labor, but all types of capital are linked by one common thread: they represent the **diversion of current consumption towards future production and consumption**.

The supply of capital comes from consumers, based on their decisions of how much money to save/invest. As the rate $r$ increases, firms want less capital, as it is more costly relative to labor... yet consumers want to provide more for higher returns!

How do household savings actually become financial capital?

1. Money can go into the **bank**, which can give a loan to firm. Banks pool money, promising riskless investments to investors, and spending them on riskier ventures.
2. People can make a direct loan to the firm by buying **corporate bonds**.
3. People can invest directly in the **stocks** of the firm.
## Intertemporal Choice

Well, how do people determine how much to save? Again, it depends on their preferences, between consumption today and consumption in the future.

Suppose you are choosing between consumption in two periods, $c_{1}$ and $c_{2}$. Then, you will have some utility function $\mathcal{U}(c_{1},c_{2})$. Now, suppose you have some income $B$, you are facing an interest rate of $i$, and the prices of consumption in the two periods are $p_{1}$ and $p_{2}$, respectively.

Then, the budget constraint you are facing is just $p_{1}c_{1}+\frac{p_{2}c_{2}}{1+i}=B$, and you can solve it with Lagrange multipliers, just as normal when you choose [[Utility and Budget#Solving Constrained Choice|between two goods]].

## Inflation and Real Interest Rate

So far, we've ignored the <span style="color:#0088ff">inflation rate</span> $\pi$, which effectively decreases the value of your money (the price of goods goes up).

> [!definition] Definition (Real interest rate)
> The <span style="color:#0088ff">real interest rate</span> $r$ equals the <span style="color:#0088ff">nominal interest rate</span> $i$ less the inflation rate $\pi$.

What really matters is the real interest rate. How do we use this interest rate to make investment decisions?

> [!idea]
> When deciding the total value of a stream of money, discount future money by the real interest rate. This gives the <span style="color:#0088ff">present value</span>.

For example, the value of $10 a year for the rest of time is actually
$$
10 + \frac{10}{1+r} + \frac{10}{(1+r)^{2}} + \cdots = 10\left( 1+\frac{1}{r} \right).
$$
Note that the higher $r$ is, the less valuable money in the future is.

> [!definition] Definition (Net present value)
> For a firm, the <span style="color:#0088ff">net present value</span> of an investment decision is the present value of all revenues minus all costs.

A firm should only invest in whatever decision yields the highest NPV, as long as it is positive. Consumers can use a similar method of converting to present value to determine their own investment decisions.

---

**Next:** [[⛺Factor Markets Homepage]]
