[[Lp as a Banach Space|Last]], we showed that $\mathcal{L}^{p}$ forms a Banach space. It turns out that when $p=2$, the space becomes more structured because there is a natural inner product for our norm.

## Hilbert Spaces

> [!definition] Definition (Inner product)
> A symmetric bilinear map $(u,v)\mapsto \langle u,v \rangle:V\times V\to \mathbb{R}$ is an ==inner product== if $\langle v,v \rangle\geq 0$, with equality if and only if $v=0$. 

> [!definition]
> A complete inner product space is called a ==Hilbert space==.

Note that for any inner product, the map $v\mapsto \sqrt{ \langle v,v \rangle }$ is a norm, by the [[Hölder's Inequality#^2c4b8a|Cauchy-Schwarz inequality]], so all Hilbert spaces are [[Lp as a Banach Space#^627497|Banach spaces]].
## $\mathcal{L}^{2}$ is a Hilbert Space

When $f\in L^{2}$, we have that $\| f \|_{2}^{2}=\langle f,f \rangle$, where the form is given by
$$
\langle f,g \rangle = \int_{E} fg \, d\mu. 
$$
We can apply some general statements about inner products in Hilbert spaces to $L^{2}$:

1. **Pythagoras' rule** states that $\| f+g \|_{2}^{2}=\| f \|_{2}^{2}+2\langle f,g \rangle+\| g \|_{2}^{2}$.
2. The **Parallelogram law** states $\| f+g \|_{2}^{2}+\| f-g \|_{2}^{2}=2\left( \| f \|_{2}^{2}+\| g \|_{2}^{2} \right)$.

When $\langle f,g \rangle=0$, we say that $f$ and $g$ are ==orthogonal==. For any subset $V\subseteq \mathcal{L}^{2}$, define
$$
V^{\perp}=\{ f\in \mathcal{L}^{2} : \langle f,v \rangle =0\text{ for all }v\in V \}.
$$
A subset $V\subseteq \mathcal{L}^{2}$ is ==closed== if, for every sequence $(f_{n}:n\in \mathbb{N})$ in $V$ with $f_{n}\to f$ in $L^{2}$, we have $f=v$ a.e. for some $v\in V$.

> [!theorem] Theorem (Orthogonal Projection)
> Let $V$ be a closed subspace of $L^{2}$. Then each $f\in L^{2}$ has a decomposition $f=v+u$, with $v\in V$ and $u\in V^{\perp}$. Moreover, $\| f-v \|_{2}\leq \| f-g \|_{2}$ for all $g\in V$, with equality only if $g=v$ a.e.

> [!proof]-
> Choose a sequence $g_{n}\in V$ such that
> $$
> \| f-g_{n} \|_{2}\to d(f,V)=\inf\{ \| f-g \|_{2} : g \in V \}.
> $$
> We will show that $g_{n}$ converges to some limit in $V$. By the parallelogram law,
> $$
> \| 2(f-(g_{n}+g_{m}) / 2) \|_{2}^{2}+\| g_{n} - g_{m} \|_{2}^{2} = 2\left( \| f-g_{n} \|_{2}^{2} + \| f-g_{m} \|_{2}^{2} \right). 
> $$
> Yet $\| 2(f-(g_{n}+g_{m} / 2)) \|_{2}^{2}\geq 4d(f,V)^{2}$, so we must have $\| g_{n}-g_{m} \|_{2}\to 0$ as $n,m\to \infty$. By completeness, $\| g_{n}-g \|_{2}\to 0$, for some $g\in L^{2}$. By completeness, $\| g_{n}-g \|_{2}\to 0$ for some $g\in L^{2}$ (the minimum distance is actually realized). By closure, $g=v$ a.e. for some $v\in V$.
> 
> It suffices to show that $v$ is actually the projection. For any $h\in V$ and $t\in \mathbb{R}$, we have
> $$
> d(f,V)^{2}\leq \| f-(v+th) \|_{2}^{2}=d(f,v)^{2}-2t\langle f-v,h \rangle +t^{2}\| h \|_{2}^{2}.
> $$
> So we must have $\langle f-v,h \rangle=0$. Hence $u=f-v\in V^{\perp}$, as desired.

---

**Next:** [[Variance, Covariance, Conditional Expectation]]