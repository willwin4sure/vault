We will show that integration with respect to the Lebesgue measure on $\mathbb{R}$ acts as an inverse to differentiation. Since here we restrict to the integration of continuous functions, the proof is the same as for Riemann integrals.

## Statement

> [!theorem] Theorem (Fundamental Theorem of Calculus)
> 1. Let $f:[a,b]\to \mathbb{R}$ be a continuous function and set
> $$
> F_{a}(t)=\int_{a}^{t} f(x) \, dx.
> $$
> Then, $F_{a}$ is differentiable on $[a,b]$, with $F_{a}'=f$.
> 2. Let $F:[a,b]\to \mathbb{R}$ be differentiable with continuous derivative $f$. Then
> $$
> \int_{a}^{b} f(x) \, dx=F(b)-F(a). 
> $$

^52fa03

## Proof

Fix $t\in[a,b)$. Given $\varepsilon>0$, there exists $\delta>0$ such that $|f(x)-f(t)|\leq\varepsilon$ whenever $|x-t|\leq\delta$. Therefore, for $0<h\leq\delta$,
$$
\begin{align*}
\left| \frac{F_{a}(t+h)-F_{a}(t)}{h} - f(t) \right| &= \frac{1}{h}\left| \int_{t}^{t+h} f(x)-f(t) \, dx  \right| \\
&\leq \frac{1}{h}\int_{t}^{t+h} |f(x)-f(t)| \, dx \\
&\leq \frac{\varepsilon}{h}\int_{t}^{t+h} \, dx =\varepsilon.
\end{align*}
$$
Therefore, $F_{a}$ is differentiable on the right at $t$ with derivative $f(t)$. Similarly, for all $t\in(a,b]$, $F_{a}$ is differentiable on the left at $t$ with derivative $f(t)$. Finally, $(F-F_{a})'(t)=0$ for all $t\in(a,b)$, so $F-F_{a}$ is constant (by the mean value theorem), and so
$$
F(b)-F(a)=F_{a}(b)-F_{a}(a)=\int_{a}^{b} f(x) \, dx .
$$
---

**Next:** [[u-Substitution]]