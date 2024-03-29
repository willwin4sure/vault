The basic (and extended) [[⛺IS-LM Homepage|IS-LM model]] that we've been developing is a great simple model to think about recessions and ways to fight them.

However, it is not a great model once the economy is close to its potential output, and we need to start worrying about the supply side of the economy as well (in the basic model, output is fully demand-determined).

In the next few lectures, we will discuss the supply side of the economy and expand the IS-LM model to incorporate supply constraints and feedbacks. In particular, we will endogenize the inflation rate.

This will lead to a reason why contractionary policies exist: at some point, while getting more output, you actually get way more inflation, which is a bad trade-off.

## Population Breakdown

Here is a picture of the breakdown of the population (the U.S. in 2018) with regards to labor:

![[labor_population.png|center|384]]

[[Economic Measurement#Unemployment Rate|Recall]] that the unemployment rate is defined as the ratio of the unemployed to the civilian labor force.

While we can track these quantities over time in aggregate, they conceal the particular labor flow of the population. Here is an example:

![[labor_flow.png|center|256]]

For example, we now see there is a large flow between employment and out of the labor force, e.g. from new graduates and retirees.

## Wage Determination

Wages typically exceed their ==reservation wage==—the amount that would make them indifferent between working or being unemployed. This generally means that you *want* to be employed.

Wages typically depend on labor market conditions. In particular, the higher that $u$ is, the less bargaining power workers and unions have.

> [!claim] (Wage setting equation)
> $$
> W=P^{e}F(u,z)
> $$

Here, $\frac{ \partial F }{ \partial u }<0$ and $\frac{ \partial F }{ \partial z }>0$, and $z$ accounts for the rest of strength of workers' bargaining power. We multiply by $P^{e}$ to account for inflation: you care about how many goods you can buy, not the number of dollars you receive.

## Price Determination

The prices set by firms depend on their costs, which in turn depends on their ==production function==, which we will simplify to 
$$
Y=AN.
$$

^ab58e4

We will simplify the worker productivity to $A=1$. Then, the cost of producing one more unit of output is literally the wage $W$. 

Assume that firms set a price as a markup $m$ above the marginal cost, so $P=(1+m)W$.

> [!claim] (Price setting equation)
> $$
> \frac{W}{P}=\frac{1}{1+m}.
> $$

The higher the markup, the lower real wage firms are willing to pay. 

## The Natural Rate of Unemployment

==Natural== refers to the equilibrium level of unemployment when $P^{e}=P$. This is why we can think of it as the average unemployment rate in the medium run. Then, we can write the wage setting relation as
$$
\frac{W}{P}=F(u^{n},z),
$$
where $u^{n}$ refers to naturality. Combining this with the price setting relation gives the following diagram:

![[natural_unemployment.png|center|384]]

In equilibrium, we have that
$$
F(u^{n},z)=\frac{1}{1+m}.
$$

> [!example] (Workers strengthened)
> Suppose $z$ increases (e.g. there are higher unemployment benefits). Then, the wage-setting relation rises. This actually leads to increased unemployment!
> 
> ![[unemployment_larger_z.png|center|384]]

If the markup $m$ were to increase, then the price-setting relation falls, also leading to higher unemployment.

---

**Next:** [[The Phillips Curve and Inflation]]