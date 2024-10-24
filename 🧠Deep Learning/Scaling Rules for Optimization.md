We've looked at two of the pieces of the [[Universal Approximation#^c04c40|machine learning puzzle]]: [[Universal Approximation|approximation]] and [[Neural Network Generalization|generalization]]. Now, we turn to optimization.

> [!idea] (The state of things)
> Approximation is a relatively solved problem. In the modern world, people tend to just use a transformer architecture for everything.
> 
> Generalization is also solved by just collecting more data. If you have the entire universe of training data, you won't overfit! (I'm a bit dubious of just trying to train on everything.)

We have many tricks for optimization, which we will discuss today. There is also certainly room for algorithmic improvement.

## The Optimization Problem

Here is the formal problem statement:

> [!problem] (The optimization problem)
> Given:
> 
> * A neural network $f(x,w)$,
> * An error measure $\mathcal{L}(\hat{y},y)$,
> * Training data $\mathcal{D}=\{ (x^{(1)},y^{(1)}),\dots, (x^{(N)},y^{(N)}) \}$,
> 
> we can construct a loss function $\mathcal{L}(w)=\frac{1}{N}\sum_{i=1}^{N}L(f(x^{(i)}, w), y^{(i)})$. The goal of optimization is to find the $w$ that minimizes $\mathcal{L}(w)$.

Of course, you should also care about generalization. Ideally you don't overfit too much to your training data.

> [!warning] (What makes optimization hard?)
> 1. There are lots of weights.
> 2. The neural network is very deep.
> 3. There is a lot of data.

For now, we will ignore the "lots of data" issue, which can be solved using stochastic gradient descent methods. For now, we will assume that we can get the full-batch gradient.

## Classical Methods

> [!definition] (First- and second-order methods)
> ==First-order methods== use 1st derivatives, i.e. the gradient $g=\frac{ \partial \mathcal{L} }{ \partial w }$. ==Second-order methods== also use 2nd derivatives, i.e. the Hessian $H=\frac{ \partial^{2}\mathcal{L} }{ \partial w^{2} }$.

Different classical approaches to optimization take a Taylor expansion as their starting point:

$$
\begin{align}
\mathcal{L}(w+\Delta w) & =\mathcal{L}(w)+\frac{ \partial \mathcal{L} }{ \partial w } ^{T}\Delta w+\frac{1}{2}\Delta w^{T}\frac{ \partial^{2} \mathcal{L} }{ \partial w^{2} }\Delta w + \cdots \\
 &\approx\mathcal{L}(w)+g^{T}\Delta w+\frac{1}{2}\Delta w^{T}H\Delta w.
\end{align}
$$
The first two terms we'll refer to as the ==linearization==, and the rest we'll refer to as the ==non-linear part==. 

