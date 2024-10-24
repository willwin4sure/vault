There are three main puzzle pieces to machine learning: ^c04c40

1. **Approximation**: does there *exist* a neural network in my model family that fits the training data?
2. **Optimization**: if it exists, can I *find* it?
3. **Generalization**: does it *work well* on *unseen* data?

In this lecture, we are only concerned with the first problem on approximation.

## Family of Curves

These are the types of functions we hope to be able to approximate. We rule out anything too pathological.

> [!definition] (Lipschitz continuous functions)
> A function $g:\mathbb{R}\to \mathbb{R}$ is ==$L$-Lipschitz== if
> $$
> |g(x+\Delta x)-g(x)|\leq L|\Delta x|
> $$
> for all inputs $x \in \mathbb{R}$ and all $\Delta x \in \mathbb{R}$.

^bb2604

> [!idea]
> Intuitively, this says that the slope of $g$ cannot exceed $L$.

To extend to $d$-dimensional functions $g:\mathbb{R}^{d}\to \mathbb{R}$, we instead restrict
$$
|g(x+\Delta x)-g(x)|\leq L\| \Delta x \|_{\text{RMS}},
$$
where the RMS norm is $\frac{1}{\sqrt{ d }}$ times the Euclidean norm. ^45dea4

## Main Theorem

Now we will prove a theorem in universal approximation.

> [!theorem] (Lipschitz functions are approximated by 3-layer NNs)
> Let $g:[0,1]^{d}\to \mathbb{R}$ be any $L$-Lipschitz function. Then, for any error $\varepsilon>0$, there exists a 3-layer ReLU network with $N=4d\left( \frac{L}{\varepsilon} \right)^{d}$ units such that
> $$
> \int_{[0,1]^{d}} | f(x) - g(x) | \, dx < 2\varepsilon.
> $$

These classes of results are pretty stupid though. Firstly, the number of hidden units grows exponentially in the input dimension and also increases if we want greater accuracy. Also, it's not of much practical use: we generally want to impose more inductive bias on the architecture of neural networks so that we can find generalizable functions (the optimizer also plays a role in this).

## Proof

> [!idea]
> The main strategy:
> 
> 1. Approximate the function with $N$ cuboids.
> 2. Show that ReLU networks can approximate cuboids.

The first step works out since $g$ is $L$-Lipschitz. If there are $N$ cuboids partitioning the $\mathbb{R}^{d}$ input space, this gives a side length of $\frac{1}{N^{1/d}}$, so the total error is at most
$$
N \cdot \frac{L}{N^{1/d}} \cdot \frac{1}{N} = L / N^{1/d},
$$
where the first term is the number of cuboids, and the second term are the maximum height of the error and the hypervolume of the base, per cuboid. So to get an error of $\varepsilon$ we need $\left( \frac{L}{\varepsilon} \right)^{d}$ cuboids.

Now we can build these cuboid indicator functions with a two-layer ReLU network. Then simply sum together $N$ of these using the third layer.

In particular, in a single dimension, we can build a rectangle by summing four ReLU functions (like building a spread out of calls), and then these can be summed together, one per dimension, and thresholded to only include the point where they all intersect. A linear combination of these cuboids lets you approximate any Lipschitz surface.

## Comments

This construction where we build a sum of indicator functions is not very natural. If we tried to train such a representation directly, we'd just massively overfit on the training data!

Two layers are actually enough to universally approximate functions!

---

**Next:** [[Width or Depth]]