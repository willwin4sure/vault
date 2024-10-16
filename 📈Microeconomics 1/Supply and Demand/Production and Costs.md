We turn our attention to firms, who are the producers. The goal of firms is to maximize profits.

The short story is not so different from what happened from consumers in the section on [[Utility and Budget]]. This time, producers try to hit some production target $q$ based on their <span style="color:#0088ff">production function</span>, but do so while minimizing the <span style="color:#0088ff">cost of production</span>.

This tells them how much it costs to produce any quantity $q$. Afterward, they will set $q$ to optimize profits given a market price $P$ (we'll discuss this in the next section on [[Firms in Competition]]). The story gets a bit more complicated when the quantity they decide to produce *affects* $P$, but we will address these issues when we discuss [[⛺Monopoly and Oligopoly Homepage|different market structures]].
## Firm Inputs and Outputs

Firms convert inputs (<span style="color:#0088ff">factors of production</span>) into outputs through a <span style="color:#0088ff">production process</span>. The inputs are capital and labor, while the outputs are the goods and services produced by the firm. ^d16137

> [!definition] Definition (Production function)
> A <span style="color:#0088ff">production function</span> determines the quantity a firm can produce given inputs. We will write this as $F(K,L)$, where $K$ is the capital and $L$ is the labor.

Labor is provided by consumers in a [[Labor Markets|labor market]], which yields both a quantity of labor and a price of labor, which is called the <span style="color:#0088ff">wage</span> $w$.

Capital consists of all machines, land, etc. that go into production, and comes from a [[Capital Markets|capital market]], which yields a capital <span style="color:#0088ff">rental rate</span> $r$. It is easiest to think of the machines as rented, not owned. The analysis is actually the same even if they are owned, since you'll have to borrow money to purchase the machine, then pay interest on your debt.

> [!definition] Definition (Cost function)
> The <span style="color:#0088ff">cost function</span> for a firm, then, is just $C(K,L)=r\cdot K+w\cdot L$.

> [!definition] Definition (Long and short run)
> In the <span style="color:#0088ff">long run</span>, we take all inputs as variable. In the <span style="color:#0088ff">short run</span>, we take labor as variable and capital as fixed.

>[!idea]
> Analogous to utility, we should expect there to be <span style="color:#0088ff">diminishing marginal product</span> in all inputs. For example, if you are digging a hole with a single shovel, adding more workers can only help up to a certain point.

^78e50d

## Short Run Production and Costs

In the short run (on the order of weeks, perhaps), you can easily hire/fire workers, but not buy/sell machines or land.

Therefore, we can write
$$
q=F(\underline{K},L),
$$
where $\underline{K}$ represents a fixed amount of capital. These problems are relatively simple, since you can only choose $L$ to influence $q$. 

> [!example] Example (Short run cost function)
> Suppose a firm has production function $F(K,L)=\sqrt{ KL }$ and the cost function $C(K,L)=rK+wL$ with $w=5$ and $r=10$, and it is operating in the short run with fixed capital $\underline{K}=1$. What is the cost function $C(q)$ in terms of the production target $q$?

> [!proof]- Solution
> To hit $q$, we require $L=\frac{q^{2}}{\underline{K}}$. Therefore, the cost will be $C(q)=r\underline{K}+w \frac{q^{2}}{\underline{K}}=10+5q^{2}$.

This allows us to write down several key definitions.

> [!definition] Definition (Fixed/variable costs)
> <span style="color:#0088ff">Fixed costs</span> $FC(q)$ are the costs of input that cannot be varied in the short run (capital), while <span style="color:#0088ff">variable costs</span> $VC(q)$ are the costs of input that can be (labor).

> [!definition] Definition (Total/marginal costs)
> <span style="color:#0088ff">Total costs</span> $C(q)$ are the sum of fixed and variable costs. <span style="color:#0088ff">Marginal cost</span> is the change in cost for the next unit of output, i.e. $\frac{ dC(q) }{ dq }$.

^ff9db8

We also have the terms <span style="color:#0088ff">average costs</span>, <span style="color:#0088ff">average fixed costs</span>, and <span style="color:#0088ff">average variable costs</span>, which are exactly what they sound like (divide by units produced).

Let's graph some of these quantities from the previous example.

![[short_run_cost_curves.jpg|center|384]]

When $q$ is low, average fixed costs dominate the total average costs, while when $q$ is high, average variable costs dominate.

Further, note that $MC(q)$ intersects $AC(q)$ at its minimum. You can derive this mathematically, or intuit about it. Before the intersection point, where marginal costs are lower than average costs, each extra unit produced is cheaper than the average so far, lowering the average. After the intersection point, marginal costs become higher than average costs, so each extra unit produced instead raises the average.

Finally, note that $MC=\frac{w}{MP_{L}}$, where $MP_{L}$ is the marginal product of labor. 

## Long Run Production and Costs

In the long run (on the order of years, perhaps), you can change both labor and capital.

> [!definition] Definition (Isoquants)
> An <span style="color:#0088ff">isoquant</span> is a level curve of the production function.

The shape of the isoquant is determined by the degree of <span style="color:#0088ff">substitutability between inputs</span>.

* Inputs could be perfectly substitutable, such as in the case of $F(K,L)=K+L$.
* Inputs could be not at all substitutable, such as in the case of $F(K,L)=\min(K,L)$.

> [!definition] Definition (Isocosts)
> An <span style="color:#0088ff">isocost</span> is a level curve of the cost function.

If we want to solve for the minimum cost to achieve a particular quantity, we need to find the lowest isocost curve that is tangent to the relevant isoquant. This comes down to Lagrange multipliers again, and we should just equate slopes.

Here is a picture:

![[optimal_inputs.png|center|256]]

> [!definition] Definition (Marginal rate of technical substitution)
> The <span style="color:#0088ff">marginal rate of technical substitution</span> is the slope of the isoquant, and is equal to $-\frac{MP_{L}}{MP_{K}}$.

> [!example] Example (Long run cost function)
> Suppose a firm has production function $F(K,L)=\sqrt{ KL }$ and the cost function $C(K,L)=rK+wL$ with $w=5$ and $r=10$, and it is operating in the long run. What is the cost function $C(q)$ in terms of the production target $q$?

> [!proof]- Solution
> The MRTS is $-\frac{MP_{L}}{MP_{K}}=-\frac{K}{L}$, and we need to set this equal to the slope of the isocost which is $-\frac{w}{r}$. This gives us $rK=wL$, so $K=\frac{wL}{r}$. 
> 
> Therefore, to hit the production target $q$, we require $q^2=KL=\frac{w}{r}L^2$, so $L=q\sqrt{ \frac{r}{w} }$ and $K=q\sqrt{ \frac{w}{r} }$. Together, this gives a cost function of
> $$
> C(q)=rq\sqrt{ \frac{w}{r} }+wq\sqrt{ \frac{r}{w} }=2q\sqrt{ rw }=10\sqrt{ 2 }\cdot q.
> $$
> Note that this is always lower than the corresponding short-run cost function no matter what $\underline{K}$ is, by the AM-GM inequality.

Finally, one more definition:

> [!definition] Definition (Returns to scale)
> <span style="color:#0088ff">Returns to scale</span> measure what happens to production when all inputs are scaled proportionally. In particular, the way $F(\alpha K,\alpha L)$ compares to $\alpha F(K,L)$ determines whether returns to scale are constant ($=$), increasing ($>$), or decreasing ($<$).

For a famous misapplication of the principle of diminishing product, check out the discussion of a [[Real-World Examples#^f44bb3|Malthusian catastrophe]].

---

**Next:** [[Firms in Competition]]