A lot of things pass on directly from discrete-time.

The two main examples we want to cover are Brownian motion and $N_{t}-t$ where $N_{t}$ is a standard Poisson process.

## Stopping Times

Work over $(\Omega,\mathcal{F},\mathbb{P})$ and consider a ==filtration== $(\mathcal{F}_{t})_{0\leq t\leq \infty}$ where $\mathcal{F}_{s}\subseteq \mathcal{F}_{t}$ whenever $s\leq t$.

> [!definition]
> A process $(X_{t})_{t\geq 0}$ is ==adapted== to the filtration if $X_{t}\in \mathcal{F}_{t}$ for all $t$.

> [!definition]
> A random variable $\tau:\Omega\to[0,\infty]$ is called a ==stopping time== with respect to $(\mathcal{F}_{t})_{t\geq 0}$ if $\{ \tau \leq t \}\in \mathcal{F}_{t}$ for all $t$, i.e. whether we have stopped at time $t$ is known by time $t$.
> 
> The ==stopping time $\sigma$-algebra== is defined as $\mathcal{F}_{\tau}=\{ A\in \mathcal{F}_{\infty} : A\cap \{ \tau \leq t \} \in \mathcal{F}_{t} \text{ for all } t \}$. Essentially, on the event that $\tau \leq t$, you can figure out whether you are in $A$ based on what is known by time $t$.

> [!warning]
> Be careful about hitting times into open sets, i.e. $=\tau\{ t:B_{t}\in G \}$ where $G$ is open is a stopping time with respect to $\mathcal{F}_{t+}$ but not $\mathcal{F}_{t}$. 
> 
> In particular, if you are at time $t$ and on the boundary of $G$, you don't know whether you will bounce off or actually enter, which is necessary for you to know if you are the infimum.

> [!claim] Claim (Basic facts about stopping times)
> 1. If $\sigma,\tau$ are stopping times with $\sigma \leq\tau$ a.s., then $\mathcal{F}_{\sigma}\subseteq \mathcal{F}_{\tau}$.
> 2. If $\sigma,\tau$ are stopping times, then $\sigma \land \tau$ (minimum) and $\sigma \lor \tau$ (maximum) are also stopping times. In particular, $\mathcal{F}_{\sigma \land \tau}=\mathcal{F}_{\sigma}\cap \mathcal{F}_{\tau}$. 

> [!proof]-
> 1. Note that $\tau \leq t$ implies $\sigma \leq t$, so $\{ \tau \leq t \}=\{ \sigma \leq t \}\cap \{ \tau \leq t \}$. If we have some $A\in \mathcal{F}_{\sigma}$, then $A\cap \{ \tau \leq t \}=(A\cap \{ \sigma \leq t \})\cap \{ \tau \leq t \}$. Both terms are in $\mathcal{F}_{t}$, as desired.
> 2. No more of this.

There are more of these, read the book if you're bored.

So far, things have ported over from discrete time fine. Here is an example of something that doesn't generalize to continuous time:

> [!claim]
> Suppose $(X_{n})_{n\geq 0}$ is adapted to $(\mathcal{F}_{n})_{n\geq 0}$ in **discrete time**. If $\sigma$ is a stopping time, then $X_{\sigma}\in \mathcal{F}_{\sigma}$. 

> [!proof]-
> For any Borel set $A$, we can check that
> $$
> \{ X_{\sigma}\in A \}\cap \{ \sigma \leq n \}
> = \bigcup_{k=0}^{n}\left( \{ \sigma=k \}\cap \{ X_{k}\in A \} \right) \in \mathcal{F}_{n},
> $$
> which implies $\{ X_{\sigma}\in A \}\in \mathcal{F}_{\sigma}$, as desired.

This proof does not work in continuous time, since we would need an uncountable union! However, this is obviously a very natural property that we want. We need to impose some extra regularity for this.

> [!definition]
> Let $(\Omega,\mathcal{F},\mathcal{F}_{t},\mathbb{P})$ be a filtered probability space. A process $X_{t}$ is a ==submartingale== (resp. ==supermartingale==) if it is:
> 1. adapted, i.e. $X_{t}\in \mathcal{F}_{t}$.
> 2. integrable, i.e. $\mathbb{E}[|X_{t}|]<\infty$ for all $t$.
> 3. for all $s\leq t$, $X_{s}\leq \mathbb{E}[X_{t}|\mathcal{F}_{s}]$ (resp. $\geq$).

> [!claim] Claim (Boosting submartingales)
> If $X_{t}$ is a submartingale and $f$ is a nondecreasing convex function, then $f(X_{t})$ is also a submartingale, provided it is still integrable.

> [!proof]- Jensen
> Clearly still adapted. Then, check that
> $$
> \mathbb{E}[f(X_{t})|\mathcal{F}_{s}]\geq f\left( \mathbb{E}[X_{t}|\mathcal{F}_{s}] \right) \geq f(X_{s}).
> $$

This makes sense, since variance only increases the value since we are convex.

> [!example]
> $B_{t}$ is a martingale, as are $B_{t}^{2}-t$ and $\exp \left| \theta B_{t}-\frac{\theta^{2}t}{2} \right|$. The last one has mean $1$ for all $t$ yet converges almost surely to $0$.

