There are cases during dynamic games where both players know they are in the same subtree; in these cases, we should expect them both to play according to a Nash equilibrium.

This is similar logic to [[Backward Induction|backward induction]]: recall the motivating example where you can construct a Nash equilibrium that is irrational on certain subgames since it is presumed that you won't reach them (potentially *because* you act irrationally there!).

This section introduces the notion that disallows such solution concepts.

## Definition

> [!definition] (Subgames)
> A ==subgame== is defined as part of a game that is a well-defined game when considered separately:
> 
> * it must contain a unique "initial" node and
> * all the moves and [[Representation of Games#^85a4dc|information sets]] from that node on must remain in the subgame.

^576823

In particular, the subtree cannot cut any information sets. So this solution concept isn't very useful in games with permanent incomplete information, e.g. poker.

> [!definition] (Subgame-perfect Nash equilibria)
> A [[Nash Equilibria#^f43b46|Nash equilibrium]] is said to be ==subgame-perfect== if it is a Nash equilibrium in every [[Subgame Perfect Nash Equilibria#^576823|subgame]].

^4f841d

> [!claim] (Method to compute SPNEs in finite-horizon games)
> * Pick a subgame that doesn't contain any other subgames.
> * Compute a Nash equilibrium of the game.
> * Assign the payoff vector of this Nash to the starting node and eliminate the subgame.
> * Iterate the procedure until a move is assigned at every contingency, when there remains no subgame to eliminate.

> [!example] (Multi-stage games)
> These are games where you repeatedly play some game, but you are exposed to the actions of all previous stages after they happen.

## Money Burning and Forward Induction

In dynamic games, an action can be part of a rational plan when it is combined by a continuation play, but it is not rational when it is combined with another continuation play.

> [!example] (Battle of the Sexes with Exit)
> Suppose Alice observes that Bob chooses to Play the game rather than Exit. Then it would be irrational for Bob to then pick A, since then it would've been strictly better to just Exit. Forward induction would then suppose that Bob will pick B, and Alice should follow suit.
> 
> ![[battle_sexes_with_exit.png|center|384]]

Should Alice attribute "Play" from Bob to be a mistake, or part of a rational plan where he will definitely pick B afterwards? 

> [!example] (Money burning)
> This is a weird implication of the forward induction logic.
> 
> ![[money_burning.png|center|384]]
> 
> Note that burning a dollar and then picking B is not a best response to any belief by A. So should B believe that if he sees Alice play "Burn", she intends to play A afterwards, so he should follow suit?
> 
> It seems unpleasing that burning a dollar here can result in Alice "getting her way" in the subsequent Battle of the Sexes game.

> [!warning] (Key weaknesses in forward induction)
> 1. The above example is not intuitively pleasing.
> 2. The reasoning relies heavily on the common knowledge assumptions of the game, e.g. what if Bob's exit reward is actually unknown?

## One-shot Deviation Principle

This allows checking that a strategy profile is a [[Subgame Perfect Nash Equilibria#^4f841d|SPNE]] in infinite-horizon games, specifically multi-stage ones that are "continuous at infinity" (i.e. if two strategies are different but agree for very long successive stages, they give very similar payoffs).

> [!claim] (One-shot deviation test)
> Consider any strategy profile $s^{*}$. Pick any stage, and suppose we are there. Pick a player $i$ who moves at that stage. Now fix the strategies of all other players for this stage and all future stages. Also fix the strategies of player $i$ for all future strategies.
> 
> Now check if there is a deviation by player $i$ this stage (a *one-shot* one) that can improve the payoff of $s^{*}$, fixing everything else.

> [!theorem] (One-shot deviation principle)
> In a multi-stage game that is continuous at infinity, a strategy profile is a [[Subgame Perfect Nash Equilibria#^4f841d|subgame-perfect Nash equilibrium]] if and only if it passes the one-shot deviation test at every stage for every player.

^acbce6

---

**Next:** [[Infinite-Horizon Bargaining]]

