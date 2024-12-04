Consider the following game.

> [!example]
> Alice offers Bob some number of candies $k\in \{ 0,1,2,3,4,5 \}$. If Bob accepts the offer, then Alice gets $5-k$ candies and Bob gets $k$ candies. Otherwise, they both get nothing.

^c41380

One very natural Nash is that Alice offers 1 candy and Bob accepts. Another less natural Nash is that Bob only accepts if Alice gives 5 candies, and Alice does so (Alice is indifferent to everything here).

However, even if Bob commits to this Nash strategy, why should he reject if he *sees* that Alice offers 1 candy? At that point, shouldn't he just accept anyway?

> [!idea]
> The problem here is that a rational player may plan on taking an irrational action in a contingency if she assigns zero probability to that contingency.

The "self-fulfilling prophecy" here is that if a player plans to make a suboptimal action $a$ at a certain information set, then that information set will not be reached; therefore, the suboptimal action is fine. In this game, Alice can actually "call Bob's bluff" and decide to offer 1 candy anyway, even if Bob insists that he will only accept 5. 

In ==dynamic games==, players learn about other players’ actions and possibly the payoffs as  
the game proceeds. Some plans that appear to be rational may turn out to be irrational  
when some unanticipated contingencies arise. This leads to more stringent rationality  
requirements on plans, leading to sharper solution concepts.

> [!example] (Backwards induction)
> The algorithm is really simple: just minimax from the bottom of the game tree. If you ever hit an indifference point, you can randomize arbitrarily.

> [!warning]
> Note that this can only really be applied in games of perfect information. Throwing infosets into the mix complicates matters.

> [!theorem]
> In a finite game of perfect information, every backward induction solution is a Nash equilibrium.

> [!idea] (Inability to commit)
> One of the main reasons certain Nashes fail when analyzed under backwards induction is that commitments cannot be honored (unless they are explicitly part of the game): they must pick the optimal action in their new subtree.

---

**Next:** [[Negotiation]]
