[[Representation of Games#^d35807|Recall]] that a two-player strategic-form game can be specified by utility functions
$$
u_{i}: S_{1} \times S_{2} \to \mathbb{R}
$$
for players $i=1,2$, where $S_{i}$ is the set of [[Representation of Games#^75700c|pure strategies]] for player $i$. We denote by $\Sigma_{i}$ the set of [[Representation of Games#^31daa5|mixed strategies]] for player $i$.

> [!definition]
> A two-player game is called ==zero-sum== if $u_{1}(s)+u_{2}(s)=0$ for any [[Representation of Games#^75700c|strategy profile]] $s$.

This allows us to simply keep track of the first player's utility $u$. Player 1 tries to maximize this value while Player 2 minimizes it.

## Security Strategies

Suppose Player 1 tries to maximize their utility under the worst-case response from Player 2.

> [!definition]
> A strategy $\sigma_{1}^{*}$ is a ==security strategy== for player 1 if it maximizes the worse-case gain function
> $$
> \text{WG}(\sigma_{1})=\min_{\sigma_{2} \in\Sigma_{2}}u(\sigma_{1},\sigma_{2})=\min_{s_{2} \in S_{2}}u(\sigma_{1},s_{2}).
> $$
> Note that player 2 doesn't have to mix since they can just pick highest EV knowing player 1's move.

We call the utility achieved by the security strategy $\underline{v}$. In particular, player 1 ==secures== a utility of $\underline{v}$: they get a lower bound of this utility, no matter how the opponent actually plays.

Symmetrically, player 2 can ensure they lose at most $\overline{v}$, no matter how player 1 plays.

## Minimax Theory

We've already seen this before in [[Minimax Hypothesis Testing#^48bd56|inference]] when we modeled nature as adversarial and tried to remain minimax optimal.

> [!theorem] (Minimax, von Neumann 1928)
> In a finite two-player zero-sum strategic form game, $\underline{v}=\overline{v}$; that is,
> $$
> \max_{\sigma_{1}\in\Sigma_{1}}\min_{\sigma_{2}\in\Sigma_{2}}u(\sigma_{1},\sigma_{2})=\min_{\sigma_{2}\in\Sigma_{2}}\max_{\sigma_{1}\in\Sigma_{1}}u(\sigma_{1},\sigma_{2}).
> $$

> [!definition]
> The common value is called the ==value of the game== $v=\underline{v}=\overline{v}$. This is equal to the utility given by any pair of securities strategies from both players.

## Characterizing Nash Equilibria

> [!theorem]
> Consider a finite two-player zero-sum strategic form game. For any strategy profile $(\sigma_{1},\sigma_{2})$, the following are equivalent:
> 
> 1. $(\sigma_{1},\sigma_{2})$ is a Nash equilibrium.
> 2. $\sigma_{1}$ is a security strategy for player 1, and $\sigma_{2}$ is a security strategy for player 2.

> [!proof]-
> For the backward direction, note that if both are security strategies then there is no strictly profitable deviation for either player.
> 
> For the forward direction, we prove the contrapositive. If at least one player doesn't have a security strategy, they can deviate to achieve the value of the game. 

This results in some important properties:

> [!claim]
> 1. **Unique equilibrium payout.** All Nash equilibria give player 1 utility $u$.
> 2. **Equilibrium exchange property.** If $(\sigma_{1},\sigma_{2})$ and $(\sigma_{1}',\sigma_{2}')$ are both Nashes, then so are $(\sigma_{1},\sigma_{2}')$ and $(\sigma_{1}',\sigma_{2})$.
> 3. **Computational tractability.** To compute the equilibria, we can compute the security strategies for each player, which is a tractable linear programming problem.
> 

---

**Next:** [[Supermodular Games]]