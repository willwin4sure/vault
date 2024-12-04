> Ilya: Deep nets generalize because they find small circuits that fit the data.
> 
> Isola: *Small* circuits? But deep nets are big! Don't we need something else to bias toward small circuits?
> 
> Ilya: No. Deep nets are finite; that is enough. Anything finite will look small once you have enough data.
> 
> — Phillip Isola's recollection of a conversation with Ilya Sutskever.

\This class is on why neural networks generalize. After all, deep nets violate certain classical generalization theory.

> [!definition] (Training and test errors)
> So far, we've just discussed approximation, which aims to minimize the ==empirical risk==
> $$
> \hat{\mathcal{R}}(\theta)=\frac{1}{N}\sum_{i=1}^{N}\mathcal{L}(f_{\theta}(\mathbf{x}_{i}),\mathbf{y}_{i}).
> $$
> However, we actually want our networks to minimize ==population risk==
> $$
> \mathcal{R}(\theta)=\mathbb{E}_{(\mathbf{x},\mathbf{y})\sim \mathcal{P}}\mathcal{L}(f_{\theta}(\mathbf{x}),\mathbf{y}).
> $$

The former is the training error while the latter is the test error. Of course, instead of the full expectation you could just have a hold-out test set.

The problem of generalization is whether minimizing empirical risk leads to low population risk. What leads to bad generalization?

## Bad Data

Obviously if you're trying to train a cat dog classifier and your training data only includes cats, you're not going to have a good time. As the old adage goes, garbage in, garbage out.

## Bad Models

> [!example] (The filing cabinet)
> This terrible model just memorizes its inputs: store all training data points in a mapping $x \mapsto y$. At inference, if we see a repeated $x$, output the corresponding $y$. Otherwise, output $0$.

^c653d0

The approximation error will be zero, since we interpolate the training set perfectly. But the generalization error will (probably) be huge!

An actual MLP generalizes much better in the gaps between the training points:

![[filing_v_mlp.png|center|512]]

> [!question]
> Are modern LLMs filing cabinets?

This is actually a common gripe about modern language models, that they are just memorizing things that are in-distribution, without capacity of generating new knowledge.

LLMs aren't just performing *pure* memorization, though. You can do this with simple combinatorics, e.g. asking it to select the number of citrus fruits from a list, and considering all permutations of that list. It can always get the answer right, despite the huge number of possible inputs.

> [!example] (Pix2pix is capable of generalizing)
> Here's an example of generalization with convolutional neural networks. One of the trained models in pix2pix is able to convert outlines of cats into filled-in versions. However, it is also able to generalize beyond to arbitrary shapes:
> 
> ![[cat_generalization.png|center|512]]
> 
> You can also produce some atrocities:
> 
> ![[many_eye_cat.png|center|512]]
> 
> This generalization is enabled by the inductive biases of CNNs, e.g. invariance to spatial translation. For example, it seems to have learned that whenever it sees an oval, it should draw an eye.

## Generalization Theory

### Classical Ideas

Classical generalization theory follows the adage of Occam:

> [!idea] (Occam's razor)
> The *simplest* model that *fits the data* will generalize best.

^0e38a5

> [!question]
> How do we measure *simplest*?

> [!example] (Kolmogorov complexity)
> One could define "simplest" as the shortest program that can generate the dataset, i.e. how to maximally compress it. This is completely intractable to compute, however.

In machine learning, we will restrict to settings of the parameters of a chosen network architecture. The more expressive our model architecture is, the higher capacity our hypothesis class $\mathcal{H}$ will have.

The classical picture of overfitting looks like this:

![[classical_overfitting.png|center|384]]

The claim here is that as your model becomes larger, you eventually interpolate the training data but are overfit to its particular characteristics (e.g. noise).

### Double Descent

Here's a picture of overfitting in polynomial regression. When we have the "correct" degree of the polynomial $d=3$, the fit looks like the following:

![[3poly.png|center|384]]

If we increase $d$, however, it starts to overfit on the noise in the dataset:

![[20poly.png|center|384]]

What do you think will happen when $d=1000$? Well, it turns out that the fit function is far more reasonable!

![[1000poly.png|center|384]]

Now, the model appears to be a sum of a "simple" predictive component and a "spiky" memorization component.

Why do we end up at such a model? Well, there are many possible degree $1000$ polynomials that fit this data. The reason we end up at a decent one is due to the other inductive biases from the training procedure, e.g. the choice of optimizer.

Here is a picture of this so-called ==double-descent phenomenon==.

![[double_descent.png|center|512]]

The reason we have a peak at the interpolation threshold is because the model has just enough capacity to fit the data, so it is *forced* to fit to the noise, and there are few choices that do so. However, as the model becomes more expressive, there are more options and the optimizer can do its good work.

Here is a picture of it actually happening in practice:

![[mnist_dd.png|center|512]]

> [!warning]
> Replicating this result is nontrivial. It only shows up in certain regimes and details such as momentum or weight decay may cause it to disappear.
> 
> These extra tricks from modern ML may reduce the negative effect of the spike in the middle.

One reason double descent may occur: as we increase the number of features, the norm of the learned weights first increases and then peaks at the interpolation threshold (since you are forced to use all of them). However, as we keep increasing the feature count, the norm begins to drop back down.

![[norm_features.png|center|512]]

## Measuring Complexity

This suggests that counting the number of parameters might not be the right way to measure model complexity. Double descent implies that it doesn't conform with the classical theory that higher complexity models are worse.

Parameter norm may be better (based on the graph above), but it has its own problems.

### Vapnik-Chervonenkis Theory

> [!idea]
> Measure complexity based on the number of distinct functions the model can represent.

This is an old theory. The claim is that if the size of the training set dwarfs the number of functions in the function class, then the generalization error is small with high probability, since false positives are unlikely.

> [!warning]
> Turns out, this is not a useful idea in practice. The issue is that neural networks can actually fit random labels, which means that the VC dimension is huge and the generalization bound is useless.

### What Else?

Measuring complexity is an open problem in deep learning!

## Ideas on Why Deep Learning Works

Here are some more recently proposed inductive biases that support deep learning.
### Simplicity Bias

It turns out that if you just *randomly* sample some parameters for your neural network, most of these setting will correspond to simple functions.

![[simplicity_bias.png|center|384]]

### Low-Rank Bias of Depth

Products of many weight matrices tend to be low rank. Low rank embeddings have better clustered structure.

### Implicit Regularization of Optimizers

Some regularization is explicit, e.g. weight decay or initialization near zero. Others include the fact that SGD tends to converge to "flat" minima (they will bounce out of sharp ones).

### Architectural Symmetries

> [!idea]
> These are probably the most important.

Invariances, e.g. a max pool of various edge detectors in a CNN will fire regardless of the orientation of some object.

Equivariances, e.g. translation commutes with CNN filters, permutation of nodes commutes with GNN aggregations.

Compositionality, e.g. the signal can often be factored into components that are stitched together.

## Summary

1. Deep nets generalize. This is an empirical fact, though we're not sure why.
2. Generalization requires *inductive biases*. We can't just arbitrarily fit the training data, we need specifications like model architecture and optimizers.
3. These inductive biases can't be characterized via classical notions of complexity (e.g. number of parameters, VC dimension).
4. We have some modern ideas on why deep networks work.

---

**Next:** [[Scaling Rules for Optimization]]