==Purchasing power parity== is a measure of standard of living while adjusting for price of goods.

Use fixed prices for goods across countries, rather than using local prices and adjusting using currency conversion.

A common pattern in growth is that the poorest economies grow the fastest.

Recall things about [[Production and Costs|production functions]] you learned in 14.01: they are increasing functions of capital and labor, but the marginal gain in production per increase in capital/labor is decreasing.

## Interactions between Output and Capital

Suppose $N$ is constant. Recall that assuming constant returns to scale, we can rewrite our production function as
$$
\frac{Y_{t}}{N}=f\left( \frac{K_{t}}{N} \right),
$$
where $f'>0$ yet $f''<0$. 

For now, let's assume that the economy is closed and there is no public deficit, so $I_{t}=S_{t}$. Further, we write $I_{t}=sY_{t}$, where $s$ is the saving rate.

The evolution of capital stock is given by
$$
K_{t+1}=(1-\delta)K_{t}+I_{t},
$$
where $\delta$ is depreciation and $I_{t}$ is investment. In per-worker terms, we can rewrite this as
$$
\frac{K_{t+1}}{N}-\frac{K_{t}}{N}=\frac{sY_{t}}{N}-\frac{\delta K_{t}}{N},
$$
The stable equilibrium occurs when investment per worker matches depreciation per worker; then the above quantity is zero. Here is a picture:

![[output_capital_dynamics.png|center|384]]

> [!idea]
> At the start of the course, you were presented with the idea that savings are bad for the economy in the short run since they dampen the feedback loop of output. Here, we see that the dynamics in the longer run are that more savings can lead to more total production per worker.

However, the real thing the economy will optimize is steady state *consumption* per worker, since that's what makes people happy (as opposed to direct wealth).

## Integrating Population Growth and Technology

First, **population growth** by $N_{t+1}=(1+g_{N})N_{t}$. Now, we still have the evolution equation $K_{t+1}=(1-\delta)K_{t}+sY_{t}$. Dividing through by $N_{t+1}$ gives
$$
\frac{K_{t+1}}{N_{t+1}}=(1-\delta) \frac{K_{t}}{N_{t+1}}+s \frac{Y_{t}}{N_{t+1}}\approx (1-\delta-g_{N}) \frac{K_{t}}{N_{t}}+s \frac{Y_{t}}{N_{t}},
$$
where $g_{N}$ is the population growth (we drop second-order terms with both $s$ and $g_{N}$). We can rewrite this as
$$
\frac{K_{t+1}}{N_{t+1}} - \frac{K_{t}}{N_{t}}=s\cdot f\left( \frac{K_{t}}{N_{t}} \right) - (\delta+g_{N})\cdot\frac{K_{t}}{N_{t}}.
$$
When output per worker is at a steady state, then output must also be growing at rate $g_{N}$.

For **technological progress**, we capture the ==state of technology== $A$ in terms of labor equivalence, i.e.
$$
Y=F(K,AN),
$$
where $AN$ is the ==effective labor==. We just pretend we have more workers.

Because of this, we can rewrite things in terms of "per effective worker" terms, i.e.
$$
\frac{Y}{AN}=F\left( \frac{K}{AN} \right).
$$
Doing the math gives
$$
\frac{K_{t+1}}{A_{t+1}N_{t+1}}-\frac{K_{t}}{A_{t}N_{t}}\approx s\cdot \frac{Y_{t}}{A_{t}N_{t}}-(\delta+g_{AN}) \frac{K_{T}}{A_{t}N_{t}}.
$$
This is saying to achieve a steady state, you need to invest enough to account for two factors: the depreciation of existing capital stock *and* to catch up with the growth in effective labor.

![[output_capital_dynamics_growth.png|center|384]]

In this steady state, capital per worker grows at the same rate as technological progress. 

Suppose $g_{A}$ increases: then, the red line tilts upwards. This actually *lowers* both the output and capital per effective worker. Is technological progress bad? No, the only reason is that $A$ in the denominator is growing fast.
