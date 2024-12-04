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

