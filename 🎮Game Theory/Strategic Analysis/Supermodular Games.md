We illustrate this concept with an example.

## Motivating Example

A firm can invest in two complementary expenditures: advertisement and R&D. Suppose that an investment of $a$ in advertisement and $b$ in R&D results in expected profits of
$$
\mathcal{U}(a,b)=\alpha(a)\beta(b)p-a-b
$$
for some fixed price $p$, where $\alpha$ and $\beta$ are both increasing functions. These are indeed ==complementary== since $\frac{ \partial^{2}\mathcal{U} }{ \partial a \partial b }>0$. They are also both complementary with $p$.

Since there is complementarity between a parameter ($p$) and a choice variable ($a$, $b$), the optimal solution is increasing in the parameter, i.e. expenditure should be increasing in price.

If instead $a$ and $b$ are chosen independently by two different players, the game may still exhibit ==strategy complementarity==, e.g. if
$$
u_{A}(a,b)=\frac{1}{2}\alpha(a)\beta(b)p-a\quad\text{and}\quad u_{B}(a,b)=\frac{1}{2}\alpha(a)\beta(b)p-b.
$$
This condition implies that the best response functions will be increasing. Here is an example of what they may look like, if $\alpha$ and $\beta$ are both $x\mapsto x^{1/3}$.

![[advertisement_rnd.png|center|256]]

Notice there are two equilibria, one at $(0,0)$ and one above, at $((p/6)^{3},(p/6)^{3})$. They are *ordered* are both are *weakly increasing* in the price $p$; the dashed lines show what happens when $p$ increases (note that in games where there is a middling equilibrium, it may decrease in $p$).

It turns out the set of rationalizable strategies is simply those within the range $[0,(p/6)^{3}]$. 

## Supermodularity

> [!definition]
> A function $f:\mathbb{R}^{2}\to \mathbb{R}$ is said to have ==increasing differences== if for any $(x_{1},x_{2})>(y_{1},y_{2})$, we have that
> $$
> f(x_{1},x_{2})-f(y_{1},x_{2})\geq f(x_{1},y_{2})-f(y_{1},y_{2}).
> $$
> We call the two inputs ==complements== (under $f$). When $f$ is twice-differentiable, this is equivalent to
> $$
> \frac{ \partial^{2}f }{ \partial x_{1}\partial x_{2} }>0. 
> $$

> [!definition]
> A function $f:\mathbb{R}^{m}\to \mathbb{R}$ is said to be ==supermodular== if any two inputs are complements under $f$. We call $f$ ==submodular== if $-f$ is supermodular.

## Optimization under Supermodular Payoffs

When the payoff function is supermodular, the set of optimal choices will shift up when the parameters move up.

> [!theorem] (Monotonicity theorem)
> Consider any continuous supermodular utility function $u:X \times\Theta \to \mathbb{R}$ where $X=X_{1}\times X_{2}\times \cdots \times X_{m} \subset \mathbb{R}^{m}$ is a nonempty, closed, and bounded set of choice variables $x$ and $\Theta \subseteq \mathbb{R}^{n}$ is a nonempty set of payoff parameters $\theta$. Then, among the set of optimal choices
> $$
> \{ x \in X | u(x,\theta)\geq u(y,\theta) \text{ for all }y \in X \},
> $$
> their maxima $\overline{B}(\theta)$ and minima $\underline{B}(\theta)$ are both weakly increasing in $\theta$.
> 

> [!example]
> Consider a monopolist who picks a price
> $$
> p^{*}(\theta,c)=\arg\max_{p\geq c'}(p-c)D(p,\theta).
> $$
> We can apply supermodularity after transforming the optimization objective into
> $$
> p^{*}(\theta,c)=\arg\max_{p\geq c'}\log(p-c)+\log D(p,\theta).
> $$
> This is supermodular in $p$ and $c$, so $p^{*}$ must be weakly increasing in $c$. It might also be supermodular in $p$ and $\theta$, depending on the shape of $D$.

## Supermodular Games

Suppose you have a game among players $N=\{ 1,2,\dots,n \}$ such that the set of strategies for player $i$ is a closed and bounded subset
$$
S_{i}=S_{i_{1}}\times \cdots S_{i_{m_{i}}}\subset \mathbb{R}^{m_{i}},
$$
with smallest and largest elements $\underline{s_{i}}$ and $\overline{s_{i}}$, respectively. Typically, $S_{i}=[\underline{s_{i}},\overline{s_{i}}]$. If each utility function $u_{i}:S\to \mathbb{R}$ is supermodular and continuous, we call this game itself ==supermodular==.

> [!example] (Differentiated Bertrand oligopoly)
> Suppose each player faces a marginal cost $c_{i}$ and a demand function
> $$
> Q_{i}(p)=A-a_{i}p_{i}+\sum_{j\neq i}^{}b_{j}p_{j},
> $$
> where all the parameters are positive. For each $i$, we assume the price is selected from $[c_{i},\overline{p_{i}}]$ for some large $\overline{p_{i}}$. This is a supermodular game since
> $$
> u_{i}(p)=(p_{i}-c_{i})Q_{i}(p)
> $$
> is supermodular, as
> $$
> \frac{ \partial^{2}u_{i} }{ \partial p_{i}\partial p_{j} }=b_{j}>0.
> $$

> [!example] (Linear Cournot duopoly)
> Consider the standard model with inverse demand function $P=A-q_{1}-q_{2}$ and cost functions $C_{1}(q_{1})$ and $C_{2}(q_{2})$. Restrict the set of possible productions to some large interval. Then, this is a "submodular" game since the utilities are
> $$
> u_{i}(q)=q_{i}P(q)-C_{i}(q_{i})
> $$
> with
> $$
> \frac{ \partial^{2}u_{i} }{ \partial q_{i}\partial q_{j} } =-1<0.
> $$
> We can make the supermodular by just flipping the direction of one of the $q_{i}$. This is a common trick that only works for two-player games.

## Key Results

> [!theorem]
> For any supermodular game, the strategy profiles
> $$
> s^{*}=\lim_{ k \to \infty } \overline{B}^{k}(\overline{s})\quad\text{and}\quad s_{*}=\lim_{ k \to \infty } \underline{B}^{k}(\underline{s})
> $$
> are Nash equilibria, and for any rationalizable strategy profile $s$,
> $$
> s^{*}\geq s\geq s_{*}.
> $$

 These arise, e.g. by starting at the largest strategy $\overline{s}$ and repeatedly 


---

**Next:** [[Backward Induction]]