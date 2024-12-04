This lecture concerns graph neural networks. They actually aren't universal... we can consider this a bonus inductive bias, though it has limitations.

## Applications

==Graph neural networks== can be used to process data represented as graphs. Some key outputs are *node embeddings* or *graph embeddings*, which can be further used for various purposes.

> [!example] (Molecule property prediction)
> Molecules are naturally represented as graphs, and GNNs can predict properties like solubility, toxicity, and drug efficacy.

When you construct your graph, you can have different types of nodes as well as different types of edges encoding various interactions.

> [!example] (Other applications for GNNs)
> * Predicting traffic times (i.e. a shortest path problem).
> * Fluid/particle physics simulations (forces between nodes as edges).
> * Combinatorial optimization. A learning-based method could find a faster approximate solution specialized for the data distribution.

## Graph Neural Networks

**Input:** Adjacency matrix $A \in \mathbb{R}^{n\times n}$ and feature matrix $X \in \mathbb{R}^{n\times d}$ (vector per node).
**Output:** Node embeddings (node2vec) or graph embeddings (graph2vec).

Ideally, we want to process the graph in a ==permutation invariant== manner. We will model this via message passing that alternates between two steps. On each round $k$,

1. **Aggregate** information from neighbors.

$$
m_{\mathcal{N}(v)}^{(k)}=\text{AGGREGATE}^{(k)}\left( \{ h_{u}^{(k-1)} : u \in \mathcal{N}(v) \} \right).
$$

2. **Update** current node's representation by incorporating messages from neighbors. This 

$$
h_{v}^{(k)}=\text{UPDATE}^{(k)}\left( h_{v}^{(k-1)}, m_{\mathcal{N}(v)}^{(k)} \right).
$$

> [!definition] (GNN aggregation)
> The $\text{AGGREGATE}$ function is constrained to be a permutation invariant, multi-set function. For example, it can be a sum or an average. Another popular choice normalizes by both your degree and your neighbor's degree:
> $$
> m_{\mathcal{N}(v)}=\sum_{u \in \mathcal{N}(v)}^{} \frac{h_{u}}{\sqrt{ |N(v)||N(u)| }}.
> $$
> We could also take a maximum or a minimum. This lets you implement shortest path via Bellman-Ford relaxation.

> [!claim] (Universal aggregation)
> A general form that [[Universal Approximation|universally approximates]] any multi-set function is
> $$
> m_{\mathcal{N}(v)}=\text{MLP}_{2}\left( \sum_{u \in \mathcal{N}(v)}^{}\text{MLP}_{1}(h_{u},h_{v}) \right).
> $$

^c3bf4e

> [!definition] (GNN update)
> The $\text{UPDATE}$ function can be arbitrary. One common choice is just a single linear layer followed by a pointwise nonlinearity.

If you want a graph embedding out of these node embeddings, you can perform another aggregate operator $\text{READOUT}$ to pool all the nodes together into a single embedding.

> [!warning]
> We haven't supported edge attributes yet, but you can modify the algorithm to support them!

Another extension is to have multiple "channels" running separate message-passing algorithms in parallel, like multi-head attention.

## Training Procedure

Training data points should look like (node, label) or (graph, label) pairs, depending on if we want node or graph embeddings.

Then, you'll need to specify aggregate, update, and readout functions (with tunable parameters) and train with SGD.

## Limitations

> [!warning]
> The particular aggregation function you pick is quite important!

Here are the performances of various setups for the aggregation function:

![[aggregation_setups.png|center|512]]

The best performing one is the one that satisfies [[Architectures for Graphs#^c3bf4e|universal aggregation]] from earlier.

> [!warning]
> There are equivalence classes of different graphs that GNNs can't distinguish!

For example, the two graphs below are indistinguishable, due to the symmetries we've imposed in the GNN's processing power.

![[indistinguishable_graphs.png|center|128]]

> [!theorem]
> Any GNN can at best distinguish the same graphs as the 1-dim [WL algorithm](https://en.wikipedia.org/wiki/Weisfeiler_Leman_graph_isomorphism_test). And this is achievable by some GNN.

This means that GNNs don't have the power to answer questions like:

* What is the length of the shortest or longest cycle in the graph?
* What is the diameter of the graph?
* How many times does a motif occur in the graph?

One way of improving the discriminative power is adding positional encodings, e.g. eigenvectors of the graph Laplacian. This does come with some added ambiguities, however. 

## Connections

A [[multilayer perceptron]] can be thought of as a graph neural network on a single node.

A [[Architectures for Grids|convolutional neural network]] can be thought of as a graph neural network on the image graph, where adjacent pixels are connected.

A [[Transformers|transfomer]] can be thought of as a graph neural network on the fully connected graph on the tokens within the attention window. The attention mechanism is a special aggregation operation with a particularly useful parameterization.

---

**Next:** [[Neural Network Generalization]]