Last, we discussed representations via embeddings and clusters. Today, we'll finish off with learning metrics.

> [!idea] (Complexity measures that accurate predict generalization)
> Three winning strategies look at:
> 
> * Geometry of the representation: consistency and separation.
> * Robustness to perturbations.

> [!example] (CIFAR-10 on true v.s. random labels)
> Recall that you can train a neural network to fit to random labels. However, the types of representations you end up with look qualitatively different!
> 
> ![[cifar_clean_vs_random.png|center|512]]
> 
> Notice how the clean labels have much tighter clusters that are well-separated!

So if your representation looks like the one on the left, you can expect it to generalize a lot better than if it looks like the one on the right.

> [!claim] (Properties of good representations)
> Continuing on from [[Representation Learning I#^b2bd9a|before]],
> 
> 1. Concentration (data from the same class is close)
> 2. Separation (different classes are far)
> 3. Robustness to irrelevant perturbation

## Metric Learning

One way of trying to encourage good representations is via feedback in terms of similarity, i.e. use pairs of similar/dissimilar inputs.

> [!example] (Metric learning)
> Basic Euclidean distance in your input space is probably quite bad. Maybe you want to learn some projection that puts similar points near each other and dissimilar points far away from each other.
> 
> So assume you have a dataset $\mathcal{S}$ of similar pairs and $\mathcal{D}$ of dissimilar pairs, and you want to learn a linear transformation $\mathbf{x}\mapsto \mathbf{W}\mathbf{x}$ that respects this similarity. The new distance, then, is
> $$
> \| \mathbf{W}\mathbf{x}_{i} - \mathbf{W}\mathbf{x}_{j} \|_{2}^{2} = \| \mathbf{x}_{i}-\mathbf{x}_{j} \|_{\mathbf{A}}^{2},
> $$
> where $\mathbf{A}=\mathbf{W}^{T}\mathbf{W}$, the ==Mahalanobis distance==.
> 
> One basic approach is to solve the constrained optimization
> $$
> \min_{\mathbf{A}\succeq 0}\sum_{(i,j)\in \mathcal{S}}^{}d_{\mathbf{A}}(\mathbf{x}_{i},\mathbf{x}_{j})^{2},\quad \text{where} \sum_{(k,\ell)\in \mathcal{D}}^{}d_{\mathbf{A}}(\mathbf{x}_{k},\mathbf{x}_{\ell})^{2}\geq 1.
> $$

There are more improvements:

> [!example] (Nonlinear transformations)
> For example, replace your linear transformation with some neural network $f$.

> [!example] (Contrastive losses)
> Maybe it is hard to evaluate how similar A and B are... what is the scale? However, it might be easier to say that B is *more similar* to A than C is. This gives rise to the ==triplet loss==
> $$
> \mathcal{L}_{\text{triplet}}(\mathbf{x},\mathbf{x}^{+},\mathbf{x}^{-})=\sum_{\mathbf{x} \in \mathcal{X}}^{}\max\left( 0,\| f(\mathbf{x})-f(\mathbf{x}^{+}) \|_{2}^{2} - \| f(\mathbf{x})-f(\mathbf{x}^{-}) \|_{2}^{2} + \varepsilon \right),
> $$
> i.e. we are unhappy if the similar point is not at least $\varepsilon$ closer than the dissimilar point. You can improve this to using multiple negative examples per positive pair via ==lifted structured loss==
> $$
> \mathcal{L}_{\text{struct}}=\frac{1}{2|\mathcal{P}|}\sum_{(i,j)\in \mathcal{P}}^{}\max\left( 0,\mathcal{L}_{\text{struct}}^{(i,j)} \right)^{2},
> $$
> where
> $$
> \mathcal{L}_{\text{struct}}^{(i,j)}=D_{ij}+\max\left( \max_{(i,k)\in \mathcal{N}}\varepsilon-D_{ik}, \max_{(j,\ell)\in \mathcal{N}}\varepsilon - D_{j\ell} \right).
> $$

Note that these loss functions give no signal if we don't give it "hard" negatives, i.e. ones that are currently misplaced by the model. ==Negative mining==, the search for these examples, accelerates learning. 

> [!example] (Normalization of representations)
> Can map onto a hypersphere. Then you can think about angles instead of Euclidean distance. This is nice because of more stable training, and clusters are always linearly separable.

## Self-Supervised Contrastive Learning

You have an encoder that maps onto the hypersphere $f:\mathcal{X}\to \mathbb{S}^{d-1}$. You want to push negative pairs apart and pull positive pairs together. We can do this via the loss function

$$
\min_{f}\mathbb{E}_{(\mathbf{x},\mathbf{x}^{+})\sim p_{\text{pos}}, \{ \mathbf{x}_{i}^{-} \}_{i=1}^{N}\sim p_{\text{data}}}\left[ -\log \frac{e^{f(\mathbf{x})^{T}f(\mathbf{x}^{+})/\tau}}{e^{f(\mathbf{x})^{T}f(\mathbf{x}^{+})/\tau}+\sum_{i=1}^{N}e^{f(\mathbf{x})^{T}f(\mathbf{x}_{i}^{-})/\tau}} \right].
$$
However, now we don't have labeled pairs anymore. For images, we can uniformly sample the rest of the data for negative pairs, and perform random augmentations to the current image for positive pairs.

> [!warning]
> These random augmentations are domain-specific. There are other ways to get around this, e.g. using views of the data. However, the basic idea is we want the representation to be *invariant to irrelevant perturbations*.

One challenge is that uniformly sampling might hit an image that's actually similar, which results in a bad signal. Explicitly removing these can help improve the representation.

Other variations of self-supervision include using multiple channels of the data, e.g. a color v.s. a depth field of an image. You can also use image-caption data like CLIP: one side is the image and the other is the caption.

> [!idea]
> CLIP uses a contrastive loss!

The contrastive loss we showed early actually just tries to maximize [[Information Measures#^cd28ea|mutual information]] between the two "views" $\mathbf{x}$ and $\mathbf{x}^{+}$ of the same thing.

However, it turns out that if you only try to maximize mutual information alone, then you get worse representations. So the contrastive loss is doing something extra. 

> [!idea]
> A good representation has *alignment* (similar samples have similar features) and *uniformity* (preserve maximal information).

---

**Next:** [[Architectural Bias on Representations]]