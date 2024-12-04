> [!definition] (Bayesian game)
> A ==Bayesian game== consists of:
> 
> * A set $N$ of ==players== $\{ 1,2,\dots,N \}$ with generic members $i$ and $j$.
> * A set $\Theta$ of payoff parameters (a.k.a. ==states==) with generic member $\theta$.
> * A set $T_{i}$ of ==types== for each player $i$ with generic member $t_{i}$.
> * A probability distribution $p$ on $\Theta \times T$, where $T=T_{1}\times \cdots \times T_{n}$ is the set of type profiles $t=(t_{1},\dots,t_{n})$.
> * A set $A_{i}$ of ==actions== for each player $i$ with generic member $a$.
> * A utility function $u_{i}:A\times\Theta \times T\to \mathbb{R}$ for each player $i$, where $A=A_{1}\times \cdots\times A_{n}$ is the set of action profiles $a=(a_{1},\dots,a_{n})$.
> 
> At the beginning, Nature chooses $(\theta,t)\in\Theta \times T$ via the distribution $p$. Then each player $i$ observes their type $t_{i}$ but not the state $\theta$ nor the other players' types. Finally, players simultaneously choose their actions, each player knowing their own type.
> 
> The payoffs depend on actions, the state, and the type profile. Each player maximizes the expected value of $u_{i}$.

