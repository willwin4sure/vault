Now we will complicate the problem setup. 
## Problem Setup

Consider again the problem of music recommendation, where the song we pick is modeled as one of the $N$ arms of the bandit.

Suppose that we actually have some information about the user, which changes the underlying reward distribution of each arm. We refer to these as ==features==, which give us some ==context==.

> [!definition] (Contextual bandits)
> An agent is still interacting with $N$ arms, but this time before every action it is also given some context features $x_{t}$. The hidden reward distribution of each arm is a function of $x_{t}$. 

## Naive Algorithm

> [!example] (Partition and bandit each)
> If the feature space is small, we can just partition by them and run an independent bandit instance per partition. 

This does not scale well with number of features, and also does not use the information given by the features in any meaningful way.

## Disjoint Linear UCB

> [!idea]
> We make the assumption that the rewards from each arm are *linear functions* of the features. We estimate these linear functions online using ridge regression.

This allows us to make predictions $\hat{r}_{a}=\theta_{a}^{T}x_{a}$, where $x_{a} \in \mathbb{R}^{M}$ are our features for the arm corresponding to action $a$ and $\theta_{a}\in \mathbb{R}^{M}$ is some learned parameter per action.

In order to fit the parameters $\theta_{a}$, if we consider all time steps until $t$, we need that
$$
\hat{R}_{0:t, a} = X_{0:t, a}\theta_{a}
$$
closely approximates the true rewards $R_{0:t, a}$ that we have seen ($X_{0:t,a}$ is our design matrix). To do this, we want to use the ridge regression solution
$$
\hat{\theta}_{a}=(X_{0:t,a}^{T}X_{0:t,a}+\lambda I)^{-1}X_{0:t,a}^{T}R_{0:t,a}
$$
for some hyperparameter $\lambda$. However, we need to solve this online for quick updates!

We denote $A_{a}=X_{0:t,a}^{T}X_{0:t,a}+\lambda I$ and $b_{a}=X_{0:t,a}^{T}R_{0:t,a}$, so that $\theta_{a}=A_{a}^{-1}b_{a}$. This facilitates the quick update rules $A_{a}\leftarrow A_{a}+x_{t,a}x_{t,a}^{T}$ and $b_{a}\leftarrow b_{a}+x_{t,a}r_{t}$, given a new (feature, action, reward) tuple $(x_{t,a},a,r_{t})$. 

> [!claim]
> Under the assumption that $R_{0:t,a}=X_{0:t,a}\theta^*+\varepsilon$ where $\varepsilon \in \mathbb{R}^{d}$ is a standard Gaussian, the standard deviation of $\hat{\theta}_{a}x_{a}$ is equal to $\sqrt{ x_{a}^{T}A_{a}^{-1}x_{a} }$.

> [!proof]-
> I will justify this for LSE. For ease of notation, I will drop all subscripts now.
> $$
> \begin{align*}
> \hat{\theta}
> &=(X^{T}X)^{-1}X^{T}R \\
> &=(X^{T}X)^{-1}X^{T}(X\theta ^{*}+\varepsilon) \\
> &=\theta^{*} + (X^{T}X)^{-1}X^{T}\varepsilon.
> \end{align*}
> $$
> Therefore, this is a multivariate Gaussian centered at $\theta^{*}$. The covariance is
> $$
> \text{Var}((X^{T}X)^{-1}X^{T}\varepsilon)
> =\mathbb{E}[(X^{T}X)^{-1}X^{T}\varepsilon\varepsilon^{T}X(X^{T}X)^{-1}]
> =(X^{T}X)^{-1}=A^{-1}.
> $$
> Therefore, projecting it onto $x_{a}$ gives a variance of $x^{T}A^{-1}x$, as desired.

> [!example] (LinUCB algorithm)
> Update parameters efficiently, as specified above. Then, for our upper confidence bound, pick
> $$
> p_{t,a}=\hat{\theta}_{a}^{T}x_{t,a}+\alpha \sqrt{ x_{a}^{T}A_{a}^{-1}x_{a} },
> $$
> where $\alpha$ is a constant hyperparameter. Take the argmax to select an action.

Once again, the first term corresponds to exploitation while the second corresponds to exploration.

## Hybrid Linear UCB

One issue is that we need to keep track of a different $\theta_{a}$ per action $a$. However, there might be a ton of different actions, and it would be better to model relationships between our actions. To do so, we parameterize our actions $a$ and also feed them into the linear model.

This function from (state, action) pairs to predicted rewards is actually a very general technique; e.g. it's called a $Q$-value in reinforcement learning. Details are in [this paper](https://arxiv.org/pdf/1003.0146.pdf).

## Square CB

Works for nonlinear things, came out as recently as [2020](https://arxiv.org/pdf/2002.04926.pdf)! Now, we will assign a probability distribution $p$ over actions. For example, if I want to sample uniformly, I would assign $p_{a}=\frac{1}{|A|}$ for all $a$.

We will use ==inverse gap weighting== to bias the distribution in order to sample higher reward actions more. First, find the optimal action $b$ by the argmax of the function $f(x_{t},a;\theta)$. Then, for all $a\neq b$, we write
$$
p_{a}=\frac{1}{|A|+\gamma(\hat{\mu}_{b}-\hat{\mu}_{a})}
$$
and then take $p_{b}=1-\sum_{a\neq b}^{}p_{a}$, where $\gamma$ is some hyperparameter. Then, we sample an action from this distribution, and then take our observation and update $f$ by applying gradient descent on $\theta$.

---

**Next:** [[⛺Stochastic Bandits Homepage]]