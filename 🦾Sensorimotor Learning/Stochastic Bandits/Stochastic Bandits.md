> Deciding between a bunch of options with fixed but unknown reward distributions.
## Problem Setup

> [!definition] Definition (Multi-Armed Bandit)
> In the ==multi-armed bandit problem==, the agent has the choice of receiving a reward from one of $N$ options (called "arms", from casino slot machines) from the environment (called the "bandit", because casino slots steal your money). Each arm has a fixed, unknown distribution from which it draws the reward to give you if you pick it.

The multi-armed bandit problem is not hard if every reward is deterministic: then you could simply try each arm and then use the best one.

The difficulty comes from the fact that the reward is stochastic: the reward of each arm is drawn from some distribution. As in other sequential decision making problems, we have to deal with the [[Exploration-Exploitation Tradeoff]].

## Optimization

> [!idea]
> We need to formalize what it means to do well in this problem. The way we model it is minimizing a quantity known as ==regret==.

Suppose we have some oracle that always produces the optimal action $a^{*}$. Then, we define the regret after $T$ time steps of a policy $\pi$ as the difference between the expected reward from following the oracle and the expected reward from following $\pi$.

Obviously we cannot compute the regret while we are learning, but we might be able to do it in hindsight. It is also just a useful metric for theoretical bounds.

> [!idea]
> For an algorithm to work across different bandit problems, we actually want to use ==minimax optimality==.

This means that we want to find a policy $\pi$ that minimizes regret given an *adversarial* bandit problem setup.

> [!claim]
> A lower bound on the minimax regret is $C\sqrt{ NT }$ for some constant $C>0$.

^3bac7a

## Explore then Commit Algorithm

> [!example] Example (ETC Algorithm)
> What it sounds like. First try each arm $K$ times, then commit to the one with the highest expected value.

Potentially not great: you could have high rolled at the start, then in your commit stage, notice that the arm is actually terrible. But then you won't swap away.

> [!claim]
> The regret of the ETC algorithm is
> $$
> R_{T}(\pi,r)\sim T^{2/3}\cdot\mathcal{O}((N\log T)^{1/3}).
> $$
> Here, $K$ would be chosen to be a constant times $T$.

This is not reaching our lower bound! In the next section, we will see a way to get closer to the lower bound.

---

**Next:** [[Upper Confidence Bound Algorithm]]