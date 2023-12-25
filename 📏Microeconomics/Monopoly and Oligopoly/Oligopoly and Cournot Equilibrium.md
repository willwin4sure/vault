We've covered two forms of markets: perfect competition (very rare) and monopolies (also rather rare). Most markets are actually <span style="color:#0088ff">oligopolies</span>, somewhere between these two extremes. There are a small number of firms that produce everything in the market.

In an oligopoly, each firm has two options: to cooperate or not to cooperate. Cooperation leads to the formation of a <span style="color:#0088ff">cartel</span>, which essentially acts as a large monopoly that then shares the profits. We'll discuss later why [[Cartels#Why Not Cartels?|cartels don't last that long]]; for now, let's discuss the noncooperative case.
## Game Theory

We will analyze this using game theory. You should already know all about this: Nash equilibria, prisoner's dilemma, payoff matrices, etc.

Prisoner's dilemma literally appears in economics in the example of advertising versus not advertising the capture the market; the dominant cooperative strategy is to both not advertise, while the dominant noncooperative strategy is to both advertise (à la prisoner's dilemma).

How do we solve this problem? Well, we make it a *repeated game*.
## Cournot Model

This is one model of noncooperative oligopolistic equilibria based off of Nash equilibria. We will consider two competing firms in the market, and each firm can decide what quantity to produce. 

> [!definition] Definition (Cournot equilibrium)
> The Cournot equilibrium is the set of quantities for each firm such that holding every other firm's quantity constant, no firm can raise their profits by deviating.

Here is an example. Suppose American Airlines and United Airlines are competing for flights from Boston to NYC. Suppose that the demand market is $P=339-Q_{d}$, and the marginal cost of operating for both airlines is constant at $147$.  ^511e64

### Monopoly Case

First, let's compute what American would do if it were in a monopoly. 

![[airline_monopoly.png|center|256]]

The marginal revenue is
$$
MR=\frac{ \partial }{ \partial q_{A} } (q_{A}(339 - q_{A}))=339-2q_{A}. 
$$
As a monopolist, we should set $MR=MC$, yielding $q_{A}=96$.

### Duopoly Case

Now, suppose that American *knew* that United would provide 64 flights, and wants to optimize its profits.

![[airline_duopoly.png|center|256]]

We should compute the *residual* demand, which is $P=275-q_{A}$. Then, the marginal revenue is
$$
MR=\frac{ \partial  }{ \partial q_{A} } (q_{A}(275-q_{A})) = 275 - 2q_{A},
$$
and equating it to the marginal cost gives $q_{A}=64$, lesser than before.

### Deriving the Cournot Equilibrium

Yet American does not know how many flights United will provide. Instead, they compute the <span style="color:#0088ff">best response curve</span> for every case of the number of flights that United provides, as shown below.

![[airline_cournot.png|center|256]]

Since the problem is symmetric in this case, United's best response curve is just the reflection over the diagonal. The point at which these two curves intersect is the Cournot equilibrium.

To compute the best response curve for American, the residual demand curve is $P=339-q_{A}-q_{U}$. Then, the revenue is $R=339q_{A}-q_{A}^{2}-q_{U}q_{A}$, so the marginal revenue is
$$
MR=\frac{ \partial R }{ \partial q_{A} } = 339 - 2q_{A} - q_{U}.
$$
Setting this equal to $MC=147$ gives $q_{A}=96-\frac{q_{U}}{2}$.

By symmetry, United's best response curve is just $q_{U}=96-\frac{q_{A}}{2}$. Therefore, the Cournot equilibrium occurs at $q_{A}=q_{U}=64$. 

Note that in aggregate, they are producing $128$ flights, which is greater than the monopolistic case, so they are losing out on some profits compared to if they cooperated.
## Many Firms?

> [!idea]
> In the limit of more and more firms, Cournot equilibrium reaches perfect competition.

> [!proposition]
> If there are $n$ firms, the markup is
> $$
> \frac{p-MC}{p}=-\frac{1}{n\varepsilon}.
> $$

## Bertrand Model

This is an alternative to the Cournot model, where instead of firms competing on quantity, they compete on price. In this case, even two firms is enough for the equilibrium to become perfectly competitive (they can keep undercutting each other marginally above marginal cost), unlike above where we need $n\to \infty$ firms.

Which model should we use? Depends on the market. Price competition is more likely when you can instantaneously meet demand (like in the cereal market). Quantity competition is more likely when there are lags (like in the automobile market).

## Product Differentiation

Finally, if you are a company in a perfectly competitive market, you can try differentiating your product so that you have something unique that (at least temporarily) you currently have a monopoly on. This actually improves consumer surplus (since they enjoy the new product), even though people are paying monopolistic prices. Of course, if there is too many products, then we may reach choice overload, an interesting topic in behavioral economics.

---

**Next:** [[Cartels]]