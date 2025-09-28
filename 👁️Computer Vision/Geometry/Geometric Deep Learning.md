> Building neural networks that are equivariant to group actions. A modern research direction is building robotics diffusion policies that are robust to 3D roto-translations of the camera in generation of the 2D images (this is not simply a group action on the 2D image anymore, but a group action on something generating the image!).

## Equivariance of CNNs

> [!definition] (Equivariance)
> ==Equivariance== is a property of a function $\Phi:X \to Y$ (e.g. a neural network layer) by which it commutes with a group action.

For example, CNNs (with the correct details) are translation equivariant. This is because the following diagram commutes:

![[cnn_translation_equivariance.png|center|256]]

Note that this is different than ==invariance==, which would mean that the function $\Phi$ is insensitive to any group operation applied on the input.

However, traditional CNNs are not rotationally equivariant! How can we design a network that is?

## Rotationally-Equivariant CNN

> Note, nobody actually uses these. But they are cool in principle.

The problem is that the kernel doesn't rotate together with the input. The solution, then, is to create copies of the filter across rotations, just like how we have copies of the filter across translations.

### Lifting Convolution

This is the first layer of the neural network, which takes in a 2D feature map and uses a 2D kernel.

![[se2_equivariant_cross_correlation.png|center|512]]

Now, given some 2D filter on a 2D feature map, we will actually produce a 3D feature map (what is shown is after ReLU) where each $xy$-layer corresponds to convolving the feature map using the kernel rotated by $\theta$.

This is $\text{SE}(2)$-equivariant in the sense that:

* Any toroidal translation on the 2D feature map results in the same translation in the 3D feature map for each $xy$-plane.
* Any rotation on the 2D feature map results in the same rotation in the 3D feature map for each $xy$-plane, in addition to some circular translation along the $\theta$ dimension (you have to rotate the filter too!).

> [!idea]
> So the action of $\text{SE}(2)$ on the 3D feature map is a bit complicated: translations do what you think, but rotations have an additional shift along $\theta$ component, twisting like a helix.

Your image sitting in $L^{2}(\mathbb{R}^{2})$ has been lifted to an element of $L^{2}(\text{SE}(2))$. Of course, like in a normal CNN, you'd have many of these filters running in parallel across channels.

### Convolutions in $\text{SE}(2)$

The subsequent layers in the neural network have to work with this 3D feature map. Your kernel map will also live in $L^{2}(\text{SE}(2))$, though it will be smaller in the $xy$-plane (the $\theta$ will have the same length though).

![[se2_g_convolution.png|center|512]]

Now you will just act on the filter using elements of $\text{SE}(2)$ and take the result's dot product with the input 3D feature map. This function (from the action you apply on the filter to the dot product) is precisely the output 3D feature map.

> [!idea]
> This is entirely analogous to what happens with a normal CNN: to get the output value for any translation element, you act on the filter with that element and dot product it with the input.

Now you can check that this convolution is still $\text{SE}(2)$-equivariant. It is obvious for translations; for rotations it is true since both the filter and feature map get acted upon in the same way.

> [!warning]
> In practice, you have to discretize the $\theta$ values as well. Note that these operations are way more expensive if you have a lot of rotations!

### Final Layer

You can do all sorts of things depending on what you want. For example, you can take the mean over rotations to be invariant to that.

### Why These Aren't Used

> [!warning]
> The cost grows linearly with the discretization of the group and combinatorially with the number of dimensions (imagine you wanted to not just be translation and rotation equivariant, but also scale equivariant!).

So nobody uses these in practice. But they still [show up naturally](https://distill.pub/2020/circuits/equivariance/) if you do data augmentation to force rotation equivariance. The way the CNN learns this is by creating many filters that are all rotations of each other.

## Practical Equivariant Networks with Steerable Bases

Write filters as linear combinations of filters that are radial basis functions multiplied with circular harmonics (which are steerable). This way you don't need to compute convolutions with every rotation of the filter, just the basis ones.

The regular way above is related to this method via a Fourier transform. This is because its a mapping of coefficients of basis vectors to the actual function on rotations.

There is some extra constraint on the filters for the future layers (when expressed as coefficients on the steerable basis) which has been derived for you. Look into [escnn](https://github.com/QUVA-Lab/escnn) if you want to use these!

## Applications in 3D Computer Vision

You often see this used in 3D computer vision, e.g. given the problem of reconstructing an object from a view of its point cloud. And maybe you want to be invariant to the perspective you are given of the point cloud. 

In 2D computer vision, you can just use data augmentation instead of equivariant neural networks, though the latter are cheaper.

One open problem is working on 3D geometry in 2D video. The issue is that 3D symmetry groups don't act directly on 2D images, and getting neural networks to be provably robust is an open problem.

---

**Next:** [[Optical Flow]]