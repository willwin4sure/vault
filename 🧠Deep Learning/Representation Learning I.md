Here's our picture of deep learning for the next few lectures:

![[rep_learning_and_gen_modeling.png|center|256]]

We will have data that we *embed* into a latent space via representation learning. We can then send similar points in this latent space backwards to *generate* more data.

## x2vec

The most famous version of this is called ==word2vec==, which mapped words into vectors in a way that respected semantic similarity. You can do this for all sorts of data "x" though, e.g. images, sounds, or molecules.

Here is a picture representing this process. The hope is that the representation space is relatively simple, e.g. a Gaussian. By contrast, the data space can be some complicated high-dimensional manifold.

![[x2vec.png|center|512]]

Here are some pictures of standard functions present in neural networks. 

![[network_functions.png|center|512]]

We can use this to visualize what happens to a MLP that is trained to separate the blue and red points below:

![[mlp_training.png|center|768]]

We can do this for more complicated neural networks too, e.g. Vision Transformers. This uses PCA for dimensionality reduction to visualize the data points, and colors based on the classes in the classification problem. Notice that the network is able to disentangle the different classes of data despite them being jumbled initially.

![[clip_rep.png|center|512]]

The idea is that each of these layers are various *representations* that gradually get simpler. 

## Visualizing and Understanding CNNs

In the human brain, we have many layers to our visual system. They start with simple pattern recognition and gradually respond to more complex shapes.

We can do a similar study on CNNs, by seeing what patterns various filters are activated the most by. In the first layer, we'll see patterns like the following:

![[cnn_layer_1.png|center|384]]

These seem to be feature detectors, e.g. edge detectors or color detectors. Here are patches for the second layer:

![[cnn_layer_2.png|center|384]]

Now we get more complex patterns. By layer 3, we get recognizable objects.

![[cnn_layer_3.png|center|512]]

By layer 5, there are clearly precise filters for, e.g. dog or human faces.

![[cnn_layer_5.png|center|384]]

> [!warning]
> One main difference is that the brain only has around 7 layers, while modern deep nets have hundreds of layers. However, the brain has recurrence so it is able to reuse parts.

## Representations

> [!definition] (Representation)
> A ==representation== is a vector embedding.
> 
> * A representation of a data domain $\mathcal{X}$ is a function $f:\mathcal{X}\to \mathbb{R}^{d}$ which assigns a feature vector for each input. This is called an encoder.
> * A representation of a datapoint $\mathbf{x} \in \mathcal{X}$ is a vector $\mathbf{z}\in \mathbb{R}^{d}$ with $\mathbf{z}=f(\mathbf{x})$.

> Generally speaking, a good representation is one that makes a subsequent learning task easier.

> [!idea] (Deep networks are not blank slates)
> A recent shift in the deep learning paradigm within the last decade is not approaching new problems in deep learning as blank slates. Doing so often led to data hungry processes, but now we are much more comfortable with applying pre-trained representations or models.

For example, suppose you already have a pre-trained encoder for music data. Then, you can train various prediction heads for different tasks such as genre recognition or preference prediction. You could also fine tune the pre-trained encoder if necessary.

> [!idea] (Modern deep net workflow)
> Pretrain a large model on a *lot of data*, while being relatively agnostic to the final use case. Then fine-tune on a *little data* for some particular task, and test it.

The trick here is that the modern method for *learning from little data* (something long desired) is to first pretrain on massive data.

