In our [[Advantage Actor-Critic Method#^3acd54|actor-critic]] algorithm, how do we choose the parameter update step size $\alpha$? 

## $L^{2}$-Bounded Gradient Steps

We can make the update $d=\alpha\cdot g(\theta)$ small, i.e. $\frac{d^{T}d}{2}<\varepsilon$. Given the objective function $J(\theta)$, we can approximate the update as
$$
J(\theta+d)=J(\theta)+d^{T}g(\theta).
$$
If we want to do constrained optimization, we can work the constraint into our optimization: pick some hyperparameter $\lambda>0$ and determine
$$
\min_{\lambda \geq 0}\max_{d}\left( J(\theta)+d^{T}g(\theta)-\lambda \left( \frac{d^{T}d}{2}-\varepsilon \right) \right).
$$
If the constraint is satisfied, then the coefficient of $\lambda$ is positive so we want to set $\lambda=0$. Otherwise, we the coefficient of $\lambda$ is negative so we can take $\lambda\to \infty$ and make the objective go to $-\infty$. 

Taking the gradient of the inside term with respect to $d$ is just $g-\lambda d$. Optimizing should result in setting $d=\frac{g}{\lambda}$. Well, how do we solve for $\lambda$? We can plug our optimal value for $d$ back in to get
$$
\min_{\lambda \geq 0}\left( J(\theta)+\frac{g^{T}g}{\lambda}-\lambda \left( \frac{g^{T}g}{2\lambda^{2}} - \varepsilon \right)  \right)
=
\min_{\lambda \geq 0}\left( J(\theta)+ \frac{g^{T}g}{2\lambda}+\lambda \varepsilon \right).
$$
Setting the gradient of the inside term with respect to $\lambda$ equal to $0$ gives
$$
- \frac{g^{T}g}{2\lambda^{2}} + \varepsilon = 0 \implies \lambda = \sqrt{ \frac{g^{T}g}{2\varepsilon} }.
$$
This results in:

> [!claim] (Dumb constraint step size)
> Under the constraint $\frac{d^{T}d}{2}<\varepsilon$ for $d=\alpha g$, the optimal step size is
> $$
> \alpha=\sqrt{ \frac{2\varepsilon}{g^{T}g} },
> $$
> where $g$ is the gradient of $J(\theta)$ with respect to $\theta$.

## Natural Policy Gradients

We are constraining each of the dimensions of $\theta$ equally, but this does not accurately represent how it affects $J(\theta)$. A much better distance metric is the KL divergence. We want to maximize $J(\theta+d)$ subject to $KL(\pi_{\theta+d}(\bullet|s)\parallel\pi_{\theta}(\bullet|s))<\varepsilon$. 

We can approximate
$$
KL(\pi_{\theta+d}(\bullet|s)\parallel \pi_{\theta}(\bullet|s))\approx \frac{1}{2}d^{T}F(\theta)d,
$$
where $F(\theta)=\mathbb{E}[\nabla_{\theta}\log \pi_{\theta}(a|s)\nabla_{\theta}\log \pi_{\theta}(a|s)^{T}]$ is the Fisher information matrix. Solving in an analogous way gives

> [!claim] (Natural policy gradients)
> Under the constraint $KL(\pi_{\theta+d}\parallel\pi_{\theta})<\varepsilon$ for $d=\alpha g$, the optimal step size is
> $$
> \alpha=\sqrt{ \frac{2\varepsilon}{g^{T}F^{-1}g} },
> $$
> multiplied by the ==natural gradient== $F^{-1}g$. This is natural because it does not care about how $\theta$ parameterizes our policy $\pi$, since we are only bounding how the policy distribution itself changes.

There is one important question left: how do we choose $\varepsilon$? Well, we definitely want that
$$
J(\theta+d)-J(\theta)>0.
$$
The idea is that we will line search for the largest $d$ such that this holds. We will investigate this problem in the next section.

---

**Next:** [[Trust Region Policy Optimization]]




