This is a bit more intuition: any sufficiently nice family of strictly positive distributions behaves locally like an exponential family.

First, consider the single parameter case. Given a family $\{ p_{\mathsf{y}}(y;x):x \in \mathcal{X}\subseteq \mathbb{R} \}$ whose members are smooth, then we can expand $\ln p_{\mathsf{y}}(y;x)$ using a Taylor expansion in $x$ (assume $0\in \mathcal{X}$):
$$
\ln p_{\mathsf{y}}(y;x)=\ln p_{\mathsf{y}}(y;0)+x\left[ \frac{ \partial }{ \partial x } \ln p_{\mathsf{y}}(y;x) \right] \bigg|_{x=0}+\mathcal{O}(x^{2}),
$$
as $x\to 0$. Then, $p_{\mathsf{y}}(y;x)$ behaves locally like a canonical exponential family with
$$
t(y)=\left[ \frac{ \partial }{ \partial x } \ln p_{\mathsf{y}}(y;x) \right] \bigg|_{x=0},\quad
\beta(y)=\ln p_{\mathsf{y}}(y;0).
$$

In the multi-parameter case, we would similarly have
$$
\mathbf{t}(\mathbf{y})=\nabla_{\mathbf{x}}\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{x})\bigg|_{\mathbf{x}=\mathbf{0}},\quad
\beta(\mathbf{y})=\ln p_{\boldsymbol{\mathsf{y}}}(\mathbf{y};\mathbf{0})
$$
in a small neighborhood around $\mathbf{0}$.

---

**Next:** [[Sufficient Statistics]]