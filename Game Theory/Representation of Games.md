> Games are often represented by trees or tables.

## Payoffs

Every player also forms beliefs, likelihood assessments about the unknown. These beliefs leads to a probability distribution on the set of all outcomes $Z$; we refer to such distributions as a ==lottery==.

> [!definition] (Preferability)
> We say that a player ==strictly prefers== a lottery $p$ to a lottery $q$ if she always picks $p$ over $q$; and that she ==weakly prefers== a lottery $p$ to a lottery $q$ if she may choose $p$ when $q$ is also available.

> [!definition] (Utility functions)
> Every player is assumed to have a ==utility function== $u:Z\to \mathbb{R}$ with the condition that $U(p)\geq U(q)$ if and only if $p$ is weakly preferred to $q$, where $U(\bullet)$ is the expected utility under the specified lottery.

We are making the assumption that players are expected utility maximizers.

## Extensive-Form Representation

Contains all the information about the game explicitly using a *game tree* and *information sets*.
You've already seen this stuff from reading papers on CFR.

> [!definition] (Terminality)
> Nodes with no children are called ==terminal==, else ==non-terminal== (or, decision nodes).

> [!definition] (Extensive-form)
> An ==extensive-form== game consists of
> * A set of players $N$. Can include nature/chance, which have known probabilities.
> * A game tree with nodes $H$, also referred to as histories. 
> * An allocation of non-terminal nodes of the tree to the players. For each $h\in H\setminus Z$, $A(h)$ denotes the set of available actions to the assigned player.
> * An informational partition of the non-terminal nodes into ==information sets==, which are subsets of decision nodes that the player-to-move cannot distinguish between.
> * Payoffs $u_{i}:Z\to \mathbb{R}$ for each player $i$ at the terminal nodes.

The actual structure of the game is always assumed to be common knowledge, i.e. everyone knows it; everyone knows everyone knows it; everyone knows everyone knows everyone knots it; etc.

> [!example] (Battle of the Sexes, extensive form)
> The epitomal game of coordination with conflict:
> 
> ![[battle_sexes_ext.png|center|256]]
> 
> Note that the two players act simultaneously, since Bob is placed into an information set without knowledge of Alice's move.

There are two pure Nashes, both of which are unfair (always pick A or always pick B). There is also a mixed Nash where you pick yourself with probability $\frac{2}{3}$, but this is deficient since there is a rather large chance of miscoordination.

> [!definition] (Strategies)
> A ==pure strategy== of a player is a complete contingent-plan that determines an action at every infoset. A ==strategy profile== is a set of strategies, one per player.

## Normal-Form Representation

> [!definition] (Normal-form)
> A ==normal-form== game is any list
> $$
> G=(N, S_{1},\dots, S_{n};u_{1},\dots,u_{n}),
> $$
> where $N=\{ 1,\dots,n \}$ is the set of players, and for each player $i\in N$, $S_{i}$ is the set of all strategies available to player $i$, and
> $$
> u_{i}: S_{1}\times \cdots \times S_{n} \to \mathbb{R}
> $$
> is player $i$'s utility function. We will often write $G=(N,S,u)$, where $S=S_{1}\times \cdots \times S_{n}$ and $u=(u_{1},\dots,u_{n})$, a mapping from $S$ to $\mathbb{R}^{n}$.

In a game with two players, you can express the game using two matrices:

> [!example] (Battle of the Sexes, normal form)
> This is the common bimatrix representation you might be used to for various games:
> 
> ![[battle_sexes_norm.png|center|256]]
> 
> Here, Alice chooses between the rows and Bob chooses between the columns.

^e1f7fa

## Best Response and Mixed Strategies

Denote $s_{-i}$ as the strategy profile of all players other than player $i$. Player $i$ may also form a belief $\beta_{-i}$ (i.e. a probability distribution) about the other players' strategies $s_{-i}$. 

> [!definition] (Best response)
> For any player $i$, a strategy $s_{i}$ is said to be a ==best response== to $s_{-i}$ if
> $$
> u_{i}(s_{i}, s_{-i})\geq u_{i}(s_{i}', s_{-i})
> $$
> for all possible strategies $s_{i}' \in S_{i}$.
> 
> We also define $s_{i}$ to be a ==best response== to $\beta_{-i}$ if it is a strategy that maximizes expected payoff against $\beta_{-i}$.

Often, in order to stay adversarially optimal, you have to play a randomized strategy.

> [!definition] (Mixed strategy)
> A ==mixed strategy== is just a probability distribution over the so-called ==pure strategies== $s_{i}\in S_{i}$.

> [!claim]
> You only randomize if the expected utility of each strategy is the same.

We've seen this in [[Minimax Hypothesis Testing|inference]] and in poker: first maximize EV, and then randomize to remain unexploitable.

---

**Next:** [[Dominance]]