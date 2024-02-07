#MIT #math #probability #02/05/24
## Constructing Brownian Motion

Our first goal is to give an informal introduction to *Brownian motion*. 

We all know the ==standard Gaussian density== on $\mathbb{R}$, given by
$$
g(x)=\frac{1}{2\pi}\exp \left( -\frac{x^2}{2} \right).
$$
Then, we write $Z\sim \mathcal{N}(0,1)$ if $Z$ is an $\mathbb{R}$-valued random variable with density $g$.

If we consider i.i.d. random variables $X_{1},X_{2},\dots,X_{n}\sim \text{Unif}(\{\pm 1\})$ and their sum $S_{n}=\sum_{i=1}^{n}X_{i}$, then by the central limit theorem, $\frac{S_{n}}{\sqrt{ n }}\xrightarrow[]{(d)}\mathcal{N}(0,1)$. 

This is called a ==simple random walk== $(S_{n})$. Over time $n$, $S_{n}$ typically covers $\mathcal{O}(\sqrt{ n })$ distance.

> [!idea]
> Rescale time by $n$ and space by $\sqrt{ n }$.

This gives us a process $X^{(n)}(t)=\frac{1}{\sqrt{ n }}S_{nt}$. For any *fixed* time $t\geq 0$,
$$
X^{(n)}(t)\xrightarrow[n\to \infty]{(d)}\mathcal{N}(0,t)
$$
by the central limit theorem. In fact, the entire process $\left( X^{(n)}(t) \right)_{t\geq 0}$ converges in distribution as $n\to \infty$ to ==standard Brownian motion== $(B(t))_{t\geq 0}$. This convergence is with respect to a suitable topology on the space of paths.

Some important basic properties are that
* For all $s\leq t$, $B_{t}-B_{s}\sim \mathcal{N}(0,t-s)$.
* For all $t_{1}\leq t_{2}\leq t_{3}\leq t_{4}$, $B_{t_{4}}-B_{t_{3}}$ is independent to $B_{t_{2}}-B_{t_{1}}$.

Later in the class, we will give a formal, rigorous definition.

## Itô's Formula

Suppose we are given the stochastic differential equation
$$
dX_{t}=\mu_{t}dt+\sigma_{t}dB_{t}.
$$
The first term is ==drift== (it's some *signal*), while the second term is ==stochastic== (it's the *noise*). For now, we can think of this informally as $X_{t+dt}-X_{t}=\mu_{t}\cdot dt+\sigma_{t}\cdot \mathcal{N}(0,dt)$.

The size of the second term is typically much much larger, as it is order $\sqrt{ dt }$ rather than order $dt$. In a deterministic process, the second term would wash out the first one. However, because it is actually random, these independent large changes will average out.

Now, let $f:\mathbb{R}\to \mathbb{R}$ be a twice-differentiable function. ==Itô's formula== characterizes the behavior of $f(X_{t})$, as follows.

