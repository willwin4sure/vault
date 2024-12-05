> For example, how modern text-to-image models work.

In particular, we are interested in ==structured prediction== tasks, e.g. image-to-image, text-to-image, image-to-text, and text-to-text. This happens when your desired output is some high-dimensional object. 

> [!idea]
> Our goal is to model some complicated *joint* distribution $P(\mathbf{X}|\mathbf{Y}=\mathbf{y})$ in high dimensions, which doesn't factor. (As opposed to ==unstructured prediction== where it factors are $\prod_{i}^{}P(X_{i}|\mathbf{Y}=\mathbf{y})$).

## Colorization

Here is one example of such a problem:

> [!example] (The colorization problem)
> You are given some grayscale image and need to output a colorized image. Your true distribution often has structure that cannot be captured by simply taking a point prediction (i.e. regression) or something like a Gaussian predictive model: you'll probably end up just outputting a lot of gray.
> 
> The true data distribution is often more complicated and needs to be modeled. For example, if you tried to classify each pixel independently, you'll run into an issue where you don't have consistency across the image:
> 
> ![[fake_colorized.png|center|256]]

> [!claim]
> Generative models have two important properties for structured prediction:
> 
> 1. They can model a multimodal distribution.
> 2. They can model joint dependencies between multidimensional predictions.

## Sat2Map

This is another example.

> [!example] (Sat2Map)
> Suppose you have pairs of maps and satellite images, and you want to train a model that predicts the satellite image from the basic map. If you use some unstructured prediction objective (e.g. least squares on pixel space), you will end up with blurry images:
> 
> ![[sat2map.png|center|512]]
> 
> This is because the model is just averaging values, and averaging over all potential trees in the park, you just get a green blob. 

## Image-to-Image

The issue is that we weren't modeling the joint structure of the data distribution. We will use an *objective function* that can model the structure!

### Conditional GANs

