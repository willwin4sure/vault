Sequential decision making is difficult since there is an exponential blow-up in the tree search. However, if we make the assumption that any transition only depends on the previous state, the graph greatly reduces in size. 

This leads to a ==dynamic programming== approach. 

## Tabular Setup

Here, we have a discrete state space with stochastic transitions as our model. Then, the value of our policy is
$$
V^{\pi}(s)=R(s,\pi(s))+\gamma \sum_{s' \in S}^{}p(s'|s,\pi(s))V^{\pi}(s'),
$$
i.e. the sum of the immediate reward and the discounted future reward. Before, we didn't know the model $p$, so we had to use Monte-Carlo rollouts. However, here we are assuming we know the stochastic transitions.

An ==optimal policy== $\pi^{*}$ from states to actions satisfies $V^{\pi^{*}}(s)\geq V^{\pi}(s)$ for all states $s$.

> [!claim] (Bellman equation)
> In particular, the optimal policy satisfies
> $$
> V^{\pi^{*}}(s)=\max_{a \in A}\left[ R(s,a) + \gamma \sum_{s' \in S}^{}p(s'|s,a)V^{\pi^{*}}(s') \right]. 
> $$
> We can achieve it by recursively applying the above update.

> [!proof] Duh

## Value Iteration

How can we compute this?

> [!example] (Generalized value iteration)
> There are two steps, the policy evaluation, which occurs over $N$ steps by repeatedly applying the Bellman equation
> $$
> V^{\pi^{k+1}}(s)=R(s,\pi^{k}(s))+\gamma \sum_{s \in S}^{} p(s'|s,\pi^{k}(s))V^{\pi^{k}}(s),
> $$
> and then the policy improvement:
> $$
> \pi^{k+1}(s)=\arg\max_{a \in A}\left[ R(s,a)+\gamma \sum_{s' \in S}^{}p(s'|s,a)V^{\pi^{k+1}}(s') \right]. 
> $$
> If the actions stop changing, we say that we have stabilized and return.

Often times, the policy will converge before the values, since we only care about the ordering of the actions (in particular, just the best action), rather than their actual magnitudes.

Finally, even though there is a unique optimal value function, there may be multiple optimal policies that achieve this. 

> [!example] (Value iteration)
> This is the special case of $N=1$, where we have directly
> $$
> V^{\pi^{k+1}}(s)=\max_{a \in A}\left[ R(s,a) + \gamma \sum_{s' \in S}^{}p(s'|s,a)V^{\pi^{k}}(s) \right].
> $$

We have made a lot of assumptions here. For example, we know all the states $S$, so this is called a ==closed world==. Further, we have a ==discrete state space==, and we have assumed that the number of states is small. Finally, we have knowledge of the transition function.

## Q-Value Iteration

^53ef4f

The ==Q-function== is
$$
Q(s_{t},a_{t})=r(s_{t},a_{t})+\gamma \mathbb{E}_{\tau}\left[ \sum_{t'}^{}\gamma^{t'-t}r_{t'} \right].
$$
Then, Q-value iteration is given by
$$
Q^{\pi^{k+1}}(s,a)=R(s,a)+\gamma \sum_{s' \in S}^{} p(s'|s,a)\max_{a'}Q^{\pi^{k}}(s',a').
$$
However, note that $Q$ actually has no dependence on the policy at all! This is because it is a function of the state *and the action*. This is why it's called an ==off-policy learning== technique.

> [!idea]
> The *data* does not need to come from your current policy to train! It can come from anything: the data collection process is decoupled from policy training. 

---

**Next:** [[Q-Learning]]