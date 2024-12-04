## Rational Preferences

We have a weak preference relationship $\succeq$. This induces strict preference $\succ$ and indifference $\sim$ in the obvious ways.

> [!theorem]
> Consider the following four requirements.
> 
> 1. **Complete:** either $(x,y)\succeq (x',y')$ or $(x',y')\succeq (x,y)$.
> 2. **Reflexive:** $(x,y)$ is indifferent to itself.
> 3. **Transitive:** if $(x,y)\succeq (x',y')$ and $(x',y')\succeq (x'',y'')$ then $(x,y)\succeq (x'',y'')$.
> 4. **Continuous:** if $(x,y)\succ (x',y')$, then there exists some $\varepsilon>0$ such that whenever $(x'',y'')$ is within $\varepsilon$ of $(x',y')$, $(x,y)\succ (x'',y'')$ too.
> 
> These imply the existence of a utility function $\mathcal{U}$ such that $\mathcal{U}(x,y)\geq \mathcal{U}(x',y')$ if and only if $(x,y)\succeq (x',y')$.

> [!example] (Continuity is required)
> For example, lexicographical preferences satisfy the first three conditions (you can sort by it), but there's no utility function that represents it.

## Revealed Preference

One way of identifying preferences is by observing what consumers do when presented with multiple bundles in their budget constraint. We say that a bundle $(x^{a},y^{a})$ is revealed to be preferred to $(x^{b},y^{b})$ if $(x^{b},y^{b})$ is within a budget constraint where $(x^{a},y^{a})$ is chosen instead.

> [!claim] (Weak axiom of revealed preference)
> If $(x^{a},y^{a})$ is revealed preferred to $(x^{b},y^{b})$, then $(x^{b},y^{b})$ cannot be revealed preferred to $(x^{a},y^{a})$.

The strong version states that there are no cycles in revealed preference.

> [!theorem]
> Demand functions are consistent with utility maximization if they satisfy:
> 
> 1. **Adding up:** $p_{x}x^{*}(p_{x},p_{y},I)+p_{y}y^{*}(p_{x},p_{y},I)=I$.
> 2. **Homogeneity of degree 0:** $x^{*}(kp_{x},kp_{y},kI)=x^{*}(p_{x},p_{y},I)$.
> 3. **Internal consistency:** the strong axiom of revealed preference.

## Behavioral Departures from Rationality

> [!example]
> 1. Non-transitivity: hamburger > salad, ice cream > hamburger, salad > ice cream.
> 2. Anchoring to initial price points.
> 3. Endowment effect where if given a good, value it higher.
> 4. Default effect where you choose the default more often.

---

**Return:** [[⛺Consumer Theory Homepage]]