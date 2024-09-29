How would you measure the aggregate output of an economy?

Measures to answer this question have only been developed quite recently.

## Gross Domestic Product (GDP)

> [!definition] (GDP)
> ==Gross domestic product== is a monetary measure of the total market value of all the final products produced by an economy in a specific time period.

> [!idea]
> The GDP does not include intermediate goods!

This ensures that GDP is not a function of the particular way the economy is structured. For example, if we have a company that sells steel to a car company, we don't include the value of that steel in GDP (otherwise if the two companies merged, GDP would change).

We can compute this in a scalable way by summing the ==values added== by each company, or by summing the ==incomes== in the economy (wages + profits).

There are two slight variants of this concept, however:

> [!definition] (Nominal and Real GDP)
> ==Nominal GDP== is the sum of quantities of final goods produced, weighted by their *current price*. ==Real GDP== is instead weighted by constant (not current) prices. We denote these respectively by $\$Y_{t}$ and $Y_{t}$.

We want to use real GDP because an increase in nominal GDP does not necessarily signify a strengthened economy; for example, there could just be a lot of inflation.

However, how do we determine the prices to use for real GDP? One way is to directly pick a base year and measure everything in terms of that (e.g. real GDP in 2012 dollars). A more sophisticated method is called ==chain weighting==.

In this class, we will analyze what causes large changes in real GDP across years.

## Unemployment Rate

> [!definition] (Unemployment)
> ==Employment== is the number $N$ of people who have a job. ==Unemployment== is the number $U$ of people who do not have a job but *are looking for one*. The ==labor force== $L=N+U$ is the sum of the employed and unemployed. The ==unemployment rate== is the ratio $u=\frac{U}{L}$.

The labor force is quite different from the population: some people are young/students, others are disabled or retired.

Sometimes, the unemployment rate is underrepresents how badly the economy is doing in deep recessions, due to ==discouraged workers==, who would like a job but have given up looking for one, and therefore are no longer counted in $U$. One way of accounting for this is the ==participation rate==, which is the ratio of the labor force to the total working age population.

## Inflation Rate

> [!definition] (Inflation)
> ==Inflation== is a sustained rise in the general ==price level== $P_{t}$. The ==inflation rate== is the rate at which the price level increases:
> $$
> \pi_{t}=\frac{P_{t}-P_{t-1}}{P_{t-1}}.
> $$
> ==Deflation== is a sustained decline in the price level, where $\pi_{t}<0$.

The ==GDP deflator== is one measure of $P$, i.e. $P_{t}=\frac{\$Y_{t}}{Y_{t}}$.

Another measure is the ==Consumer Price Index (CPI)==, which is a measure of *cost of living*. This is published monthly by the Bureau of Labor Statistics (BLS), which collects price data for 211 items in 38 cities.

Headline CPI is rather noisy, but we can remove the most volatile items to get the core CPI, which is much more stable.

---

**Next:** [[The Goods Market]]