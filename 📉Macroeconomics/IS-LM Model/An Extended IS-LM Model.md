So far, we've made a lot of simplifying assumptions throughout our construction of this model. We will hold on to many of them, but this section is dedicated to seeing what happens when we relax a couple:

1. Nominal versus real interest rates
2. Risk and risk premium

## Nominal and Real Interest Rates

The ==nominal interest rate== is the interest rate in terms of dollars, $i_{t}$. The ==real interest rate== is the interest rate in terms of goods, $r_{t}$. In particular, it is defined so that an investment of 1 good today results in $(1+r_{t})$ goods next year.

Let's derive an expression for this. Investing 1 good today is equivalent to investing $P_{t}$ dollars today, which is $(1+i_{t})P_{t}$ dollars tomorrow (using a nominal bond). The number of goods next year is
$$
1+r_{t}=\frac{(1+i_{t})P_{t}}{P_{t+1}^{e}},
$$
where $P_{t+1}^{e}$ is the expected price of goods next year. These are forced to be priced equally through ==arbitrage==. 

We denote the expected ==inflation== by
$$
\pi_{t+1}^{e}:= \frac{P_{t+1}^{e}-P_{t}}{P_{t}},
$$
which implies that
$$
1+r_{t}=\frac{1+i_{t}}{1+\pi_{t+1}^{e}}.
$$

> [!claim] (Real interest rate approximation)
> $$
> r_{t}\approx i_{t}-\pi_{t+1}^{e}
> $$

This approximation works well when interest and inflation are small. We've seen this in microeconomics when we discussed [[Capital Markets#Inflation and Real Interest Rate|capital markets]].

## Risk and Risk Premium

> [!idea]
> Lending to a business is clearly more risky than lending to the U.S. government. Therefore, the interest rate should be higher for businesses, called the ==risk premium==.

We will denote this premium by $x_{t}$, so
$$
r_{t}^{F}=r_{t}+x_{t}.
$$
Here, $r_{t}$ is the real interest rate (the safe rate), while $r_{t}^{F}$ is the real interest rate faced by firms. If we ignore risk aversion, we can compute the risk premium by using the probability of default $p_{t}$ by matching expected values. Then, we need to equate
$$
1+r_{t}=(1-p_{t})(1+r_{t}^{F}).
$$

> [!claim] (Risk premium in terms of probability of default)
> $$
> x_{t}=\frac{p_{t}}{1-p_{t}}(1+r_{t}).
> $$

During severe recessions, $p_{t}$ can rise a lot (more than $r_{t}$ can decline), so $r_{t}^{F}$ rises.

## Putting it Together

All the key changes are in the IS curve:

> [!claim] (Extended IS curve)
> $$
> Y=C(Y-T)+I(Y,i-\pi^{e}+x)
> $$

where $\pi^{e}$ is the expected inflation and $x$ is the risk premium. The LM curve remains as
$$
i=\overline{i}.
$$
