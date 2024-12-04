Today, we will consider another [[Scaling Rules for Optimization#^d7feaf|perspective on neural computation]], this time very abstractly as a map through a sequence of vector spaces. In deep learning, we want a good representation of the data at the final layer, e.g. it's linearly separable.

We will develop an advanced tool that shows that a neural architecture (even without training) already expresses an opinion about data similarity.

## Kernel Methods

Suppose we want to fit some one-dimensional data $\{ (x_{i},y_{i}) \}_{i=1}^{N}$. What space of functions should we consider?

One method is to try placing a ==kernel== (a "bump function") on each data point. This gives functions of the form
$$
f(x)=\sum_{i=1}^{n}\alpha_{i}k(x,x_{i}),
$$
where $k(x,x_{i})$ is the "bump" centered at $x_{i}$. Here's a picture of this process:

![[kernel_fitting.png|center|256]]

> [!definition]
> If we consider the space of functions
> $$
> \mathcal{F}=\left\{ f | f(n)=\sum_{i=1}^{\infty}\alpha_{i}k(x,x_{i}) \right\},
> $$
> then it is called a ==reproducing kernel Hilbert space==. 

## Stochastic Processes

Another way of constructing a function space is to draw random functions consistent with the data, i.e. keep sampling from a stochastic process (your prior) and only keep functions that interpolate your data points (to get your posterior).

We will look at a special way of drawing random functions that uses [[Gaussian Spaces and Processes|Gaussian processes]].

> [!claim]
> It turns out that the "mean" of this distribution of functions is the kernel interpolator that is minimal under some natural norm.

Here are some correspondences between the types of functions spaces:

![[correspondences_between_function_spaces.png|center|512]]

In particular, we will see that neural networks in the infinite width limit, with random weights, are equivalent to a Gaussian process.

## Gaussian Processes

You've learned about these formally in [[Gaussian Spaces and Processes|stochastic calculus]], though we won't necessarily restrict to centered processes.

The executive summary is that $f(x)$ is a random variable for all $x \in \mathcal{X}$, and any finite dimensional slice $(f(x_{1}),\dots,f(x_{n}))$ is a jointly Gaussian vector. Recall that such processes have structure defined by a covariance function $\Sigma$, which would dictate the shape of the Gaussian for any finite slice (this generalizes the covariance matrix).

Typically, we would want a covariance function $\Gamma$ that is large for "nearby" points and small otherwise, due to a desire for continuity. The covariance function would encode what we mean by "nearby", e.g.

* The squared exponential $\Sigma(x,x')=e^{-(x-x')^{2}}$.
* The inner product $\Sigma(x,x')=\langle x, x'\rangle$. 

> [!example] (Performing inference with a Gaussian process)
> Suppose we model our function $f$ as coming from a particular Gaussian process, i.e. we have the mean and covariance functions. Then, if we're given data $f(x_{1}),\dots,f(x_{n})$ and we want to predict $f(x_{*})$, we can simply look at the distribution of $f(x_{*})$ *conditional* on our data; this itself will be some Gaussian.
> 
> The mean $\mu(x_{*})$ and variance $\sigma^{2}(x_{*})$ conditional on the data will have simple closed form formulae.

Here's a picture of this process:

![[gaussian_process_fitting.png|center|256]]

## Neural Networks as Gaussian Processes

> [!claim] (NN-GP Correspondence)
> If we sample i.i.d. the weights of a neural network, then as the width goes to infinity, the joint distribution of any finite collection of network outputs $f(x_{1}),\dots, f(x_{n})$ is Gaussian. The covariance function depends on the architecture and non-linearity.

> [!proof]
> 1. For a fixed input and a fixed layer, the activations are i.i.d. random variables. This is proven via induction on depth.
> 2. For any collection of $n$ inputs, the network outputs are jointly Gaussian. This is proven via the [[Central Limit Theorem|central limit theorem]] on the final layer.

We can try this with real MLPs. Here's an example with a CIFAR image and its perturbed versions, on 1000 randomly initialized 3-layer MLPs with width 1000.

![[nn_gp_correspondence.png|center|512]]

Notice that the architecture already expresses some opinion on which inputs should be similar to each other. This can be extracted by considering the infinite width limit and looking at the covariance function of the resulting Gaussian process.

> [!question] (My own question)
> What happens after training? Does it match the conditioning of Gaussian processes?

> [!example] (MLPs with ReLU)
> Consider the following architecture: $L$ layers of weight matrices $W_{i}$.
> 
> * Pointwise non-linearities $\phi(x)=\sqrt{ 2 }\cdot \text{ReLU}(x)$,
> * Weights sampled i.i.d. from $\mathcal{N}\left( 0, \frac{1}{\text{fan\_in}} \right)$.
> 
> Then, for inputs $x,x' \in \mathbb{R}^{d}$, we have that
> 
> * $\mathbb{E}[f(x)]=0$.
> * $\mathbb{E}[f(x)f(x')]=\underbrace{h \circ h \circ \dots \circ h}_{L-1\text{ times}}\left( \frac{x^{T}x'}{d} \right)$, where $h(t)=\frac{1}{\pi}\left[ \sqrt{ 1-t^{2} }+t(\pi-\arccos t) \right]$.
>   
> This is called the ==compositional arccosine kernel==.

> [!warning]
> The initialization above is standard in PyTorch, since it preserves activation magnitudes at initialization (if the inputs are unit variance the outputs are also unit variance).
> 
> However, this might not be optimal, e.g. if the fan in is much larger than the fan out, since at initialization a ton of stuff hits the null space so preserving magnitudes scales up the non-null space, but as training progresses the activations will learn to align with this non-null space, making things much too large.
> 
> This is related to [[Maximal Update Parameterization (muP)]].

> [!question]
> Here are some natural questions.
> 
> 1. How does $\Sigma(x,x')$ depend on the choice of architecture and weight initialization?
> 2. Can this inform architecture design or weight regularization strategies?

Unfortunately, the field of designing neural architectures via limiting covariance structures hasn't emerged. Instead, the modern day meta is to just pick a [[Transformers|transformer]] architecture.

---

**Next:** [[Deep Generative Models]]

