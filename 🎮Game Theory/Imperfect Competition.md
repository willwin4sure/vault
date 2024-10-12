This is the first economic section of the class, and should be familiar from [[Oligopoly and Cournot Equilibrium|14.01]]. 

As some review: monopolists artificially raise prices to capture more profit from those high up on the demand curve, but this causes needless deadweight loss from forgone trade opportunities. As more firms join in production, the social inefficiency decreases (though the firms suffer).

Recall there are two main models for firm competition: [[Oligopoly and Cournot Equilibrium#^3d012e|Cournot's model]] and [[Oligopoly and Cournot Equilibrium#^d5d3f1|Bertrand's model]]. In the former, firms compete on quantity, while in the latter, firms compete on price.

Cournot's model would suggest that regulation such as price caps can increase market efficiency, while Bertrand's model would suggest even having two firms in competition would cause prices to be set optimally. 

> [!idea] (Relevancy of models)
> Even if Bertrand's model is a more accurate description of competition, Cournot's predictions are correct when it comes to capacity creation, e.g. when farmers decide how much of a produce to bring to a market.

From 14.01:

![[Oligopoly and Cournot Equilibrium#^3b81d5]]

## Principles of Demand and Supply

The [[Supply and Demand Curves#^f766a5|demand curve]] $Q_{d}(P)$ specifies how much quantity is demanded by consumers when the good is at a fixed price $P$ (people optimize [[Utility and Budget|utility under budget]]).

The [[Supply and Demand Curves#^9dfd5e|supply curve]] $Q_{s}(P)$ specifies how much quantity is produced when the good is at a fixed price $P$ (firms optimize [[Production and Costs|production under costs]]). Recall also that the price will equal the [[Production and Costs#^ff9db8|marginal cost]] $P=C_{i}'(q_{i})$ for each firm $i$.

The economy is in a [[Supply and Demand Curves#^7a4c9d|competitive equilibrium]] when supply meets demand, i.e. where supply and demand curves intersect.

## Cournot Competition

> [!definition] (Cournot oligopoly)
> In a ==Cournot oligopoly==,  we have $n$ firms that produce $q_{i}\geq 0$ units at a fixed marginal cost $c\geq 0$ and sell it at a price
> $$
> P=\max\{ 1-Q,0 \},\quad Q=q_{1}+\cdots+q_{n}.
> $$
> Therefore, the payoff for firm $i$ is $\pi_{i}=q_{i}(P-c)$. Note that in this case, each firm *can* affect the price level $P$.

Note that we can compute the best response to opposing total production of $Q_{-i}\leq 1$ as
$$
q_{i}^{B}(Q_{-i})=\frac{1-Q_{-i}-c}{2},
$$
obtained by maximizing $\pi_{i}(p_{i})=q_{i}(1-q_{i}-Q_{-i}-c)$.

> [!example] (Cournot duopoly)
> In the two firm case, the Nash is obtained by setting both best response relationships true, yielding
> $$
> q_{1}^{*}=q_{2}^{*}=\frac{1-c}{3}.
> $$
>

You can show that this is the unique rationalizable strategy using an iterative approach.

Unfortunately, when there are three or more firms, you cannot eliminate any strategies except for the ones over the monopoly quantity $\frac{1-c}{2}$. However, the unique Nash is still where
$$
q=\frac{1-c}{N+1},\quad \pi=\left( \frac{1-c}{N+1} \right) ^{2},
$$
where $q$ is the produced quantity and $\pi$ is the profit. In particular, note that the price is marked-up from the competitive price $c$ by $\frac{1-c}{N+1}$, which hurts the consumer.

## Bertrand Competition

Now each firm picks some price $p_{i}$, and the one with the lower price $p_{i}<p_{j}$ sells $1-p_{i}$ units while the other sells none. In this case, the profit is
$$
\pi_{i}(p_{1},p_{2})=p_{i}Q_{i}(p_{1},p_{2})=\begin{cases}
(1-p_{i})p_{i} & \text{if }p_{i}<p_{j} \\
\frac{(1-p_{i})p_{i}}{2} & \text{if }p_{i}=p_{j} \\
0 & \text{otherwise}
\end{cases}.
$$
Note that if one firm just sets the price at $p_{j}=0$, then the other firms always gets $0$ profit no matter what they set $p_{i}$ as. Therefore:

> [!claim]
> Under Bertrand competition:
> 
> 1. Every strategy is rationalizable (since they are best responses to zero).
> 2. $p_{1}^{*}=p_{2}^{*}=0$ is a Nash equilibrium.

Indeed, $(0,0)$ is the only Nash, since you are always incentivized to slightly undercut.

## Discretized, Nonzero Prices

Suppose instead that the only prices you can set are
$$
P=\{ 0.01,0.02,0.03,\dots \}.
$$

> [!claim]
> This new setup is dominance solvable and the only rationalizable strategy is $(P_{\min},P_{\min})$.

In general, you can only pick this minimal price (which we assume to be positive). Sketch of the proof:

* First eliminate all prices greater than the monopoly price $0.5$ with a mixed strategy that assigns some probability $\varepsilon>0$ to the price $P_{\min}$ and the rest to $P_{\text{mon}}=0.5$. 
* If the remaining strategies are up to $\bar{p}$, you can strictly dominate it by playing $\bar{p}-0.01$ with probability $1-\varepsilon$ and $P_{\min}$ with probability $\varepsilon$.

> [!idea]
> Note that the $\varepsilon$ is necessary for *strict* dominance, e.g. in the case where the opponent undercuts you!

## Search Costs

One important assumption of Bertrand's model is that the lower price will be selected. However, in practice, it may be difficult for consumers to locate the lowest offer for a good.

