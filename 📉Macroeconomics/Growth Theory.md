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
The stable equilibrium occurs when investment per worker matches depreciation per worker; then the above quantity is zero.

> [!idea]
> At the start of the course, you were presented with the idea that savings are bad for the economy in the short run since they dampen the feedback loop of output. Here, we see that the dynamics in the longer run are that more savings can lead to more total production per worker.

However, the real thing the economy will optimize is *consumption* per worker.