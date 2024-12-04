Not much to see here: expected value and concave utility functions.

There's also the [Allais paradox](https://en.wikipedia.org/wiki/Allais_paradox), which appears to violate the *independence assumption* that states that your preference between two lotteries $x$ and $y$ is the same as your preference between $\pi$ chance of $x$ and $1-\pi$ chance of $z$ and $\pi$ chance of $y$ and $1-\pi$ chance of $z$, for any other lottery $z$.

> [!definition] (Absolute risk aversion)
> This is the mean-variance tradeoff of some utility function at some wealth $W$ via equating
> $$
> \mathcal{U}(W-y)=\frac{1}{2}\mathcal{U}(W+z)+\frac{1}{2}\mathcal{U}(W-z).
> $$
> Then,
> $$
> \frac{y}{z^{2}}=\frac{1}{2} \frac{-\mathcal{U}''(W)}{\mathcal{U}'(W)}=\frac{1}{2}A(W),
> $$
> where $A(W)$ is called the ==absolute risk aversion==.

> [!example] (CARA utility)
> You've seen this utility function before:
> $$
> \mathcal{U}(x)=-\exp(-Ax).
> $$
> Economists like it because it has constant absolute risk aversion.

> [!definition] (Relative risk aversion)
> We instead consider proportional changes, i.e.
> $$
> \mathcal{U}(W(1-y))=\frac{1}{2}\mathcal{U}(W(1+z))+\frac{1}{2}\mathcal{U}(W(1-z)).
> $$
> Then,
> $$
> \frac{y}{z^{2}}=\frac{1}{2} \frac{-W\mathcal{U}''(W)}{\mathcal{U}'(W)}=\frac{1}{2}R(W),
> $$
> where $R(W)$ is called the ==relative risk aversion==.

> [!example] (CRRA utility)
> You've also seen one of these utility functions before.
> $$
> \mathcal{U}(x)=\begin{cases}
> \frac{x^{1-R}}{1-R} & \text{if }R\neq 1 \\
> \ln(x) & \text{if }R=1
> \end{cases}.
> $$
> So the classic log-wealth utility function just has constant RRA at $1$.

---

**Next:** [[Game Theory]]