In 1958, A.W. Phillips found a negative relation between inflation and unemployment. Two years later, Paul Samuelson and Robert Solow labeled this relation the ==Phillips curve==. 

## Derivation of the Phillips Curve

In the model from last lecture on the [[The Labor Market|labor market]], we had the two relations
$$
\begin{align}
W&=P^{e}F(u,z), \\
P&=(1+m)W.
\end{align}
$$
We can assume a linear functional form for $F$ by
$$
F(u,z)=1-\alpha u+z.
$$
We can combine to solve
$$
P=P^{e}(1+m)(1-\alpha u+z).
$$
Therefore, controlling for expected prices, if unemployment drops, then prices rise. 

Now, divide both sides of the equation by $P_{-1}$, the price level in the previous period. Then,
$$
\frac{P}{P_{-1}}=\frac{P^{e}}{P_{-1}}(1+m)(1-\alpha u+z).
$$
Note that $\frac{P}{P_{-1}}=1+\pi$ and $\frac{P^{e}}{P_{-1}}=1+\pi^{e}$. Then,
$$
1+\pi=(1+\pi^{e})(1+m)(1-\alpha u+z).
$$
Using the Taylor approximation $\ln(1+x)\approx x$ for small values gives:

> [!claim] (Phillips Curve)
> $$
> \pi=\pi^{e}+m+z-\alpha u.
> $$

Therefore, when unemployment decreases, inflation increases.

> [!idea]
> Uh, if everyone has a job, then everyone has more money. So then price levels rise to catch up.

This model worked well up to the 60s. In the 70s, the correlation disappeared. Economists explain this by saying that there were shocks to $m$. Further, $\pi^{e}$ became de-anchored as the world changed.

## The Role of Expected Inflation

We can model
$$
\pi_{t}^{e}=(1-\theta)\overline{\pi}+\theta \pi_{t-1}.
$$
Inflation hovers around some fixed amount $\overline{\pi}$, but is also tied to the amount of inflation that occurred last year. In the 60s, $\theta \approx 0$: inflation was not inertial. However, in the 70s-90s, we got very close to $\theta=1$. This is called an ==accelerationist Phillips curve==. 

> [!idea]
> In the regime of $\theta=0$, we are told the inflation rate. In the regime of $\theta=1$, we are told the first derivative of the inflation rate. 

Nowadays, we are back to a stable amount of inflation of $\overline{\pi}\approx 2\%$ so inflation expectation has been re-anchored. We had a scare during Covid-19, but it was mostly well-managed.

## Relationship to Natural Rate of Unemployment

If we substituted $\pi=\pi^{e}$, then we could get a relationship on the natural rate of unemployment $u^{n}$, i.e.
$$
u^{n}=\frac{m+z}{\alpha}.
$$
Replacing this back into the Phillips curve gives
$$
\pi-\pi^{e}=-\alpha(u-u^{n}).
$$
Why doesn't a country's central bank just lower unemployment as much as possible (by increasing output)? Well, then there would be too much inflation!

---

**Next:** [[The IS-LM-PC Model]]