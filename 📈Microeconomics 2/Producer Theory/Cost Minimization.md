Story also told in [[Production and Costs|14.01]]. Now you have wages $w$ and a rental rate $r$, and you want to minimize $wl+rk$ subject to producing $f(k,l)\geq q$. This can be solved using [[Optimization#Lagrange's Method|Lagrange multipliers]] as well.

For now, we will assume ==competitive factor markets==, which means that firms *take prices as given*. We will relax this assumption when we talk about [[📈Microeconomics 2/Miscellany/Monopoly|monopolies]].

> [!definition]
> The ==(long run) cost function== $C(q)$ expresses the minimum cost to produce a quantity of $q$ under some (implicit) wage $w$ and rental rate $r$.

We write the average cost function as $AC(q)=\frac{C(q)}{q}$ and the marginal cost function as $MC(q)=\frac{ \partial C(q) }{ \partial q }$.

## Relationship to Returns to Scale

If we have [[Production Functions#^21aad1|constant returns to scale]], then that means $C(q)=q\cdot C(1)$, which means that $AC(q)=MC(q)=C(1)$. 

If we have [[Production Functions#^21aad1|increasing returns to scale]], then $C(sq) < sC(q)$ for $s>1$. Therefore, $qC'(q)<C(q)$ which means that $MC(q)<AC(q)$ and $AC(q)$ is decreasing: it always gets cheaper to make more.

If we have [[Production Functions#^21aad1|decreasing returns to scale]], then $C(sq)>sC(q)$ for $s>1$. This time, $MC(q)>AC(q)$ and $AC(q)$ is increasing in $q$.

> [!definition]
> If we have increasing and then decreasing returns to scale, then we have a ==minimum efficient scale== which occurs when average cost is minimized. In this case, $AC(q)=MC(q)$ as well.

## Short and Long Run

> [!definition]
> We define the ==short run== as the case where capital is fixed as $k=\overline{k}$ (but labor is allowed to vary). This is because capital inputs are usually harder to change rapidly.

> [!question]
> Show that short run costs are always at least long run costs.

## At Optimum

We can consider how $C(r,w,q)$ behaves when we vary $r$ and $w$ and keep $q$ fixed; this is analogous to the [[Hicksian Demand and Expenditures#^b2f21c|properties of expenditure functions]].

> [!claim]
> * Homogenous of degree $1$ in $(r,w)$.
> * Shepherd's lemma: $l^{*}=\frac{ \partial C }{ \partial w }$ and $k^{*}=\frac{ \partial C }{ \partial r }$.
> * Concave in $(r,w)$.

---

**Next:** [[Profit Maximization]]