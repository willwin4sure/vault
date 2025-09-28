This lecture gives some deeper insight into what's going on with the Fourier transform. We've seen that circulant matrices (and therefore convolutions) are diagonalized by the Fourier transform, which gives rise to the convolution theorem. What's going on? Can we find analogous bases for other group operations?

For some deep learning motivation—CNNs are translation equivariant, but not usually rotation equivariant. Are there analogous bases for rotations in addition instead of shifts?

## Math Background

A bit of review. You know what a ==group== is.

> [!example] (Groups in computer vision)
> The ==translation group== $\mathbb{R}^{2}$ and the ==roto-translation group== $\text{SE}(2)$.

A ==representation== is some group homomorphism $\rho:G \to \text{GL}(V)$ which materializes the abstract elements of the group as linear operators in some vector space. The operators must respect the group operation accordingly.

> [!definition] (Dual representation)
> If $V$ is a representation of a group $G$, its ==dual representation== $V^{*}$ is given by taking any linear functional $\lambda \in V^{^{*}}$ to the linear functional given by $(g\lambda)v=\lambda(g^{-1}v)$.

You need the inverse for things to commute properly, e.g. $((gh)\lambda)v=\lambda((gh)^{-1}v)$. It's the slightly counterintuitive fact you learn in middle school, that to shift a function $f(x)$ to the *right* by $c$, you need to write $f(x-c)$.

We can slightly generalize this idea to where $f$ has generic domain $X$ that $G$ acts upon. Then, if $f\in L^{2}(X)$, we can write $\rho(g)f(x)=f(g^{-1}x)$ for a representation $\rho:G\to \text{GL}(L^{2}(X))$.

## Steerable Bases

> [!definition] (Steerable basis)
> A vector $Y(x)=\begin{bmatrix}\vdots\\ Y_{\ell}(x)\\ \vdots\end{bmatrix}\in K^{L}$ with basis functions $Y_{\ell}\in L^{2}(X)$ is ==steerable== if for all $g\in G$,
> $$
> Y(gx)=\rho(g)Y(x),
> $$
> where $gx$ denotes the action of $G$ on $X$ and $\rho(g)\in K^{L\times L}$ is a representation of $G$.

In other words, you can transform all basis functions (the left hand side) by simply taking a linear combination of the original basis functions (the right hand side).

> [!example] ($\sin$ and $\cos$ form a steerable basis)
> Suppose $G=\mathbb{R}$ acts on $X=\mathbb{R}$ via translation. Then, $Y(x)=\begin{bmatrix}\cos x\\ \sin x\end{bmatrix}$ forms a steerable basis via the representation $\rho(\theta)=\begin{bmatrix}\cos\theta & -\sin \theta\\ \sin\theta & \cos \theta\end{bmatrix}$.

This is the fact that if you phase shift a sine wave, you can reconstruct it by simply taking a linear combination of zero phase sine and cosine waves.

> [!claim] 
> The Fourier basis is steerable.

This is a consequence of the fact that sine and cosine are steerable.

> [!idea]
> Can we construct other steerable filters?

[For sure!](https://people.csail.mit.edu/billf/publications/Design_and_Use_of_Steerable_Filters.pdf) For example, you might want some steerable edge detectors that allow you to detect edges of any orientation by only taking two convolutions (here, $G=S^{1}$). Here's an example:

![[steerable_gaussians.png|center|512]]

Normally, if you want to detect edges of all kinds, you might have to rotate your filter and recompute a full convolution with the image, which is expensive.

The steerability property, together with the linearity of convolution, allows you to instead just compute the convolution with two base filters, and then take some linear combination of the results to get the convolution with any rotated filter.

The construction here is just the partial derivative of a 2D Gaussian, e.g.
$$
\frac{ \partial }{ \partial x } e^{-(x^{2}+y^{2})} = -2x e^{-(x^{2}+y^{2})}.
$$
The property holds due to the rotational symmetry of the second term and the fact that single $x$ comes out the front.

## Finding Steerable Bases

Here's a recipe:

1. Find a *linear operator* such that:
	1. It *commutes* with the group action. This means its eigenspaces are unchanged under the group action.
	2. Its eigenvectors are guaranteed to be a complete basis for the function space. For example, it can just be self-adjoint (Hermitian) via spectral theorem.
2. Compute its eigendecomposition.
3. The eigenfunctions must form a steerable basis for the group transformation!

For the first step, this is true because if $L$ is an operator that commutes with a group transformation $T$, then
$$
LT\phi=\lambda T\phi
$$
for any eigenfunction $\phi$ of $L$ with eigenvalue $\lambda$, meaning $T\phi$ is also in the $\lambda$-eigenspace. If $L$ is self-adjoint then by the spectral theorem the eigenfunctions form a basis! And it is steerable.

> [!idea] ("Deriving" the Fourier transform)
> If you consider any self-adjoint operator that commutes with translations (e.g. convolution with any symmetric kernel), then its eigenspectrum will reveal the Fourier basis!

![[toeplitz_eigenfunctions.png|center|512]]

## Spherical Harmonics

Can we find a steerable basis for functions on a sphere $S^{2}$, where the group action is rotations in $\text{SO}(3)$?

We can apply the formula above on the Laplace-Beltrami operator, whose eigendecomposition gives us the ==spherical harmonics==.

![[spherical_harmonics.png|center|512]]

These are rotation-steerable, though often people will use this basis without utilizing that property. They're just a pretty natural basis for spherical functions in general.

> [!claim]
> In fact, the spherical harmonics correspond to electron orbitals in chemistry! Probably some mathematical reason. You can recognize the s, p, and d orbitals as the first three rows.

One application of these features in [PlenOctrees](https://alexyu.net/plenoctrees/), which are like NeRFs but remove the viewing angle input to the neural network, by just having the network output linear combinations of spherical harmonics instead. Note that they aren't using the rotation steerability property here.

There is some similar work on shape-to-shape correspondence with "functional maps".

---

**Next:** [[Geometric Deep Learning]]

