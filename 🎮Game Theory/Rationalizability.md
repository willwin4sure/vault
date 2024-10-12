Games with [[Dominance#^f690d2|dominant-strategy equilibria]] are easy to solve but not very interesting to analyze, since each player can just independently pick an optimal strategy.

In the definition of a game, we assume *common knowledge* that all players are rational; the implications of this assumption constitute a concept called ==rationalizability==.

> [!example]
> Consider the following game:
> 
> ![[tmb_lr.png|center|256]]
> 
> Note that Player 1 will never play $M$, since it is strictly dominated by mixing equally between $T$ and $B$. Therefore, Player 2 should always play $R$, since it is dominant if Player 1 only picks $T$ or $B$. So Player 1 should pick $B$ always.

> [!idea] (Iterated elimination of strictly dominated strategies)
> Start with all strategies for each player, and then iteratively eliminate ones that are *strictly* dominated by a mixed strategy of all other options.

You cannot remove weakly dominated strategies.  ^df20ee

> [!definition] (Rationalizability)
> A strategy is said to be ==rationalizable== if it survives iterated elimination of strictly dominant strategies. The set of all rationalizable strategy profiles is
> $$
> S^{\infty}=\bigcap_{k=0}^{\infty}S^{k},
> $$
> where $S^{k}$ is the strategy profile after $k$ rounds of elimination. A game is said to be ==dominance-solvable== if each player has a unique rationalizable strategy.

^2d4244

> [!warning]
> One problem with rationalizability is that often too many strategies are rationalizable, leading to weak predictive power. Consider for example the [[Representation of Games#^e1f7fa|battle of the sexes]]: everything is rationalizable.

> [!example] (Beauty contest)
> Each of $n$ players submits a number $x_{i} \in [0, 100]$, and your goal is to get as close to $\frac{2}{3}$ the mean as possible (e.g. your utility is the negative square of your distance to $\frac{2}{3}$ of the mean).
> 
> You can iteratively remove the top third of the interval every time, until the only rationalizable strategy is to submit $0$.

---

**Next:** [[Nash Equilibria]]



