Today, we will dive deep into ==variational autoencoders== (VAE). 

Recall the [[Representation Learning I#^550b04|autoencoder]], which learns representations for data via compression through a bottleneck. We could use this as a generative model by simply feeding some [[Deep Generative Models#Noise|noise]] $\mathbf{z}$ through just the decoder in order to produce new data.

> [!warning]
> How do we know what noise $\mathbf{z}$ to pick? Ideally we should sample it from some distribution $p_{\mathbf{z}}$, but it's unclear what to choose in this formulation.

The autoencoder training process gives no guarantees that the learned distribution is easy to sample from. The variational autoencoder aims to solve this problem.

## Mixture of Gaussians

> [!claim] (Perspective 1)
> A VAE is an autoencoder where the encoder maps to a simple distribution, e.g. a Gaussian.

We can then use the back half as a generative model. One bonus is that the learned representations also turn out to be better.

> [!definition] (Mixture of Gaussians)
> A ==mixture of Gaussians== is a [[Mixture Models|mixture model]] between Gaussian distributions, i.e.
> $$
> p_{\theta}(\mathbf{x})=\sum_{i=1}^{k}w_{i}\mathcal{N}(\mathbf{x};\mu_{i},\Sigma_{i})
> $$
> is a mixture between $k$ Gaussians with means $\mu_{i}$ and covariances $\Sigma_{i}$, where the mixture weights are $w_{i}$.

It turns out that we actually care about *infinite* mixtures of Gaussians. This can't be parameterized explicitly, but we can do it implicitly using a neural network $g_{\theta}$ with a finite number of parameters.

> [!definition] (Infinite mixture of Gaussians)
> Suppose $p_{\mathbf{z}}$ is a unit Gaussian distribution. Sample some $\mathbf{z}$ and pass it through a neural network $g_{\theta}$ to get a mean $\mu$ and covariance $\Sigma$, which results in a Gaussian distribution.
> 
> These are the mixture components of our Gaussian, which are weighted by the probability of sampling $\mathbf{z}$ from $p_{\mathbf{z}}$. The implied density is
> $$
> 	p_{\theta}(\mathbf{x})=\int_{\mathbf{z}}p_{\mathbf{z}}(\mathbf{z})\mathcal{N}(\mathbf{x};g_{\theta}^{\mu}(\mathbf{z}), g_{\theta}^{\Sigma}(\mathbf{z})) \, d\mathbf{z}. 
> $$ 

> [!idea]
> We saw this in inference when we discussed [[Variational Inference#Latent Variable Models|latent variable models]]; $\mathbf{z}$ are our latent variables.

> [!example] (Infinite GMM objective)
> The objective is to [[Modeling as Inference|model]] the distribution of data well by optimizing
> $$
> \max_{\theta}\mathbb{E}_{\mathbf{x}\sim p_{\text{data}}}\left[ \log p_{\theta}(\mathbf{x}) \right],
> $$
> where our hypothesis space is given by
> $$
> p_{\theta}(\mathbf{x})=\int_{\mathbf{z}}p_{\theta}(\mathbf{x}|\mathbf{z})p_{\mathbf{z}}(\mathbf{z}) \, d\mathbf{z}, 
> $$
> where $\mathbf{z}$ is drawn from a unit Gaussian and $\mathbf{x}$ conditional on $\mathbf{z}$ is drawn from Gaussians with mean $g_{\theta}^{\mu}(\mathbf{z})$ and covariance $g_{\theta}^{\Sigma}(\mathbf{z})$.
> 
> This process results in both a density model $p_{\theta}:\mathcal{X}\to[0,\infty)$ and a sampler $g_{\theta}:\mathbf{z}\mapsto p(\mathbf{x}|\mathbf{z})$.

Unfortunately, computing the integral above is intractable and requires some additional trickery. Suppose our training data is $\{ \mathbf{x}^{(i)} \}_{i=1}^{N}$. We can rewrite the problem as
$$
\theta ^{*}=\arg\max_{\theta}\sum_{i=1}^{N}\log \int_{\mathbf{z}}\mathcal{N}(\mathbf{x}^{(i)};g_{\theta}^{\mu}(\mathbf{x}),g_{\theta}^{\Sigma}(\mathbf{x}))\cdot\mathcal{N}(\mathbf{z};\mathbf{0},\mathbf{I}) \, d\mathbf{z}.
$$
> [!idea] (Trick #1: Monte Carlo sampling)
> We approximate the integral for $p_{\theta}(\mathbf{x})$ with a discrete sum
> $$
> \frac{1}{M}\sum_{i=1}^{M}p_{\theta}(\mathbf{x}|\mathbf{z}^{(j)}),
> $$
> where $\mathbf{z}^{(j)}$ are sampled from $p_{\mathbf{z}}$.

> [!idea] (Trick #2: Importance sampling)
> If we want to compute $p_{\theta}(\mathbf{x})$, it is much better to sample $\mathbf{z}$ values that are near $\mathbf{x}$ since they will have nontrivial weights. A la [[Monte Carlo Methods#Importance Sampling|importance sampling]], we need to rescale the samples based on the probabilities:
> $$
> \mathbb{E}_{\mathbf{z}\sim p_{\mathbf{z}}}\left[ p_{\theta}(\mathbf{x}|\mathbf{z}) \right] =\int _{\mathbf{z}}q_{\mathbf{z}}(\mathbf{z}) \frac{p_{\mathbf{z}}(\mathbf{z})}{q_{\mathbf{z}}(\mathbf{z})}p_{\theta}(\mathbf{x}|\mathbf{z}) \, d\mathbf{z} = \mathbb{E}_{\mathbf{z}\sim q_{\mathbf{z}}}\left[ \frac{p_{\mathbf{z}}(\mathbf{z})}{q_{\mathbf{z}}(\mathbf{z})}p_{\theta}(\mathbf{x}|\mathbf{z}) \right].
> $$
> It turns out that it is optimal to use $q_{\mathbf{z}}(\mathbf{z})=p_{\theta}(\mathbf{z}|\mathbf{x})$ as our sampling distribution.

> [!idea] (Trick #3: Predict the optimal sampling distribution for each datapoint)
> We can't actually compute the conditional distribution $p_{\theta}(\mathbf{z}|\mathbf{x})$ for sampling, but we can model it using another Gaussian encoder $f_{\psi}$, which outputs a mean and variance too. So our sampling distribution becomes $q_{\psi}(\mathbf{z}|\mathbf{x})=\mathcal{N}(f_{\psi}^{\mu}(\mathbf{x}),f_{\psi}^{\Sigma}(\mathbf{x}))$.
> 
> How do we train this network? Well, we want to maximize the objective
> $$
> \begin{align}
> J_{q}(\mathbf{x},\psi)&=-D_{KL}(q_{\psi}(\mathbf{z}|\mathbf{x})\parallel p_{\theta}(\mathbf{z}|\mathbf{x})) \\
> &=\mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x})}\left[ -\log q_{\psi}(\mathbf{z}|\mathbf{x})+\log p_{\theta}(\mathbf{x}|\mathbf{z})+\log p_{\mathbf{z}}(\mathbf{z}) \right] - \log p_{\theta}(\mathbf{x}),
> \end{align}
> $$
> where equality holds via Bayes' rule.

We now have all the components. The *encoder* $f_{\psi}$, which specifies a distribution $q_{\psi}(\mathbf{z}|\mathbf{x})=\mathcal{N}(f_{\psi}^{\mu}(\mathbf{x}),f_{\psi}^{\Sigma}(\mathbf{x}))$ for each input $\mathbf{x},$ is learned via
$$
\begin{align}
\psi^{*}&=\arg\max_{\psi} \frac{1}{N}\sum_{i=1}^{N}J_{q}(\mathbf{x}^{(i)},\psi) \\
&=\arg\max_{\psi} \frac{1}{N}\sum_{i=1}^{N}\mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x}^{(i)})}\left[ -\log q_{\psi}(\mathbf{z}|\mathbf{x}^{(i)}) + \log p_{\theta}(\mathbf{x}^{(i)}|\mathbf{z}) + \log p_{\mathbf{z}}(\mathbf{z}) \right].
\end{align}
$$
The *decoder* $g_{\theta}$, which also specifies a distribution $p_{\theta}(\mathbf{x}|\mathbf{z})=\mathcal{N}(g_{\theta}^{\mu}(\mathbf{x}),g_{\theta}^{\Sigma}(\mathbf{x}))$, is learned via
$$
\begin{align}
\theta ^{*} &= \arg\max_{\theta} \frac{1}{N}\sum_{i=1}^{N}J_{p}(\mathbf{x}^{(i)},\theta) \\
&=\arg\max_{\theta} \frac{1}{N}\sum_{i=1}^{N} \log \mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x})}\left[ \frac{p_{\mathbf{z}}(\mathbf{z})}{q_{\psi}(\mathbf{z}|\mathbf{x})} p_{\theta}(\mathbf{x}|\mathbf{z}) \right].
\end{align}
$$
There is one final trick, where the current Monte Carlo estimator for the learning problem for $\theta$ actually is a biased estimator, which can be fixed by pulling the log inside of the expectation:
$$
\begin{align}
J_{p}(\mathbf{x},\theta)\geq\mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x})}\left[ \log \left( \frac{p_{\mathbf{z}}(\mathbf{z})}{q_{\psi}(\mathbf{z}|\mathbf{x})}\cdot p_{\theta}(\mathbf{x}|\mathbf{z}) \right)  \right]=J,
\end{align}
$$
which is the real VAE objective. We can rewrite it as
$$
J=\mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x})}\left[ \log p_{\theta}(\mathbf{x}|\mathbf{z}) \right] - D_{KL}(q_{\psi}(\mathbf{z}|\mathbf{x})\parallel p_{\mathbf{z}}),
$$
which is called the ==evidence-based lower bound (ELBO)== (by maximizing this lower bound, we also maximize the real objective). We saw this in [[Variational Inference|variational inference]].