> [!example] (Newton's method)
> Optimize the second-order Taylor expansion above by taking the derivative with respect to $\Delta w$ to get
> $$
> g+\frac{1}{2}\cdot(H+H^{T})\Delta w=0 \implies \Delta w=-H^{-1}g.
> $$
> This is standard in optimization theory: you take your gradient and ==precondition== it with the inverse Hessian.

> [!idea]
> If two weights cause correlated shifts in the loss, then you will end up dividing a gradient through one of them evenly across the two.

There are some big issues with Newton's method though:

1. $H$ is a huge matrix, with size that is the square of the number of parameters. Storing and inverting it is computationally infeasible.
2. You might travel to a maxima instead of a minima. One way of fixing this is via cubic regularization.

> [!example] (Gauss-Newton method)
> Suppose we have a "composite" objective $\mathcal{L}=L\circ f$ (which we do). Then,
> $$
> g=\frac{ \partial \mathcal{L} }{ \partial w } = \frac{ \partial L }{ \partial f } \frac{ \partial f }{ \partial w }
> $$
> and
> $$
> H=\frac{ \partial^{2}\mathcal{L} }{ \partial w^{2} } = \frac{ \partial f }{ \partial w } \frac{ \partial^{2} L }{ \partial f^{2} }\frac{ \partial f }{ \partial w } + \frac{ \partial L }{ \partial f } \frac{ \partial^{2}f }{ \partial w^{2} }. 
> $$
> The full Hessian decomposes into two terms. The $\frac{ \partial^{2}L }{ \partial f^{2} }$ term is referred to as the ==curvature of error== while the $\frac{ \partial^{2}f }{ \partial w^{2} }$ term is referred to as the ==curvature of model==.
> 
> Now, for a square loss, $L=\frac{1}{2}(f-y)^{2}$ so $\frac{ \partial^{2} L }{ \partial f^{2} }=1$. Further, we will just ignore the curvature of the model. This allows us to approximate
> $$
> \tilde{H}=\frac{ \partial^{2}L }{ \partial w^{2} } =\frac{ \partial f }{ \partial w } \frac{ \partial f }{ \partial w },
> $$
> which we then plug into the Newton method to get the weight update
> $$
> \Delta w = -\tilde{H}^{-1}g=-\left[ \frac{ \partial f }{ \partial w } \frac{ \partial f }{ \partial w }  \right]^{-1}g.
> $$

Note that $\frac{ \partial f }{ \partial w }$ is not the gradient $g$, which involves the full loss $\mathcal{L}$. This one only involves the model $f$. This also has issues:

1. You still need to make this big matrix and invert it.
2. You need to keep track of these additional gradients.

> [!example] (Steepest descent)
> In this case we replace the entire non-linear part with just the norm of $\Delta w$, i.e.
> $$
> \mathcal{L}(w+\Delta w)\approx \mathcal{L}(w)+g^{T}\Delta w+\frac{\lambda}{2}\| \Delta w \|^{2}.
> $$
> What norm should we pick? Well, some [[Lp Norms|$L^p$ norm]] is usually reasonable. In the case of $L^{2}$ norms, we will get an update step
> $$
> g+\lambda\Delta w=0 \implies\Delta w=-\frac{1}{\lambda}g,
> $$
> which is just basic gradient descent. This is because when we are constrained in a ball, to maximize the dot product with $g$ we just pick a scaled-down version of $g$. If instead we picked an $L^{\infty}$ norm, we will get an update step
> $$
> \Delta w=- \frac{\| g \|_{1}}{\lambda}\cdot\text{sign}(g).
> $$
> This is because when we are constrained in a cube, to maximize the dot product with $g$ we just pick the sign of $g$ in each dimension.

> [!claim] (A homework problem)
> Minimizing the expression above admits the "dual formulation"
> $$
> \arg\min_{\Delta w}\left[ g^{T}\Delta w + \frac{\lambda}{2} \| \Delta w \|^{2} \right] = \underbrace{\frac{\| g \|^{\dagger}}{\lambda}}_{\text{step size}} \cdot \underbrace{\arg\max_{t : \| t \| =1}g^{T}t}_{\text{direction}},
> $$
> where the ==dual norm== $\| \bullet \|^{\dagger}$ of a vector is defined via
> $$
> \| a \|^{\dagger}=\max_{b \in \mathbb{R}^{n}: \| b \|=1}a^{T}b.
> $$

## Heuristics for Scaling

> [!question]
> How large should weight updates be?

We could try various matrix norms: the Frobenius norm, the spectral norm, the nuclear norm, or a Schatten-$p$ norm.

Here are some perspective on neural computation:

1. **Neural** perspective: you have neurons connected by edges.
2. **Tensor** perspective: you work with weight matrices that act on vectors.
3. **Spectral** perspective: decompose each matrix using SVD.

One idea for what stable training is is that the singular values should change stably.

> [!definition] (Spectral norm)
> The ==spectral norm== of a matrix is the maximal scaling the matrix can do to a vector's Euclidean norm:
> $$
> \| M \|_{*}=\max_{v : \| v \|_{2} = 1} \| Mv \|_{2}.
> $$
>

> [!claim]
> The spectral norm of a matrix is the size of its largest singular value.

If you replaced the condition with $v:\| v \|_{q}=1$ and the part being maximized with $\| Mv \|_{p}$ (i.e. swap out the norms), this becomes the ==induced operator norm== $\| M \|_{q \to p}$.

> [!definition] (RMS-RMS operator norm)
> Recall the [[Universal Approximation#^45dea4|RMS norm]] $\| v \|_{\text{RMS}}=\frac{1}{\sqrt{ d }}\| v \|_{2}$. The ==RMS-RMS operator norm== is the induced operator norm
> $$
> \| M \| _{\text{RMS-RMS}}=\max_{v:\| v \|_{\text{RMS}}=1}\| Mv \|_{\text{RMS}}.
> $$
> 

> [!idea]
> In modern transformer architectures, we usually make an effort to normalize the activations of a layer in the RMS norm. This is the LayerNorm (the entries of the vector end up with standard deviation 1).

> [!question]
> Show that $\| \bullet \|_{RMS-RMS}=\sqrt{ \frac{d_{\text{in}}}{d_{\text{out}}} }\| \bullet \|_{*}$, i.e. RMS-RMS norm is a rescaled spectral norm.

## Scaling Woes

Why do we care about these norms? Well, as you try to scale your neural network, some issues may crop up.

![[scaling_woes.png|center|512]]

For example, when you scale width, the optimal learning rate often drifts: you can achieve better performance with wider models but you need to decrease your learning rate appropriately. In modern times, people have been able to fix this drift.

When you scale depth, the performance can get worse as the network gets deeper! In modern times, people have been able to get around this issue with various techniques (e.g. residual blocks).\

### Solving Width Scaling

> See [this paper](https://arxiv.org/pdf/2310.17813) for more.

> [!claim]
> To remove drift in the optimal learning rate as the width is varied, for layers $\ell=1,\dots,L$, do:
> 
> 1. Initialize $W_{\ell}$ so that $\| W_{\ell} \|_{\text{RMS-RMS}}\sim 1$,
> 2. Update by a $\Delta W_{\ell}$ such that $\| \Delta W_{\ell} \|_{\text{RMS-RMS}}\sim 1$.

By keeping the RMS-RMS operator norms near 1, this ensures that all the vectors have RMS norm near 1 as they flow through the network.

### Solving Depth Scaling

The trick seems to be to parameterize your residual block in the "right" way. Recall that
$$
\lim_{ L \to \infty } \left( 1+\frac{x}{L} \right) ^{L}=\exp(x).
$$
Here, we are multiplying a ton of "blocks" together but we end up with something finite even in the limit. This is rather sensitive to the scaling, however.

In a residual block, we have the update $x \mapsto x + \text{Block}(x)$. You should think of this as a multiplicative update on your weights. So the correct scaling for $L$ layers is probably something like $x\mapsto x + \frac{1}{L}\text{Block}(x)$, so that the tower looks like $\left( x+\frac{1}{L}\text{Block(x)} \right)^{L}$, which behaves nicely.

A transformer model will use $x\mapsto x+\frac{1}{\sqrt{ L }}\text{Block}(x)$, the motivation being that each layer behaves like a *linear* update on the weights, and the initial random walk grows with $\sqrt{ L }$.

## Building a Theory: Modularization

This is ongoing research by Jeremy Bernstein, the lecturer. Feel free to be skeptical.

> [!idea]
> If you want an optimization theory that handles complicated neural networks... build the theory together with the neural network. 

A "module" is something with weights $w \in \mathcal{W}$, inputs $x \in \mathcal{X}$, and outputs $y \in \mathcal{Y}$. Then, we write a "library" of atomic modules. As in PyTorch, every module `M` will have the standard `M.forward` and `M.backward` functions.

The new thing is that each module will be equipped with a norm `M.norm`, which is a function $\mathcal{W}\to \mathbb{R}$. Here are some existing layers:

* `ReLU`: no weights, so doesn't need a norm.
* `Linear`: the RMS-RMS norm works well.
* `Conv2D`: ?
* `Embedding`: ?

Then, we will have module composition. For example, if `M(*) = M2(M1(*))`, then we have that

* `M.forward` is just function composition on the individual `forward` functions.
* `M.backward` obeys the chain rule.
* `M.norm` is a bit of an open question... is there an automatic way to compose the norms?

---
\
**Next:** [[]]