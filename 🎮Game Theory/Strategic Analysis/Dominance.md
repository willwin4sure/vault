> If one strategy *always* produces a better result than another strategy, you should prefer it, e.g. Prisoner's Dilemma, Newcomb's paradox.

Consider some player $i \in N$ and a [[Representation of Games#Normal-Form Representation|normal form game]] $G=(N,S_{1},\dots,S_{n},u_{1},\dots,u_{n})$. We implicitly assume that players form beliefs $\beta_{-i}$ about other players' strategies and play strategies that maximize expected utility under that belief, in which case we call the player ==rational==.

> [!definition] (Pure dominance)
> For any player $i \in N$, a [[Representation of Games#^75700c|strategy]] $s_{i}^{*}\in S_{i}$ is said to ==(strictly) dominate== a strategy $s_{i} \in S_{i}$ if
> $$
> u_{i}(s_{i}^{*},s_{-i})>u_{i}(s_{i},s_{-i})
> $$
> for all $s_{-i}\in S_{-i}$.

> [!example] (Prisoner's dilemma)
> Choosing to defect strictly dominates choosing to cooperate, no matter the distribution of the other's strategy.

> [!definition] (Mixed dominance)
> A [[Representation of Games#^31daa5|mixed strategy]] $\sigma_{i}$ is said to ==(strictly) dominate== a strategy $s_{i}$ if
> $$
> u_{i}(\sigma_{i},s_{-i})>u_{i}(s_{i},s_{-i})
> $$
> for all $s_{-i}\in S_{-i}$.

> [!theorem]
> Assume there are finitely many strategies. A strategy $s_{i}$ is a [[Representation of Games#^6bee0b|best response]] to some belief if and only if $s_{i}$ is not strictly dominated by any strategy, mixed or pure.

A strategy is called ==strictly dominant== if it strictly dominates all other strategies; it is ==weakly dominant== if it weakly dominates all other strategies and it is strictly better in some cases.

> [!definition]
> A [[Representation of Games#^75700c|strategy profile]] $s^{*}=(s_{1}^{*},\dots,s_{n}^{*})$ is said to be a ==dominant-strategy equilibrium== if $s_{i}^{*}$ is weakly dominant for all $i$.

^f690d2

> [!example] (Stag-Hunt game)
> Consider the following game.
> 
> ![[stag_hunt.png|center|256]]
> 
> There is no dominant-strategy equilibrium, since Stag is the best response to Stag and Hare is the best response to Hare.

> [!example] (Second-price auction)
> Everyone submits a sealed bid simultaneously. The top bid pays the second price.
> 
> This is a beautifully designed trading mechanism where a dominant-strategy equilibrium is just to always bid your fair value, which makes it irrelevant to your strategy what other players are doing.

---

**Next:** [[Rationalizability]]