Finally, note that this learning objective $J$ for $g_{\theta}$ is the same objective as the one we had before for $f_{\psi}$.

> [!example] (Variational autoencoder)
> Train an encoder $f_{\psi}$ and decoder $g_{\theta}$ via the objective function
> $$
> \theta^*,\psi^*=\arg\max_{\theta,\psi} \frac{1}{N}\sum_{i=1}^{N} J(\mathbf{x}^{(i)}, \theta, \psi)
> $$
> where
> $$
> J(\mathbf{x}^{(i)},\theta,\psi)=\mathbb{E}_{\mathbf{z}\sim q_{\psi}(\mathbf{z}|\mathbf{x})}\left[ \log p_{\theta}(\mathbf{x}|\mathbf{z}) \right] - D_{KL}(q_{\psi}(\mathbf{z}|\mathbf{x})\parallel p_{\mathbf{z}}).
> $$

> [!idea]
> Intuitively, this is saying to first maximize the log likelihood of the observed data given that our latents are sampled from our encoded distribution. However, this is not the real distribution that the latents are sampled from, so we also penalize by the difference to the real distribution.

Here's a picture of the full process:

![[variational_autoencoder.png|center|512]]

It's basically an encoder but there's an extra term squishing the distribution in the center to a Gaussian. It can't squeeze it entirely since there is also a reconstruction term.

> [!claim] (Training a VAE, step by step)
> 1. Sample one or more $\mathbf{x}\sim \{ \mathbf{x}^{(i)} \}_{i=1}^{N}$.
> 2. *Encode* the data with a forward pass through $f_{\psi}$.
> 3. For each datapoint, create one or more noisy latent codes using the distribution parameterized by the encoder.
> 4. *Decode* the data by passing the noisy latent codes through the $g_{\theta}$.
> 5. Compute the losses and backprop to update $\theta$ and $\psi$.

## Latent Space

It turns out that in autoencoders, and in other generative models such as GANs, movement in certain dimensions in latent space correspond intuitively to changes in the output (e.g. one dimension in latent space might control bird orientation, say).

Here's a dummy example:

![[vae_training.png|center|512]]

Note that when encoding into the Gaussian, you will end up with unfilled "seams" between far-away parts of the original data distribution. Analogously, you will end up with "tendrils" when you decode the full Gaussian back.

## Comparing Generative Models

![[comparing_generative_models.png|center|512]]

---

**Next:** [[Structured Objectives]]
