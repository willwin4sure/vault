This lecture gives some deeper insight into what's going on with the Fourier transform. We've seen that circulant matrices (and therefore convolutions) are diagonalized by the Fourier transform, which gives rise to the convolution theorem. What's going on? Can we find analogous bases for other group operations?

For some deep learning motivation—CNNs are translation equivariant, but not usually rotation equivariant. Are there analogous bases for rotations in addition instead of shifts?

## Math Background

A bit of review. You know what a ==group== is.

> [!example] (Groups in computer vision)
> The ==translation group== $\mathbb{R}^{2}$ and the ==roto-translation group== $\text{SE}(2)$.

A ==representation== is some group homomorphism $\rho:G \to \text{GL}(V)$ which materializes the abstract elements of the group as linear operators in some vector space. The operators must respect the group operation accordingly.

> [!definition] (Dual representation)
> If $V$ is a representation of a group $G$, its ==dual representation== $V^{*}$ is given by taking any linear functional $\lambda \in V^{^{*}}$ to the linear functional given by $(g\lambda)v=\lambda(g^{-1}v)$.


