First, we show a helpful lemma.

> [!theorem] Theorem (Fatou's Lemma)
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of nonnegative measurable functions. Then,
> $$
> \mu\left(\liminf_{ n \to \infty } f_{n}\right)\leq \liminf_{ n \to \infty } \mu(f_{n}).
> $$
> 

^11e9b4

> [!proof]-
> For $k\geq n$, we have
> $$
> \inf_{m\geq n}f_{m}\leq f_{k}.
> $$
> Therefore,
> $$
> \mu\left(\inf_{m\geq n}f_{m}\right)\leq \inf_{k\geq n}\mu(f_{k})\leq \liminf_{ n \to \infty } \mu(f_{n}).
> $$
> However, as $n\to \infty$, we have that
> $$
> \inf_{m\geq n}f_{m}\uparrow \sup_{n}\left( \inf_{m\geq n}f_{m} \right) =\liminf_{ n \to \infty } f_{n}.
> $$
> Therefore, by the monotone convergence theorem,
> $$
> \mu \left( \inf_{m\geq n}f_{m} \right) \uparrow \mu\left(\liminf_{ n \to \infty } f_{n}\right).
> $$
> The result follows.

Next, a longer chain of inequalities that depends on the previous lemma.

> [!theorem] Theorem (Fatou-Lebesgue Theorem)
> Let $(f_{n}:n\in \mathbb{N})$ be a sequence of such functions. Suppose that $g$ is some integrable function such that $|f_{n}|\leq g$ for all $n$. Then, the inequality
> $$
> \mu \left( \liminf_{ n \to \infty } f_{n} \right)\leq \liminf_{ n \to \infty } \mu(f_{n})\leq \limsup_{ n \to \infty } \mu(f_{n})\leq \mu \left( \limsup_{ n \to \infty } f_{n} \right)  
> $$
> holds.

> [!proof]-
> The first inequality follows by Fatou's Lemma on $g+f_{n}$, which is nonnegative. In particular, we can write
> $$
> \mu(g)+\mu(f)=\mu \left( \liminf_{ n \to \infty } (g+f_{n}) \right) \leq \liminf_{ n \to \infty }\mu(g+f_{n})=\mu(g)+\liminf_{ n \to \infty } \mu(f_{n}).
> $$
> Similarly, by Fatou on $g-f_{n}$, we get
> $$
> \mu(g)-\mu(f)=\mu \left( \liminf_{ n \to \infty } (g-f_{n}) \right) \leq \liminf_{ n \to \infty } \mu(g-f_{n})=\mu(g)-\limsup_{ n \to \infty } \mu(f_{n}).
> $$
> Since $g$ is integrable, we can subtract from both sides to get the desired inequality.

---

**Next:** [[Dominated Convergence Theorem]]