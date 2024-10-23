This class is on why neural networks generalize. After all, deep nets violate certain classical generalization theory.

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

Classical generalization theory follows the adage of Occam:

> [!idea] (Occam's razor)
> The simplest model that fits the data will generalize best.

