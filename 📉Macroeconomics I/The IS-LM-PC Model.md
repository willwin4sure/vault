This integrates the [[The Phillips Curve and Inflation|Phillips curve]] model into the [[The IS-LM Model|IS-LM model]].

## Rewriting the Phillips Curve

Recall that
$$
Y=C(Y-T)+I(Y,\overline{i}-\pi^{e}+x)+G.
$$As a practical assumption, we will now assume that the central bank sets the *real* interest rate $\overline{r}=\overline{i}-\pi^{e}$. This is not what actually happens, but this simplification will reduce the number of moving components.

We need to rewrite the Phillips curve so that it relates unemployment and output. If $L$ is the labor force, then using our [[The Labor Market#^ab58e4|basic production function]] where output is equal to employment, we can write $Y=L(1-u)$. Therefore,
$$
Y-Y^{n}=-L(u-u^{n}).
$$
This allows us to rewrite the Phillips curve in terms of the ==output gap== instead of unemployment:
$$
\pi-\pi^{e}=\frac{\alpha}{L}(Y-Y^{n}).
$$
When people say that the economy is *"running too hot"*, they mean that the output gap is too large, so inflation will be higher than expected. 

![[is_lm_pc.png|center|256]]

Here is a picture, where we are working in the accelerationist Phillips curve, and $\pi^{e}=\pi_{-1}$. You could easily replace it with $\overline{\pi}$, however.

This is why we don't want output to be too high. It increases inflation too much. 

## From Short to Medium Run

In the short run above, the interest rate is too low, causing inflation to be too high (or to increase). In the medium run, the central bank should begin to react by increasing the interest rate. 

> [!definition] (Wicksellian interest rate)
> The ==Wicksellian interest rate== is the real interest rate such that the natural output $Y^{n}$ is achieved. This occurs when $\pi=\pi^{e}$ exactly. This is also called the ==natural interest rate==. 

This is the target that the central bank is shooting for, and it will gradually reach it for a soft landing. If we move too quickly, we might have issues like when regional banks collapsed in 2023. Further, the parameters of the model are not actually directly observable, so we want to move slowly so we don't crash. 

> [!claim]
> In the medium run, real variables are not impacted by monetary policies.

The central bank is powerful over the short run.

## Deflationary Spiral

Suppose the IS curve shift inward to the point where you cannot achieve the necessary real interest rate $r_{n}$ in order to hit the natural output $Y^{n}$.

Then, your output is too low, and the inflation rate will begin to decrease. Eventually, this will begin to affect expected inflation rate $\pi^{e}$, which means that the real inflation rate you can achieve will start to *increase*, spiraling the problem out of control.

## Stagflation

Consider the shock to a market comprising an increase to the price of oil. We can interpret this as an increase in markup, lowering the [[The Labor Market#^059ce5|PS curve]], causing an intersection with the [[The Labor Market#^08448d|WS curve]] at a point with higher natural rate of unemployment (companies pay a lower real wage, so more people stay unemployed). 

This causes the Phillips curve to shift upwards, since the natural rate of output $Y^{n}$ must decrease. This also leads to a higher interest rate if the central bank wants to reduce inflation.

This is called ==stagflation==: it is characterized by high inflation, low economic growth, and high unemployment. This is contrary to aggregate demand shocks, where if consumers get more optimistic, there is more inflation due to more output; we have a negative relationship instead.

---

**Next:** [[Financial Markets and Expectations]]