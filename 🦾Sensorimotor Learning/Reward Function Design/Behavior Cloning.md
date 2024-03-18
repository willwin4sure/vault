Raw behavior cloning uses an expert dataset $\mathcal{D}$ and uses the gradient update
$$
g_{\theta}=\sum_{(s,a)\in \mathcal{D}}^{}\left[ \nabla_{\theta}\log \pi_{\theta}(a|s) \right].
$$
However, this suffers the problem of ==covariate shift== if the agent goes out of distribution during inference.

One idea is to first behavior clone and then apply a standard reinforcement learning technique after (e.g. PPO) to continue optimization.

Another is to behavior clone and reinforcement learn *at the same time*, using the gradient
$$
\sum_{(s,a)\in \mathcal{D}}^{}w(s,a)\nabla_{\theta}\log \pi_{\theta}(a|s)+\sum_{(s,a)\sim p_{\theta}}^{}\left[ \nabla_{\theta}\log \pi_{\theta}(a|s)\cdot A^{\pi_{\theta}}(s,a) \right], 
$$
where $w(s,a)$ is some weighting. One idea is to use behavior cloning if and only if the data is in distribution. However, this might not be the best if the expert is suboptimal.

Ideally, we would set $w(s,a)=A^{\pi_{\mathcal{D}}}(s,a)$, where $\pi_{\mathcal{D}}$ would be a policy trained only on behavior cloning. However, this is not easily computable so instead we approximate it with the constant weighting
$$
w(s,a)=\lambda_{0}\lambda_{1}^{m}\max_{(s',a')\sim p_{\theta}}A^{\pi_{\theta}}(s',a')
$$
for some hyperparameters $\lambda_{0}$ and $\lambda_{1}$, where $m$ is the number of iterations.