> [!theorem] Theorem (Itô's Formula)
> $$
> df(X_{t})=\left( f'(X_{t})\mu_{t} + \frac{f''(X_{t})\sigma_{t}^2}{2} \right)dt + f'(X_{t})\sigma_{t}dB_{t}. 
> $$

^86f449

We can derive this mathematically. First, by a Taylor expansion, we can write
$$
\begin{align*}
df(X_{t)})&=f'(X_{t})dX_{t}+\frac{1}{2}f''(X_{t})(dX_{t})^2\\
&=f'(X_{t})(\mu_{t}dt+\sigma_{t}dB_{t})+\frac{1}{2}f''(X_{t})(\mu_{t}dt+\sigma_{t}dB_{t})^{2}.
\end{align*}
$$
The first term is left as is, while for the second term, since $dB_{t}$ is on order $\sqrt{ dt }$, the only nonnegligible term is $\sigma_{t}^2(dB_{t})^2$. However, we can also note that $(dB_{t})^2-dt$ is a mean zero random variable with variance on order $(dt)^2$, so it is negligible when aggregated over time. 

Summing terms gives the desired formula.

## Brownian Motion is Conformally Invariant

First, let's review a bit of complex analysis. Given a function $f:D\to \mathbb{C}$ where $D\subseteq \mathbb{C}$ is open, we say that $f$ is ==holomorphic== (complex differentiable) at $z\in D$ if
$$
f'(z)=\lim_{ h \to 0 } \frac{f(z+h)-f(z)}{h}.
$$
Note that $h\to 0$ in $\mathbb{C}$; in particular, the limit must work from any direction. We will write $z=x+iy$ and $f=u+iv$ where $x$, $y$, $u$, and $v$ are real-valued. 

By equating the cases where $h\to 0$ along $\mathbb{R}$ or $i\mathbb{R}$, we get that
$$
u_{x}+iv_{x}=\frac{ \partial f }{ \partial x } =\frac{1}{i}\frac{ \partial f }{ \partial y }=v_{y}-iu_{y}.
$$
This gives us the ==Cauchy-Riemann equations==
$$
\begin{align*}
u_{x}&=v_{y},\\
v_{x}&=-u_{y}.
\end{align*}
$$
This further implies that both $u(x,y)$ and $v(x,y)$ are ==harmonic==, as $\nabla^2u=\nabla^2v=0$.

Finally, we say that $f:D\to D'$ is ==conformal== if it is holomorphic with a holomorphic inverse, i.e. $f'(z)\neq0$ everywhere.

We define this because we want to prove the following famous fact:

> [!claim]
> Brownian motion in $\mathbb{C}$ is conformally invariant.

First, we need to define Brownian motion in $\mathbb{C}$. This is done the way you expect: let $(X_{t},Y_{t})$ be two standard Brownian motions in $\mathbb{R}$. Then, $Z_{t}=X_{t}+iY_{t}$ is a standard Brownian motion in $\mathbb{C}$.

Now, let $f:D\to D'$ be conformal, and write $f=u+iv$. We will start a Brownian motion $Z_{t}$ inside $D$, which terminates once it hits the boundary.

We can similarly derive [[Informal Overview#^86f449|Itô's Formula]] in two dimensions. We can use the same Taylor expansion idea to get
$$
du(Z_{t})=\begin{bmatrix}
u_{x}(Z_{t}) & u_{y}(Z_{t})
\end{bmatrix}\begin{bmatrix}
dX_t \\
dY_{t}
\end{bmatrix}+\frac{1}{2}\begin{bmatrix}
dX_{t} & dY_{t}
\end{bmatrix}\begin{bmatrix}
u_{xx} & u_{xy} \\
u_{yx} & u_{yy}
\end{bmatrix}\begin{bmatrix}
dX_{t} \\
dY_{t}
\end{bmatrix}.
$$
Let's analyze the second order term. The cross terms are of the form $u_{xy}dX_{t}dY_{t}$, which have mean zero and variance on the order of $(dt)^2$; these vanish. The main diagonal term is nonnegligibly different from $\frac{1}{2}(u_{xx}+u_{yy})dt$, which also vanishes since $u$ is harmonic.

Therefore, we can write
$$
\begin{bmatrix}
du(Z_{t}) \\
dv(Z_{t})
\end{bmatrix}=\begin{bmatrix}
u_{x} & u_{y} \\
v_{x} & v_{y}
\end{bmatrix}\begin{bmatrix}
dX_{t} \\
dY_{t}
\end{bmatrix}.
$$
However, the Cauchy-Riemann equations imply that this is just a spiral similarity: it's some rotation matrix plus and dilation by $|f'(z)|$. Yet the standard 2D Gaussian is rotationally symmetric, so the rotation does nothing to the distribution. The scaling also doesn't affect the ==trace== of the Brownian motion; it only changes the time reparameterization. 

---

[[🚶Stochastic Calculus Portal]]