In a [[Deep Generative Models#Generative-Adversarial Networks (GANs)|GAN]], you jointly train a generator $g_{\theta}$ and a discriminator $d_{\phi}$ where $g$ attempts to synthesize fake images to fool $d$. In particular,
$$
d_{\phi}^{*}=\arg\max_{\phi}\mathbb{E}_{\mathbf{x},\mathbf{y}}\left[ \log d_{\phi}(g_{\theta}(\mathbf{x})) + \log(1-d_{\phi}(\mathbf{y})) \right],
$$
i.e. $d_{\phi}$ has codomain $[0,1]$ and attempts to output high probabilities for outputs of $g_{\theta}$ and low probabilities for real images. On the other hand,
$$
g_{\theta}^{*}=\arg\min_{\theta}\mathbb{E}_{\mathbf{x},\mathbf{y}}\left[ \log d_{\phi}(g_{\theta}(\mathbf{x})) \right].
$$
This is a minimax game with equilibrium
$$
\arg\min_{\theta}\max_{\phi}\mathbb{E}_{\mathbf{x},\mathbf{y}}\left[ \log d_{\phi}(g_{\theta}(\mathbf{x}))+\log(1-d_{\phi}(\mathbf{y})) \right],
$$
i.e. we will end up with the best $\theta$ when it knows it must face off against the best $\phi$ in response.

From the perspective of $g$, the function $d$ is like a loss function (also feels like [[Advantage Actor-Critic Method|actor-critic]])!

> [!idea]
> Rather than being some hand-designed error like MSE, the loss function given by $d$ is *learned* and *highly structured*.

For a ==conditional GAN==, $g_{\theta}$ is given some input image and needs to synthesize the corresponding image in the image-to-image task. However, this means that $d_{\phi}$ must also be given the input image, else $g_{\theta}$ could just output some random realistic looking image unrelated to the input.

The *architecture* of the discriminator is critical for determining what your loss function will be sensitive to. One example is a ==patch discriminator==, which grades each patch of the photo as real or fake, rather than the entire image at once.

Here's a cartoon:

![[why_structured_objectives.png|center|512]]

If we apply this to the previous problem of Sat2Map, we get the following result, which is much better!

![[cgan_sat2map.png|center|512]]

### Conditional VAEs

One issue with conditional GANs is mode collapse, where we get good at modeling some modes but don't capture all of them. The idea is to pull the [[Variational Autoencoders|VAE]] trick of treating such a choice as sampling some latent variable injected into the network to model this uncertainty.

We will again use the idea that the latent $\mathbf{z}$ is obtained via an information bottleneck. Suppose we want to model some complicated $p(\mathbf{y}|\mathbf{x})$. Then, we will push $\mathbf{y}$ through an information bottleneck to get a Gaussian latent $\mathbf{z}$, which can be used to reconstruct $\hat{\mathbf{y}}$. This reconstruction will also be given the information $\mathbf{x}$.

> [!idea]
> In a conditional VAE, $\mathbf{z}$ will learn to encode the *missing information* to go from $\mathbf{x}$ to $\mathbf{y}$. In the unconditional case, you need to reconstruct $\mathbf{y}$ fully from $\mathbf{z}$.

Now, to perform sampling, you can just sample $\mathbf{z}\sim \mathcal{N}(0,I)$ (it is imposed to be Gaussian) and then push it through the reconstruction with $\mathbf{x}$. This lets you model multiple possible answers for a given input:

![[conditional_VAEs.png|center|512]]

> [!idea]
> You can control your output data in two ways: explicit inputs via conditioning or random latent variables.

## Text-to-Text

[[Deep Generative Models#Autoregressive Models|Autoregressive models]] are already, by construction, conditional generative models: they predict next tokens of prefixes.

Conditional prediction as a major use case of LLMs came about in the era of GPT-3, when we realized that prompting and instruction are very powerful. 

## Image-to-Text

One powerful way of doing this is via a [[Transformers|transformer]] model; in fact, we've already discussed such an [[Transformers#Encoder v.s. Decoder Models|architecture]]. In particular, note first that both text and image is converted into tokens, i.e. placed on equal footing.

Then, we can use an image encoder model with self-attention to map the image tokens into a prefix that can be placed before text tokens, which are processed via causal self-attention in a text decoder to produce the completion.

## Text-to-Image

One generation back, these were [[Deep Generative Models#Diffusion Models|diffusion models]], though they were actually a combination of diffusion with GANs and VAEs.

Now, we have a domain translation problem. Here's a general strategy:

![[domain_translation.png|center|512]]

We now have training data in the form of pairs $(\mathbf{x},\mathbf{y})$. So we squeeze $\mathbf{x}$ through an information bottleneck (which we can also force to be Gaussian), and then try to reconstruct $\mathbf{y}$ on the other side. 

> [!example] (DALL-E 1)
> DALL-E 1 worked as a conditional VAE that we saw before, where we condition on text via a text encoder. Here is the training procedure, which operates on image-caption pairs:
> 
> ![[dalle1.png|center|512]]
> 
> Then, at inference time, we can simply sample some $\mathbf{z}^{(i)}$ from a Gaussian to feed in with a prompt.  

A more modern approach is diffusion. 

> [!example] (DALL-E 2, Stable Diffusion)
> DALL-E 2 works via conditional diffusion, where your denoiser is simply conditioned on some prompt:
> 
> ![[dalle2.png|center|512]]
> 
> The text provides guidance on what the denoiser should choose. One version of stable diffusion did this via a text encoder conditioned UNet model for denoising:
> 
> ![[stable_diffusion.png|center|512]]
> 
> In modern times, we would use a vision transfomer instead of a UNet, leading to an architecture called a diffusion transformer.

## Mix and Match

Now we have some foundations of multimodal models. These ideas can be used for all sorts of tasks, e.g. visual question answering (VQA). One modern architecture for this is called [LLaVa](https://arxiv.org/abs/2304.08485). All of these ideas can be combined, e.g. in [Taming Transformers](https://arxiv.org/abs/2012.09841):

![[taming_transformers.png|center|512]]

For example, recall [VQ-VAE](obsidian://open?vault=Awareness&file=VQ-VAE) and how it allows us to have discretized tokens we can feed into autoregressive transformer models.

## Unpaired Translation

One issue with everything we've looked at so far is that we need training data consisting of pairs in order to perform the translations. What would we do without such pairs?

One way is to train a generator in both directions, but also train a discriminator on both sides that detects if the they are admissible in the data distributions. This is called [CycleGAN](https://arxiv.org/abs/1703.10593), which enforces bijectivity between the two domains. Our loss function is given by the cycle-consistency loss. 

There is not really a theoretical guarantee that these generators will be good, but the fact that you can cycle usually makes them promising. The architecture and optimizer provide inductive biases to push us towards simple mappings, if they exist. 

---

**Next:** [[Out-of-distribution Generalization]]