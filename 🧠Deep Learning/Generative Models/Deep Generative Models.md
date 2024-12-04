A ==generative model== is an algorithm that generates data. Another definition is a joint distribution $p(x,y,\dots)$ of some data. 

> [!example] (Applications of generative models)
> Of course you've seen image and video generation. It can also be applied to biology or medical fields.

## Generating Distributions

So far, we've thought of neural networks as one-to-one mappings $f_{\theta}:\mathcal{X}\to \mathcal{Y}$. Now, we will consider neural networks as mappings $f_{\theta}:\mathcal{X}\to \mathcal{P}(\mathcal{Y})$, where we generate a distribution over possible outputs.

Of course, the neural networks we've seen before probably produce distributions anyway (e.g. via a softmax). However, we will also be interested in cases where the neural network *implicitly* specifies a distribution (e.g. diffusion models), rather than *explicitly* outputting the parameters.

## Noise

You can think of a generator model as the inverse of a classifier model, e.g. instead of feeding in an image of a bird and getting the label `bird`, we feed in a prompt `bird` and expect an image of a bird.

> [!warning]
> The problem here is the backward direction is underspecified!

The solution, of course, is to add randomness, often called "noise". You can think of these random values as changing the shape of the particular bird that is outputted.

> [!idea]
> Noise is latent variables.

## Learning Data Generators

1. **Direct approach**: learn a function $G:\mathcal{Z}\to \mathcal{X}$ to generate the data. This is called an "implicit generative model", since the generator *implicitly* specifies the sampling distribution.
2. **Indirect approach**: learn a function $E:\mathcal{X}\to \mathbb{R}$ that scores the data, and then generate data that scores highly under this function, e.g. via [[Markov Chain Monte Carlo (MCMC)|MCMC]] or [[Monte Carlo Methods#Rejection Sampling|rejection sampling]].

> [!idea]
> For a general density model, we want real data to be maximum likelihood under our model. We can do this by producing the [[Alternating Projections#^c8c715|M-projection]] $p_{\theta}:\mathcal{X}\to \mathbb{R}$ of $p_{\text{data}}$ onto our space of parameterized models.

> [!question]
> Why not just take a delta function spike on each of the real data points? In other words, set $p_{\theta}=p_{\text{data}}$? 

This is the [[Neural Network Generalization#^c653d0|filing cabinet]] model we saw earlier in the class, where we just memorize the training data. This is a terrible solution that never makes anything new.

This is an analogous problem to overfitting; what your data model should do is maximize the likelihood of real data from the real data *distribution*, not just the particular subset in your training set. So keeping a hold-out validation set works well.

## Energy-Based Models

Energy is just an unnormalized probability model, which can be converted to a distribution via Boltzmann:
$$
p_{\theta}=\frac{1}{Z(\theta)}e^{-E_{\theta}},
$$
where $Z(\theta)$ is the partition function. This still supports techniques like [[Markov Chain Monte Carlo (MCMC)||MCMC]] since you can still compute ratios of probabilities even though the partition function is intractable to compute.

To train an energy-based model, you should decrease energy where the real data lives and you should increase data where the model currently places low energy! This is called ==contrastive divergence==.

> [!example] (Contrastive divergence)
> Our goal is still to minimize the cross entropy loss. The corresponding gradient is
> $$
> \nabla_{\theta}\mathbb{E}_{\mathbf{x}\sim p_{\text{data}}}\left[ \log p(\mathbf{x}) \right] = \nabla_{\theta}\mathbb{E}_{\mathbf{x}\sim p_{\text{data}}}\left[ \log \frac{e^{-E_{\theta}(\mathbf{x})}}{Z(\theta)} \right] = -\mathbb{E}_{\mathbf{x}\sim p_{\text{data}}}\left[ \nabla_{\theta}E_{\theta}(\mathbf{x}) \right] - \nabla_{\theta}\log Z(\theta),
> $$
> where we have applied [[Differentiation Under the Integral Sign|differentiation under the integral sign]] to swap the expectation and the gradient. The first term decreases the energy at the data points. It turns out the second term will increase the energy of where the distribution currently is under the model; we can rearrange it to get
> $$
> -\nabla_{\theta}\log Z(\theta)=-\mathbb{E}_{\mathbf{x}\sim p_{\theta}}\left[ \nabla_{\theta}E_{\theta}(\mathbf{x}) \right].
> $$

## Zoo of Models

### Autoregressive Models

This is what an LLM does. For each prefix, you generate a distribution for the next token and sample from it. Then, push the new prefix in again and repeat. This factors the joint distribution via the chain rule
$$
p(\mathbf{x})=p(\mathbf{x}_{1})p(\mathbf{x}_{2}|\mathbf{x}_{1})\cdots p(\mathbf{x}_{n}|\mathbf{x}_{1},\dots,\mathbf{x}_{n-1}).
$$
You can do this in pixel space too: you generate pixels one-by-one, and take a local patch of already-generated pixels to push through your model in order to output a distribution of next pixels.

### Diffusion Models

This is what many image generators do. We take existing images and push them through a diffusion process which converts them into Gaussian noise.

Diffusion models are trained via supervised learning to reverse this diffusion process (i.e. denoising). There are many variations on this, but this is the "vanilla" method.

The forward process from $\mathbf{x}_{0}$ to $\mathbf{x}_{T}$ is to add Gaussian noise via sampling
$$
q(\mathbf{x}_{t}|\mathbf{x}_{t-1})=\mathcal{N}(\sqrt{ 1-\beta_{t} }\mathbf{x}_{t-1},\beta_{t}).
$$
These betas are chosen carefully so in the infinite limit the result has unit Gaussians. The goal of the reverse process is to predict the mean via
$$
p_{\theta}(\mathbf{x}_{t-1}|\mathbf{x}_{t})=\mathcal{N}(f_{\theta}(\mathbf{x}_{t},t),\sigma^{2}).
$$
Note that the model is given the current timestep $t$ so it can condition on the current amount of noise in the image.

### Generative-Adversarial Networks (GANs)

Now you have two competing networks: a generative model $g_{\theta}$ that attempts to synthesize images and a discriminator model $d_{\phi}$ that attempts to identify whether the image is real or synthetic.

By training these two networks in tandem, we end up with a strong generative model.

---

**Next:** [[Variational Autoencoders]]