> [!claim] (Martingales don't explode)
> Let $X_{t}$ be a sub/super martingale. Then $\sup \{ \mathbb{E}|X_{s}| : 0\leq s\leq t \}<\infty$ for all $t\leq \infty$.

> [!proof]-
> Without loss of generality, $X_{t}$ is a submartingale. Then $(X_{t})_{+}$ is also a submartingale (we are boosting using a ReLU). Then,
> $$
> |X_{t}|=2(X_{t})_{+}-X_{t},
> $$
> which is a difference of two submartingales. Now, we can sandwich this by
> $$
> \mathbb{E}\left[ 2(X_{0})_{+}-X_{t} \right] \leq \mathbb{E}|X_{s}|
> \leq \mathbb{E}\left[ 2(X_{t})_{+} - X_{0} \right] 
> $$
> by the submartingale property, for all $0\leq s \leq t$.

> [!claim] (Weak optional stopping, discrete time)
> Let $(X_{n})_{n\geq 0}$ be a **discrete-time** submartingale, and let $\tau$ be a bounded stopping time, i.e. $0\leq \tau \leq n$ for some finite $n$. Then,
> $$
> \mathbb{E}[X_{0}]\leq \mathbb{E}[X_{\tau}]\leq \mathbb{E}[X_{n}].
> $$

> [!proof]- Omitted

> [!claim] (Doob's maximal inequality, discrete time)
> Let $(X_{n})_{n\geq 0}$ be a **discrete-time** submartingale. Then, for any $\lambda>0$,
> $$
> \lambda\cdot\mathbb{P}\left( \max_{k\leq n} |X_{k}|\geq\lambda \right)\leq \mathbb{E}\left[ |X_{0}|+2|X_{n}| \right].
> $$

> [!proof]-
> The left hand side looks like a Markov bound. Let $\tau=\min\{ k : |X_{k}|\geq\lambda \text{ or }k=n\}$. This is a bounded stopping time. Further, we can write $|X_{k}|=2(X_{k})_{+}-X_{k}$ as difference of two submartingales. Finally, we can rewrite the left hand side as
> $$
> \lambda\cdot \mathbb{P}(|X_{\tau}|\geq\lambda)\leq \mathbb{E}|X_{\tau}|
> = \mathbb{E}[2(X_{\tau})_{+}-X_{\tau}]\leq \mathbb{E}[2(X_{n})_{+}-X_{0}], 
> $$
> which is at most the right hand side.

> [!idea]
> This is saying that for martingales, you can control the entire process using just endpoint information.

This also works for supermartingales. You can get a slight improvement if you restrict to full martingales. In particular, if we denote $A=\{ \max_{k\leq n}|X_{k}|\geq\lambda \}$, then
$$
\lambda \mathbb{P}(A)=\lambda \mathbb{P}(|X_{t}|\geq\lambda)\leq \mathbb{E}[|X_{\tau}|\mathbf{1}_{A}]=\mathbb{E}[((X_{\tau})_{+}+(X_{\tau})_{-})\mathbf{1}_{A}].
$$
Since $X_{t}$ is a martingale, both $(X_{\tau})_{+}$ and $(X_{\tau})_{-}$ are submartingales, which means we can bound this by $\mathbb{E}[|X_{n}|\mathbf{1}_{A}]$.

> [!claim] (Doob's $L^{p}$ inequality, discrete time)
> Let $(X_{n})_{n\geq 0}$ be a **discrete-time** martingale. If $p,q$ are conjugate indices with $p>1$, then
> $$
> \left\| \max_{0\leq k\leq n}|X_{k}| \right\| _{p} \leq q\| X_{n} \|_{p}.
> $$

[!proof]
Call the term inside the norm on the left hand side $S_{n}$. For simplicity, assume that $S_{n}\in L^{p}$. Then,
$$
\begin{align*}
\mathbb{E}[S_{n}^{p}]
&= \int_{0}^{\infty} \mathbb{P}(S_{n}^{p}\geq y) \, dy \\ 
&= \int_{0}^{\infty} py^{p-1}\mathbb{P}(S_{n}\geq y) \, dy \\
&\leq \int_{0}^{\infty} py^{p-1} \frac{\mathbb{E}[|X_{n}|\mathbf{1}_{S_{n}\geq y}]}{y} \, dy \\
&= \frac{p}{p-1}\mathbb{E}\left[ |X_{n}|\int_{0}^{S_{n}} (p-1)y^{p-2} \, dy  \right] \\
&= q\cdot \mathbb{E}[|X_{n}|S_{n}^{p-1}],
\end{align*}
$$
where in the first inequality we used the maximal inequality for martingales. Then, by Hölder, this is at most
$$
q\| |X_{n} \|_{p} \cdot \| S_{n}^{p-1} \|_{q}  
$$
TODO

[!remark]
The above is false with $p=1$. There is a counterexample using simple random walk.
TODO: think about this

For the above two theorems, the **same is true in continuous-time**, if we replace maximums with supremums and assume that $X_{t}$ has **right-continuous sample paths**. If you do this, you can approximate everything with sequences of rationals.







