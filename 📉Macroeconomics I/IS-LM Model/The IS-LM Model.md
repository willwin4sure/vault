This the foundation of the most important topic in this course, and combines our study of [[The Goods Market|the goods market]] and [[The Financial Market|the monetary policy]].

The goods market gave us $Y$, while the financial market gave us $i$. The IS-LM model will allow us to understand them jointly, and will formalize the impact of monetary and fiscal policy on output.

## IS Curve

> [!definition] (IS curve)
> The ==investment-savings curve== captures all combinations of $Y$ and $i$ that are consistent with equilibrium in the goods market.

Our model before was too simple, however, as too many variables were exogenous. We need to add $i$ into it: this time, we model a more realistic investment function
$$
I=I(Y,i),
$$
where $\frac{ \partial I }{ \partial Y }>0$ and $\frac{ \partial I }{ \partial i }<0$. The idea is that as output increases, more people are willing to invest, but locking up that capital is more expensive when the interest rate is higher.

> [!claim] (IS relation)
> $$
> Y=C(Y-T)+I(Y,i)+G.
> $$

You will often assume that $C$ and $I$ are both linear functions. In any case, you can solve for $Y$ given $i$ to get the IS curve.

> [!example] (Shifting IS curve)
> If taxes $T$ rise, or government spending $G$ decreases, the IS curve shifts lower:
> 
> ![[is_curve_shift.png|center|384]]

## LM Curve

> [!definition] (LM curve)
> The ==liquidity preference-money supply curve== captures all combinations of $Y$ and $i$ that are consistent with equilibrium in the financial market.

Recall equilibrium in the financial markets requires $M=\$Y\cdot L(i)$. We can divide both sides by the price level $P$ to get:

> [!claim] (LM relation)
> $$
> \frac{M}{P}=Y\cdot L(i).
> $$

Had you taken this course ten years ago, you'd say that $\frac{ \partial Y }{ \partial i }>0$ since if interest rate increases, $L(i)$ decreases, so $Y$ must increase. This is because central banks would set the money supply constant.

However, nowadays, central banks don't set monetary aggregate. Instead, they just set an interest rate directly by adjusting the money supply!

Therefore, the *modern* LM relation is actually literally a constant $i=\overline{i}$. During normal times, only the central bank can change $\overline{i}$.

## The IS-LM Model

Now let's put them together:

![[is_lm_model.png|center|384]]

The point $A$ is the only pair $(Y,i)$ that is consistent with equilibrium in both the goods and financial markets. 

> [!example] (Contractionary fiscal policy)
> For example, the government can increase taxes $T$ or decrease spending $G$. This shifts the LS curve to the left. This decreases output $Y$.
> 
> ![[is_curve_left.png|center|384]]

Another thing that could cause this shift is a decrease in autonomous spending $c_{0}$.

> [!example] (Expansionary monetary policy)
> For example, the central bank can decrease interest rate $\overline{i}$. This shifts the LM curve down. This increases output $Y$ from increased investment.
> 
> ![[lm_curve_down.png|center|384]]

In a dire economic situation, a country can go all-out and employ both expansionary fiscal and monetary policy to increase output $Y$.

If we are in a [[The Financial Market#The Liquidity Trap|liquidity trap]] at the zero lower bound of the interest rate, we lose access to monetary policy. Therefore, the only way to bail out is an expansionary fiscal policy. This can leave countries in a lot of debt.

> [!example] (Counterbalancing fiscal and monetary policies)
> We can also have an expansionary monetary policy combined with a contractionary fiscal policy. This can be done in a way to hold output constant.
> 
> ![[is_lm_counterbalance.png|center|384]]

Why would we do this? Well, the economy is not experiencing a recession or overheating, since we are not interested in changing $Y$. However, the composition changes, e.g. maybe the government has too much debt.

---

**Next:** [[An Extended IS-LM Model]]