> This is the architecture you should use today! It will get you jobs.

> [!idea] (Some motivation from CNN limitations)
> CNNs operate locally which means that far apart image patches don't tend to interact. The receptive field is bounded by the size of the filter and the depth.

The ==attention mechanism== allows far away patches to attend to each other, but sparsely (unlike a fully connected MLP).

## Tokens

> [!definition] (Tokens)
> A ==token== is a vector of neurons. In the [[Architectures for Graphs|GNN lecture]], we called these "node attributes". The idea is that these are an encapsulated bundle of information.

Of course, the initial motivation came from [[💬Natural Language Processing Portal|NLP]], where tokens are the atomic units that make up words.

The transformer architecture is relatively agnostic to the input signal: all we need is to be able to convert it into a set of tokens.

> [!example] (Images can be tokenized)
> The standard way is to chop the image up into patches and project each into a $d$-dimensional vector via some matrix $W_{\text{tokenize}}$.

Now, we can construct two types of operations analogous to a MLP. If we have $N$ tokens each with hidden size $d$, we can package them into a single $N\times d$ matrix $T$. Then, a linear combination of tokens is given by $WT$, where $W$ is $1\times N$.

The analogue for a pointwise nonlinearity is a MLP applied to each token. Note that these are learned, unlike in the MLP case (where it's just a fixed ReLU).

![[neural_token_net.png|center|512]]

Note that the picture on the right matches an unrolled [[Architectures for Graphs|GNN]] on a complete graph.

## The Attention Mechanism

This mechanism replaces the graph connectivity structure in GNNs for the linear combination layers. Rather than allowing free parameters $W$, we will use an attention matrix $A$ that is a *function* of the input data.

> [!example] (Query-key-value self-attention)
> In a single head of attention:
> 
> * Every token will ask a ==query== via $q=tW_{q}$.
> * Every token will listen to queries similar to their ==key== $k=tW_{k}$.
> * Every token will respond with a ==value== $v=tW_{v}$.
> 
> For a given query, it is dotted with the keys of all the tokens. This is passed through a softmax and then dotted with the values.
> 
> Concretely, suppose our current tokens are $T\in \mathbb{R}^{N\times d}$. The learnable parameters are $W_{q}\in \mathbb{R}^{d\times d_{k}}$, $W_{k}\in\mathbb{R}^{d\times d_{k}}$, and $W_{v}\in \mathbb{R}^{d\times d_{v}}$. These are multiplied to produce
> $$
> Q=TW_{q} \in \mathbb{R}^{N\times d_{k}},\quad K=TW_{k} \in \mathbb{R}^{N\times d_{k}},\quad V=TW_{v} \in \mathbb{R}^{N\times d_{v}}.
> $$
> The output is
> $$
> Z=\text{softmax}\left( \frac{QK^{T}}{\sqrt{ d_{k} }} \right) V \in \mathbb{R}^{N\times d_{v}},
> $$
> where the softmax is applied to each row.

^0e3e26

One nice thing about learning the weight matrices is that there is no dependence on the sequence length $N$. Here's some example pseudo-Python code for a vision transformer.

```python
# x : input data (RGB image)
# K : tokenization patch size
# d : token/query/key/value dimensionality (all the same here)
# L : number of layers
# W_q_T, W_k_T, W_v_T : transposed query/key/value projection matrices
# mlp : tokenize mlps

T = tokenize(x, K)  # N x d array of token code vectors

for l in range(L):
	# Attention layer.
	Q, K, V = nn.matmul(nn.layernorm(T), [W_q_T[l], W_k_T[l], W_v_T[l]])
	A = nn.softmax(nn.matmul(Q, K.transpose()), dim=0) / sqrt(d)
	T = nn.matmul(A, V) + T  # Note residual connection.

	# Token-wise MLP.
	T = mlp[l](nn.layernorm(T)) + T  # Note residual connection.
```

## Positional Encoding

> [!warning]
> One thing to note about the transformer architecture is that it is permutation equivariant, i.e. both attention layers and token-wise MLPs commute with permutation.

> [!example] (Positional encodings)
> * There are various positional encodings for NLP, e.g. RoPE.
> * One good positional encoding for images represents it on a Fourier basis, i.e. map $(x,y)$ to various values $\sin(x), \sin\left( \frac{x}{B} \right), \sin\left( \frac{x}{B^{2}} \right), \dots$ and $\sin(y),\sin\left( \frac{y}{B} \right),\sin\left( \frac{y}{B^{2}} \right),\dots$
> * For spheres, you could use spherical coordinates or some encoding using spherical harmonics.
> * For graphs, a typical encoding is via eigenvectors of its Laplacian. This maps onto the sinusoids of a lattice. 

> [!question] (My own question)
> What happens to a transformer if you just pad it with a bunch of nothing? Does the positional encoding freak out? Any interpretability on what positional encodings do? What if you swap the positional encoding out with random noise/other things?

## Encoder v.s. Decoder Models

One note for decoder transformer architectures is that you have to ==causally mask== the attention matrix above the main diagonal, so that each token can only attend to ones before it. This allows you to generate text autoregressively!

Another thing you can do is construct encoder-decoder models. For example, you can have encoder layers with full self-attention process an image, and then feed these hidden states into the start of a text decoder with causal masking that autoregressively generates a caption.

Here's an image of such an architecture:

![[image_text_encoder_decoder.png|center|512]]

---

**Next:** [[Hacker's Guide to Deep Learning]]