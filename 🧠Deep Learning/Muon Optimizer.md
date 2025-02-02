Recall from lecture we discussed [[Scaling Rules for Optimization#^8409b1|steepest descent]] under norms, and that the update steps of the [[Scaling Rules for Optimization#^386145|Adam optimizer]] without momentum are in the same direction as steepest descent under the $L^{\infty}$ norm.

> [!question]
> Show that the solution to steepest descent under the spectral norm, i.e.
> $$
> \arg\min_{\Delta W}\operatorname{Tr}(G^{T}\Delta W) + \frac{\lambda}{2}\| \Delta W \|_{*}^{2}
> $$
> is equal to
> $$
> \Delta W=-\frac{\operatorname{Tr}(\Sigma)}{\lambda}UV^{T},
> $$
> where the SVD is given by $G=U\Sigma V^{T}$.

You can think of this roughly as taking $G$ and setting its singular values to one, i.e. the [matrix sign function](https://en.wikipedia.org/wiki/Matrix_sign_function)?

In fact, this is the principle behind the Muon optimizer, shown below. It also adds Nesterov momentum, uses low precision, and runs the iterative Newton-Schulz method rather than performing a slow singular value decomposition.

![[muon_optimizer.png|center|512]]

This is currently the best method for quickly training a nanoGPT model; see [Keller Jordan's repository](https://github.com/KellerJordan/modded-nanogpt).

