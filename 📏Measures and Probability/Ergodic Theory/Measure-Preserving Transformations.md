> Ergodicity means that everything gets "mixed together".

## Definitions

> [!definition] Definition (Measure-preserving transformation)
> Let $(E,\mathcal{E},\mu)$ be a measure space. A measurable function $\theta:E\to E$ is said to be a ==measure-preserving transformation== if
> $$
> \mu(\theta ^{-1}(A))=\mu(A)
> $$
> for all $A\in \mathcal{E}$. 

Let $\theta$ be some measure-preserving transformation.

> [!definition] Definition (Invariance of sets and functions)
> A set $A\in \mathcal{E}$ is ==invariant== with respect to $\theta$ if $\theta ^{-1}(A)=A$. A measurable function $f$ is ==invariant== if $f=f\circ\theta$.

The condition for measurable functions is equivalent to all pre-images through $f$ of measurable sets being invariant, as we can rewrite it as $f^{-1}=\theta ^{-1}\circ f^{-1}$.

In particular, the set of all invariant sets forms a $\sigma$-algebra, which we denote by $\mathcal{E}_{\theta}$. Then, $f$ is invariant if and only if it is $\mathcal{E}_{\theta}$-measurable.

> [!definition] Definition (Ergodic)
> We say that a measure-preserving transformation $\theta$ is ==ergodic== if $\mathcal{E}_{\theta}$ contains only sets of measure zero and their complements.

> [!idea]
> This is saying there isn't some nontrivial subset of $E$ that is mapped to itself, and doesn't mix with everything else.

## Examples

> [!example] Example (Translation map on the torus)
> Let $E=[0,1)^{n}$ with the Lebesgue measure on its Borel $\sigma$-algebra, and consider addition modulo $1$ in each coordinate. For $\mathbf{0}\neq a\in E$, we can set
> $$
> \theta_{a}(x_{1},\dots,x_{n})=(x_{1}+a_{1},\dots x_{n}+a_{n}).
> $$
> This is a measure-preserving map, and is ergodic if the $a_{i}$ are rationally independent.

> [!example] Example (Bakers' map)
> Take $E=[0,1)$ with the Lebesgue measure, and consider $\theta(x)=2x-\lfloor 2x \rfloor$. This is measure-preserving and ergodic (intuitively, look at binary expansion).

## Useful Facts

> [!claim]
> If $f$ is integrable and $\theta$ is measure-preserving, then $f\circ\theta$ is integrable and
> $$
> \int_{E}f \, d\mu = \int_{E}f\circ\theta \, d\mu.
> $$

> [!claim]
> If $\theta$ is ergodic and $f$ is invariant, then $f=c$ a.e., for some constant $c$.

Hopefully you can at least intuit why these are true. For example, for the second claim, $\theta$ mixes everything together yet $f$ is invariant to this mixing; thus it must be constant almost everywhere.

---

**Next:** [[Bernoulli Shifts]]