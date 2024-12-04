[[Rationalizability#^2d4244|Rationalizability]] often has weak predictive power because it requires you to cope with the entire range of strategic possibilities of all your opponents, and your own behavior may be easily swayed as your beliefs about other peoples' strategies changes.

In real life, however, games are played in context, and you can often assume conventions about people's behavior. Nash equilibria are plausible solution concepts in stable environments where you can reasonably guess what your opponents will do, and they correspond to the steady-states of adjustment processes where players adjust their behavior according to others' behavior.

> [!definition] (Nash equilibrium)
> A [[Representation of Games#^75700c|strategy profile]] $s^{*}=(s_{1}^{*},\dots,s_{n}^{*})$ is said to be a ==Nash equilibrium== if $s_{i}^{*}$ is a [[Representation of Games#^6bee0b|best response]] to $s_{-i}^{*}$ for each $i$. That is, no player has any (strict) incentive to deviate, holding all other strategies fixed.

^f43b46

> [!idea]
> One quick way to identify pure Nash equilibria is to just intersect best responses for both players.

## Relationship to Dominance and Rationalizability

It turns out that Nash equilibria are weaker than [[Dominance#^f690d2|dominant-strategy equilibria]] and stronger than [[Rationalizability#^2d4244|rationalizability]]. 

> [!idea]
> This is, by the way, one justification for why [[Rationalizability#^df20ee|we can't remove weakly dominated strategies]] in determining rationalizable strategies, since that would cut out Nash equilibria.

> [!theorem]
> If $s^{*}$ is a dominant-strategy equilibrium, then $s^{*}$ is a Nash equilibrium.

> [!proof]-
> Well, we know that $u_{i}(s_{i}^{*},s_{-i})\geq u(s_{i},s_{-i})$ for all $s_{i}$ and $s_{-i}$. Picking $s_{-i}=s_{-i}^{*}$ finishes.

> [!example] (Nash equilibria can involved weakly dominated strategies)
> Consider the game where each player selects a number $s_{i}\in\{ 0,1,2,\dots \}$ and the payoff is $s_{i}s_{j}$. Then $(0,0)$ is a Nash but it is weakly dominated.

> [!theorem]
> If $s^{*}$ is a Nash equilibrium, then $s_{i}^{*}$ is rationalizable for every player $i$.

> [!proof]-
> We show that none of the strategies $s_{i}^{*}$ can be eliminated at any finite round $k$. Induct on $k$; given that the other strategies in $s^{*}$ exist, $s_{i}^{*}$ is a best-response and therefore cannot be eliminated.

## Mixed Nash Equilibria

> [!definition] (Mixed Nash equilibrium)
> A mixed-strategy profile $\sigma^{*}=(\sigma_{1}^{*},\dots,\sigma_{n}^{*})$ is said to be a ==Nash equilibrium== if for every player $i$, $\sigma_{i}^{*}$ is a best response to $\sigma_{-i}^{*}$, i.e. $u_{i}(\sigma_{i}^{*},\sigma_{-i}^{*})\geq u_{i}(\sigma_{i},\sigma_{-i}^{*})$ for every mixed strategy $\sigma_{i}$.

> [!example] (Matching-penny game)
> In the matching-penny game, both players pick heads or tails. Player one wins if they match and Player two wins if they don't. Here is the payoff matrix:
> 
> ![[matching_penny.png|center|256]]
> 
> While there is no pure Nash, we can solve for a mixed Nash. Give each player $p_{i}$ chance of picking heads; then, the expected payoff for player one is
> $$
> 	u_{1}(p_{1},p_{2})=p_{1}p_{2}+(1-p_{1})(1-p_{2})-p_{1}(1-p_{2})-p_{2}(1-p_{1})=(2p_{1}-1)(2p_{2}-1).
> $$
> Note that this game is [[Zero-Sum Games#^ac7f67|zero-sum]] so it suffices to minimize/maximize this quantity. The best responses have a unique intersection at $\left( \frac{1}{2}, \frac{1}{2} \right)$ (if either player has more or less than $\frac{1}{2}$, the best response is to pick $0$ or $1$).

Note that in a mixed Nash, you must be indifferent between all the pure strategies with positive probability, and these must have maximal expected payoff, fixing every other player's strategy as constant.

> [!idea]
> There is no general way to find all mixed Nashes, but one powerful method is to first eliminate all non-rationalizable strategies. 

## Application: The Investment Game

> [!example] (Investment game)
> Here is the relevant payoff matrix:
> 
> ![[investment_game.png|center|256]]
> 
> where $\theta$ and $c>0$ are known parameters, e.g. whether to collaborate on a project.

If $\theta<0$, then stay out strictly dominates. If $\theta>c$, then invest strictly dominates.

So the only interesting case is where $0<\theta<c$. Coordinating to both invest or both stay out are both pure Nashes.

> [!idea]
> To compute mixed Nash equillibria, make the other player indifferent.

Say you pick invest with probability $q$. For the other player to be indifferent to the two options, we need that
$$
\theta q+(\theta-c)(1-q)=0 \implies q=1-\frac{\theta}{c}.
$$
This is the unique mixed Nash equilibrium.

The existence of the "bad" equilibrium where both players stay out (and get a lower payout than the "good" equilibrium of both investing) is intriguing.

It corresponds to so-called ==poverty traps==, where workers don't invest in their human capital anticipating lack of demand, and firms do not invest in skill-intensive technologies anticipating lack of skilled labor.

## Application: Hawks, Doves, and Evolution

> [!example] (Hawks and doves)
> Here is the relevant payoff matrix:
> 
> ![[hawk_dove.png|center|256]]
> 
> where $V,c>0$ are known parameters.

When $V>c$, this is a Prisoner's dilemma. When $V<c$, this is a game of chicken. The former case has a strictly dominant strategy, while the latter case has three Nashes: two asymmetric ones at $(\text{Hawk},\text{Dove})$ and $(\text{Dove},\text{Hawk})$, and a symmetric mixed Nash.

To compute the mixed Nash, we assert indifference. If there is a probability $q$ of picking Hawk, then the other player is indifferent if
$$
q\left( \frac{V-c}{2} \right)+(1-q)V = (1-q) \frac{V}{2}, 
$$
or in other words if $q(V-c)+(1-q)V=0$. Hence
$$
q=\frac{V}{c}.
$$

We can now imagine an evolution model based on this game. Initially there are $H_{0}$ hawks and $D_{0}$ doves. Every round, they are randomly paired off, and the reproduce based on the payoff matrix above.

> [!claim]
> The only steady state in this model is when the proportion of hawks if $\frac{V}{c}$ (and also the ones with all hawks or all doves).

---

**Next:** [[🎮Game Theory/Strategic Analysis/Imperfect Competition|Imperfect Competition]]