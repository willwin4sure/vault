We build neural networks like Lego, in modularized fashion out of individual building blocks. Can we also write a theory of neural networks that is modular?

A layer consists of: an input $\mathbf{x}$, some weights $\mathbf{w}$, and an output $\mathbf{y}$.

> [!question]
> How **sensitive** is the output to the weights and the inputs?

This is a fundamental question about neural networks that informs how we train neural networks and ensure they are robust to adversarial inputs.

> [!question]
> Suppose we understand the sensitivity of individual layers. Can we extend this to combinations?

Ideally, each individual layer should have its own theory, and we should have a system of rules for combining layers. Then, the theoretical framework extends to arbitrary networks! So far, this already has some practical payoffs in terms of fixing hyperparameter scaling issues and achieving new nanoGPT speed records with the [[Muon Optimizer|Muon optimizer]].

## I. Optimization Theory

We've discussed some of these topics before when talking about [[Scaling Rules for Optimization|scaling rules for optimization]]. Recall that many optimizers can be viewed as [[Scaling Rules for Optimization#^8409b1|steepest descent]] where we approximate the Hessian with a particular norm and sharpness parameter.

### Bounding the Hessian

The goal is to find some bound
$$
\Delta \mathbf{w}^{T} (\nabla^{2}_{w}\mathcal{L}) \Delta \mathbf{w}\leq \lambda \| \Delta \mathbf{w} \|^{2}.
$$
We know that the loss function is a composite
$$
\mathcal{L}(\mathbf{w})=\ell \circ f(\mathbf{w}),
$$
where $\ell$ is our error measure and $f$ is our neural network. Recall that the [[Scaling Rules for Optimization#^10183f|Gauss-Newton method]] allows us to decompose the Hessian into
$$
\Delta \mathbf{w}^{T}(\nabla_{w}^{2}\mathcal{L})\Delta \mathbf{w}=\Delta \mathbf{w} (\nabla_{w}^{2}f)\Delta \mathbf{w}(\nabla_{f}\ell) + \Delta \mathbf{w}(\nabla_{w}f)(\nabla_{f}^{2}\ell)(\nabla_{w}f)\Delta \mathbf{w},
$$
where the first term involves the curvature of the neural network and the second involves the curvature of the error measure.

Suppose we know a good norm $\| \bullet \|$ on the network output. This is usually something simple like a vector, so the standard norms you might pick include [[Lp Norms|$L^p$-norms]] or the [[Universal Approximation#^45dea4|RMS norm]]. 

Then, we can bound the Gauss-Newton decomposition via Cauchy-Schwarz to get that it is at most
$$
\| \Delta \mathbf{w}(\nabla_{w}^{2}f)\Delta \mathbf{w} \| \underbrace{\| \nabla_{f}\ell \|}_{\text{dual norm}} + \underbrace{\| \nabla_{f}^{2}\ell \|}_{\text{operator norm}}\| (\nabla_{w}f)\Delta w \|^{2}. 
$$
Therefore, our problem reduces to showing:

1. Our network is ==Lipschitz smooth==, i.e.
$$
\| \Delta \mathbf{w}(\nabla_{w}^{2}f)\Delta \mathbf{w} \| \leq \alpha \| \Delta \mathbf{w} \| ^{2}.
$$
2. Our network is ==Lipschitz==, i.e.
$$
\| (\nabla_{w}f)\Delta \mathbf{w} \|^{2}\leq\delta \| \Delta \mathbf{w} \|^{2}.
$$
We are seeking a norm on the weight space that gives us convenient upper bounds here. One way of thinking about this is via the Taylor expansion of the network output in terms of its weights:
$$
f(\mathbf{x};\mathbf{w}+\Delta \mathbf{w})=f(\mathbf{x};\mathbf{w})+(\nabla_{w}f)\Delta \mathbf{w}+\Delta \mathbf{w}(\nabla_{w}^{2}f)\Delta \mathbf{w}+\cdots
$$
Such a norm on the weight space would be very useful since it bounds how much your network output can change given a certain change in weights.

### Producing a Weight Norm

How do we produce a good norm to satisfy these constraints? One issue is that neural networks are rather complicated.

One way is to first find norms on the activations that pass through the network. These then induce norms on the operators between them. Finally, we need to take these layer-wise norms and combine them into a norm on the full weight space.

## II. The Theory of Modules

We define a ==module== as any box that takes in inputs $\mathbf{x}$ and weights $\mathbf{w}$ and returns some output $\mathbf{y}$. Given two modules, they can *composed* via composition or concatenation: these correspond to running the modules in series or in parallel.

You have some basic utility modules such as addition and multiplication. These can be composed to build very complicated neural networks.

### Types of Modules

We identify three kinds of modules:

1. **Atoms:** these have hand-declared attributes, e.g. `Linear`, `Conv2d`, or `Embed`.
2. **Bonds:** these have hand-declared attributes and no weights, e.g. `ReLU` or `FunctionalAttention`.
3. **Compounds:** anything else constructed from atoms and bonds.

Now, the ==sensitivity== of a module is how the size of $\Delta \mathbf{y}$ depends on $\Delta \mathbf{x}$ and $\Delta \mathbf{w}$. Then, if we can characterize this for atoms and bonds, how can we generalize to compounds?

### Formal Definition of a Module

> [!definition] (Module)
> A ==module== $M$ must have three vector spaces: an input space $\mathcal{X}$, a weight space $\mathcal{W}$, and an output space $\mathcal{Y}$. It also has four attributes:
> 
> 1. A function `M.forward` that maps $\mathcal{X}\times \mathcal{W}\to \mathcal{Y}$.
> 2. A number `M.sensitivity` in $\mathbb{R}^{+}$.
> 3. A number `M.mass` in $\mathbb{R}^{+}$.
> 4. A norm `M.norm` that maps $\mathcal{W}\to \mathbb{R}^{+}$.

> [!definition] (Well-normed module)
> A module $M$ is ==well-normed== if:
> 
> 1. The input space $\mathcal{X}$ has norm $\| \bullet \|_{\mathcal{X}}$.
> 2. The output space $\mathcal{Y}$ has norm $\| \bullet \|_{\mathcal{Y}}$.
> 
> And the first derivatives of the module satisfy
> 
> 1. $\| \nabla_{w}M \diamond \Delta \mathbf{w} \|\leq \text{M.norm}(\Delta \mathbf{w})$.
> 2. $\| \nabla_{x}M \diamond \Delta \mathbf{x} \|\leq \| \Delta \mathbf{x} \|_{\mathcal{X}}$.

### Some Atomic Modules

> [!example] (Linear module $L$)
> We can let:
> 
> * $\text{L.forward}(\mathbf{w},\mathbf{x})=\mathbf{w}\mathbf{x}$.
> * $\text{L.sensitivity} = 1$.
> * $\text{L.mass} = 1$.
> * $\text{L.norm} = \| \bullet \|_{*} \cdot \sqrt{ \text{fan-in} / \text{fan-out} }$.
> 
> Then, if we equip the input and output spaces with the RMS-norm, then it is well-normed so long as $\| \mathbf{x} \|_{\mathcal{X}}\leq 1$ and $\text{L.norm}(\mathbf{w})\leq 1$.

Basically, we just want to have bounded inputs and bounded weights in the spectral norm. This might be a reason why explicitly normalizing features inside neural networks may be helpful; it provides Lipschitz guarantees.

> [!example] (Embedding module $E$)
> We can let
> 
> * $\text{E.forward}(\mathbf{w},\mathbf{x})=\mathbf{w}\mathbf{x}$.
> * $\text{E.sensitivity} = 1$.
> * $\text{E.mass} = 1$.
> * $\text{E.norm} = \max_{i}\| \text{column}_{i}(\mathbf{w}) \|_{\text{RMS}}$.
> 
> Even though the forward function is the same as the linear module, this module has very different semantics, which we encode by giving it a different norm. This is reasonable since you only pass in one-hot vectors and get columns as outputs.
> 
> Then, if we equip the input and output spaces with the RMS-norm, then it is well-normed under the same conditions $\| \mathbf{x} \|_{\mathcal{X}}\leq 1$ and $\text{L.norm}(\mathbf{w})\leq 1$.

### Composition

You want compound modules to be automatically good. Here are some key properties:

1. Module combination is associative.
2. Module combination preserves well-normed-ness.
3. Feature learning is apportioned by mass.

This is where mass comes it, it allows you to tune learning rates of different layers if so desirable.

> [!definition] (Module composition)
> Given two modules $M_{1}$ and $M_{2}$, their composite is $M=M_{2}\circ M_{1}$ with attributes:
> 
> 1. $\text{M.forward} = \text{M}_{2}\text{.forward} \circ \text{M}_{1}\text{.forward}$.
> 2. $\text{M.sensitivity}=\text{M}_{1}\text{.sensitivity} \cdot \text{M}_{2}\text{.sensitivity}$ (Lipschitz constants multiply).
> 3. $\text{M.mass}=\text{M}_{1}\text{.mass}+\text{M}_{2}\text{.mass}$.
> 4. $\text{M.norm}=\max(p\cdot \text{M}_{1}\text{.norm},q\cdot \text{M}_{2}\text{.norm})$.
> 
> Here, $p=\text{M.mass} / \text{M}_{1}\text{.mass} \cdot \text{M}_{2}\text{.sensitivity}$ and $q=\text{M.mass} / \text{M}_{2}\text{.mass}$. This lets you reweight the composite norm using the masses. Further, the first module is given information about the sensitivity of the module it feeds into next.

> [!definition] (Module concatenation)
> Given two modules $M_{1}$ and $M_{2}$ their tuple $M=(M_{1},M_{2})$ is the module with attributes:
> 
> 1. $\text{M.forward}=(\text{M}_{1}\text{.forward},\text{M}_{2}\text{.forward})$.
> 2. $\text{M.sensitivity}=\text{M}_{1}\text{.sensitivity}+\text{M}_{2}\text{.sensitivity}$.
> 3. $\text{M.mass}=\text{M}_{1}\text{.mass}+\text{M}_{2}\text{.mass}$.
> 4. $\text{M.norm}=\max(p\cdot \text{M}_{1}\text{.norm},q\cdot \text{M}_{2}\text{.norm})$.
> 
> Here, $p=\text{M.mass} / \text{M}_{1}\text{.mass}$ and $q=\text{M.mass} / \text{M}_{2}\text{.mass}$.

You can prove that these compositions preserve well-normedness.

### Second Order

We can extend the theory to the second order.

> [!definition] (Module sharpness)
> A module $M$ is $(\alpha,\beta,\gamma)$-sharp if second derivatives obey:
> 
> 1. $\| \Delta \mathbf{w} \nabla_{w}\nabla_{w}M \Delta \tilde{\mathbf{w}} \|_{\mathcal{Y}}\leq \alpha \cdot \text{M.norm}(\Delta \mathbf{w}) \cdot \text{M.norm}(\Delta  \tilde{\mathbf{w}})$.
> 2. $\| \Delta \mathbf{x} \nabla_{x}\nabla_{w}M \Delta \mathbf{w} \|_{\mathcal{Y}}\leq \beta \cdot \text{M.norm}(\Delta \mathbf{w}) \cdot \| \Delta \mathbf{x} \|_{\mathcal{X}}$.
> 3. $\| \Delta \mathbf{x} \nabla_{x} \nabla_{x} W \Delta \tilde{\mathbf{x}} \|\leq \gamma \cdot \| \Delta \mathbf{x} \|_{\mathcal{X}} \cdot \| \Delta \tilde{\mathbf{x}} \|_{\mathcal{X}}$.

Then, it turns out that the sharpness tuple $(\alpha,\beta,\gamma)$ obeys associative combination laws, and that neural network loss functions are Lipschitz smooth in the modular norm with non-dimensional Lipschitz constants, so long as the error measure is smooth in the module output.

## III. Scaling

> [!idea] (Thesis for good scaling)
> Given non-dimensional and tight Lipschitzness, we will see learning rate transfer across scale.

In particular, for a generic module $M$ we want to achieve:

1. Lipschitz constants independent of width, depth, etc.
2. Bounds that stay tight across scale.

Then, controlling $\text{M.norm}(\Delta \mathbf{w})$ gives us control over $\| \Delta \mathbf{y} \|_{\mathcal{Y}}$. Formally, we want $\| \Delta \mathbf{y} \|_{\mathcal{Y}}\leq \text{M.norm}(\Delta \mathbf{w})$.

Some interesting papers are [On the distance between two neural networks and the stability of learning](https://arxiv.org/abs/2002.03432) and [A spectral condition for feature learning](https://arxiv.org/abs/2310.17813). For linear layers, one way of getting the guarantees we want is imposing the spectral conditions
$$
\sqrt{ \text{fan-in} / \text{fan-out} } \cdot \| \mathbf{w} \|_{*} = 1,\quad 
\sqrt{ \text{fan-in} / \text{fan-out} } \cdot \| \Delta \mathbf{w} \|_{*} = \text{LR}. 
$$
See also [Scalable optimization in the modular norm](https://arxiv.org/abs/2405.14813). Bernstein is working on the package [`modula`](https://github.com/modula-systems/modula) to implement these ideas. For example, here would be some simple code to train a ReLU network with the correct LR transfer:

```python
import torch

from modula.atom import Linear
from modula.bond import ReLU

mlp = Linear(10, 10000) @ ReLU() @ Linear(10000, 1000)

weights = mlp.initialize(device="cpu")
data, target = torch.randn(1000), torch.randn(10)

for step in range(steps := 20):
	output = mlp.forward(data, weights)
	loss = (target - output).square().mean()
	loss.backward()

	with torch.no_grad():
		grad = weights.grad()
		mlp.normalize(grad)  # Package should be updated to say "dualize"
		weights -= 0.1 * grad
		weights.zero_grad()
```

## IV. Modular Duality

Suppose we have a good weight norm, so that we can bound
$$
\mathcal{L}(\mathbf{w}+\Delta \mathbf{w})\leq \mathcal{L}(\mathbf{w})+(\nabla_{w}\mathcal{L})^{T}\Delta \mathbf{w} + \frac{1}{2}\lambda \| \Delta \mathbf{w} \|^{2}.
$$
Recall from the [[Scaling Rules for Optimization#^817bd4|solution to steepest descent]] we can solve the optimization step via
$$
- \underbrace{\| \nabla_{w}\mathcal{L} \|^{\dagger} / \lambda}_{\text{step size}} \cdot \underbrace{\arg\max_{\| \Delta \mathbf{w} \| =1} (\nabla_{w}\mathcal{L})^{T}\Delta \mathbf{w}}_{\text{direction}}.
$$
The right term can be interpreted as a "duality map". Note that [[Actor-Critic Step Size#Natural Policy Gradients|natural policy gradients]] also fit into this formulation.

> [!idea] (Philosophical waffles)
> A gradient is a *dual vector*, and the only thing you are allowed to do with a dual vector is to hit it with a normal vector (i.e. a weight) to get a single real number; this is the linearization of $\mathcal{L}$. Therefore, the standard picture of gradient descent where you *add* the gradient to the weight is not a natural procedure (wrong types!); instead, you need this duality map to convert dual vectors back to the primal space.

So, you shouldn't do the procedure `weight -= LR * gradient`, unless you are sure your space is Euclidean. Instead, you want to do `weight -= LR * dualize(gradient)`.

Can we propose a modular way of constructing a duality map?

1. For each module, we can solve the duality map using the norm:
$$
\text{dualize}(G)=\arg\max_{\| A \| =1}\langle A, G\rangle.
$$
2. Then, there is some way of combining these to dualize the full network.

 Intuitively, we are squeezing as much juice out of the first-order effects as we can, without the second-order effects dominating (since the norm is bounded and we have Lipschitzness).

One interesting case study is the [Shampoo optimizer](https://proceedings.mlr.press/v80/gupta18a/gupta18a.pdf). The core primitive in this algorithm is the funny gradient transformation
$$
\Delta \mathbf{w}=-\eta \cdot (G G^{T})^{-1/4} G (G^{T} G)^{-1 / 4}.
$$
Well, this optimizer can also be framed in terms of steepest descent under some norm. This can be easily implemented in `modula` by just overriding the `normalize` function of the linear layer.

This is actually an issue with the `modula` package right now: it should say `dualize` instead of `normalize`.

---

**Next:** [[Inference Methods for Deep Learning]]