> [!claim] (Properties of good representations)
> The first two are reminiscent of [[Neural Network Generalization#^0e38a5|Occam's razor]]. 
> 1. Compact (*minimal*)
> 2. Explanatory (*sufficient*)
> 3. Disentangled (*independent factors*)
> 4. Interpretable
> 5. Make subsequent problem solving easy

^b2bd9a

## Unsupervised Learning

Suppose we just have inputs $x^{(i)}$ without any targets. Can we still learn good representations?

Some of these include:

1. Embeddings, as we've been discussing.
2. Clusters.
3. Metrics.

We will discuss metrics in the following lectures.

> [!idea] (Compression or prediction)
> How do we ensure our representations are good? One way is to target *compression*, i.e. make sure that you can reconstruct the initial data. Another is to allow *prediction*, i.e. determining missing data.

> [!example] (Autoencoders for embeddings)
> This is the basic method you learned in intro ML:
> 
> ![[autoencoder.png|center|512]]
> 
> You push it through a neural network with lower and lower dimensional vectors in order to get your compressed embedding. However, you also need the representation to be sufficient so you simultaneously train a decoder network to ensure you can reconstruct the data. Here is another image:
> 
> ![[autoencoder_training.png|center|512]]
> 
> In vanilla autoencoders, there's no guarantee that the representation space is simple, though we can force it to be low-dimensional. However, variational autoencoders can do so.

^550b04

The key reason that autoencoders work is that the data is forced to be squeezed through a low-dimensional space. You can also enforce other forms of simplicity on the data.

> [!claim]
> If both $f$ and $g$ are constrained to be linear, then the learned autoencoder embeddings spans the same subspace as PCA. 

Note that the linear autoencoder problem is
$$
\min_{W_{f},W_{g}}\| XW_{f}W_{g}-X \|_{F}^{2},
$$
while the PCA problem is
$$
\min_{W : W^{T}W=I}\| XWW^{T}-X \|_{F}^{2}\quad \Longleftrightarrow \quad \max_{W : W^{T}W=I}\| XWW^{T} \|_{F}^{2}.
$$
In this lens, autoencoders are just a nonlinear generalization of PCA.

> [!example] (Autoencoder usefulness)
> Here's a basic experiment on the usefulness of these autoencoder embeddings.
> 
> ![[autoencoder_power.png|center|512]]
> 
> For example, we can train an autoencoder on various images of colored shapes. Then, we perform a nearest neighbor probe on new queries and notice that nearby embeddings do have similar colors/shapes.
> 
> We can also use it for a 1NN classifier. Then, note that the accuracy for the shape gets better while the accuracy for the color deteriorates as the network gets deeper. This may be because color is easy for shallow networks to learn (its superficial from pixel space), while shape is more challenging.

> [!example] ($k$-means for clustering)
> This trains an encoder $f:\mathcal{X}\to \{ 1,2,\dots,k \}$ which clusters the data points. $k$-means does this by minimizing the distance from points to the means of their assigned clusters. Note that the mean of any cluster is precisely the point that minimizes the sum of squares of distances to the points in the cluster.
> 
> From this perspective, $k$-means is the same as an autoencoder, but $\mathcal{X}$ has to be squeezed through the finite set $\{ 1,2,\dots,k \}$ and back out into $\mathbb{R}^{N}$, where the loss is just the sum of square distances between inputs and outputs.

This can be computed using block coordinate descent (e.g. [[The Expectation Maximization (EM) Algorithm|EM algorithm]]).

> [!example] (Self-supervised learning for prediction)
> You can take some data, run it through an encoder-decoder architecture, and set your target as some other part of the data. This gives you an optimization objective to train your encoder on.
> 
> For example, the input could be the grayscale version of an image, while the output could be the color information. Then, the representation detects objects well (since learning objects is very valuable for predicting colors)!

Some other options are future frame prediction or next pixel prediction. For example, language models are self-supervised by predicting the next token in a sequence.

> [!example] (Masked autoencoder)
> This chunks the image input into pieces, masks some of them out, and the rest are fed through an encoder model and a decoder model that is trained to reconstruct the full image. Conceptually, this is similar to how BERT is trained.

> [!claim]
> Masked prediction often performs better than autoencoding!

Some hypotheses as to why:

1. It's hard to control compression via a dimensional bottleneck (low dimensions have bad properties!). It's nicer to use the mutual information between the masked input and the full output.
2. Autoencoders are learning shortcuts where they can copy part of the input and get a decent loss. They can fall into these local minima traps.
3. Masked prediction is closer to the downstream problems we care about, which are mainly about prediction.

---

**Next:** [[Representation Learning II]]
