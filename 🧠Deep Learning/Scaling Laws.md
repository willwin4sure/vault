> Model performance improves *predictably* with scale.

One recent insight in deep learning is that scaling up data, compute, and model size increases performance in a *predictable* way, via ==scaling laws==. These are very useful for training huge models efficiently.

> [!idea] (The bitter lesson)
> Rich Sutton states that most of our clever ideas and algorithms don't really pan out, when they try trickery via human knowledge. Instead, simple methods that scale well have performed better: your resources will increase over time.

^989179

Here is the fundamental question we want to answer:

> [!question]
> If you have a given budget of compute, what model would you train on what data?

OpenAI has a good example of a scaling law used when choosing how to train GPT-4:

![[gpt4_scaling_law.png|center|512]]

Notice how the performance of next-word prediction improves as compute scales up. This lets you design the models at 10,000x the scale, for massive savings!

> [!warning]
> Recent news about failures in Orion training (GPT-5 attempts) may have signaled the potential failure of continued scaling. We are likely hitting limits of available data.

There are similar scaling laws for predicting downstream performance of the model, which is rather impressive!

The seminal work in this field was [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361), by a team at OpenAI, though some of the authors spun off to form Anthropic. 

## Takeaways from Kaplan et al.

We find relationships of the following form:

> [!definition] (Power law relationship)
> This is when loss follows some relationship with scale via
> $$
> \mathcal{L}(X)=X^{-\alpha}.
> $$
> This means if you double your scale, your performance should loss should go down by some factor.

> [!claim] (Empirical power laws)
> Some choices for $X$, the thing we're scaling:
> 
> * Dataset size $D$, in tokens. Gives relationship $\mathcal{L}(D)=(D_{c}/D)^{\alpha_{D}}$, where $\alpha_{D}\sim 0.095$ and $D_{C}\sim 5.4\times 10^{13}$ tokens.
> * Parameter count $N$. Gives relationship $\mathcal{L}(N)=(N_{c}/N)^{\alpha_{N}}$, where $\alpha_{N}\sim 0.076$ and $N_{c}\sim 8.8\times 10^{13}$ non-embedding parameters.
> * Compute $C$. Gives relationship $\mathcal{L}(C)=(C_{c}^{\min}/C^{\min})^{\alpha_{C}^{\min}}$, where $\alpha_{C}^{\min}\sim 0.050$ and $C_{c}^{\min}\sim 3.1\times 10^{8}$ PF-days.

Here are the plots:

![[power_scaling_laws.png|center|640]]

Note that the compute scaling law is discussing the minimal compute you need to achieve that loss if you optimally set dataset size and parameter count. The blue curves depict the training curves that make up the Pareto-optimal frontier.

The two plots on the right above show what happens when you scale one of the properties and *optimize* along the other dimensions (take them to infinity). If you don't scale up everything together, performance will drop off:

> [!claim] (Scaling in tandem)
> Performance increases predictably so long as we scale both $N$ and $D$ in tandem, but it will enter a regime of diminishing returns if either is held fixed while the other increases, as shown in the plot below. You can actually fit a joint law
> $$
> \mathcal{L}(N,D)=\left[ \left( \frac{N_{c}}{N} \right)^{\alpha_{N}/\alpha_{D}}+\frac{D_{c}}{D} \right]^{\alpha_{D}}
> $$
> with different parameters.

![[tandem_scaling.png|center|384]]

Analogous behavior is observed when jointly scaling number of parameters and training steps:
$$
\mathcal{L}(N,S_{\min})=\left( \frac{N_{c}}{N} \right)^{\alpha_{N}}+\left( \frac{S_{c}}{S_{\min}} \right)^{\alpha_{S}}.
$$
Similar power laws have been discovered in other domains, e.g. vision transformers. And another key observation is that these power laws **extrapolate** to orders of magnitude beyond the data you use for fitting.

> [!claim] (Dependence on model shape)
> Performance depends strongly on scale, but actually weakly on model shape. In particular, a wide range of architectures achieve similar performance.

![[model_shape_dependence.png|center|640]]

This is a pretty good takeaway. Don't fuss too much about your model shape, just make it the right size!

> [!claim] (Sample efficiency)
> Larger models actually require **fewer samples** to reach the same performance.

![[scaling_sample_efficiency.png|center|384]]

This is a rather surprising finding from a classical point of view. You might imagine a larger model requires more data to not overfit, but test loss actually goes down faster with scale. This is reminiscent of some lessons from [[Neural Network Generalization#Double Descent|double descent]], where our classical statistical ideas of training aren't correct.

> [!warning]
> One idea you might get from this is that overfitting just doesn't occur. Apparently this is true in the regime of these models; note for example that each model is trained with a single pass through the data (one epoch). Overfitting certainly does occur in other regimes.

Another thing the graph above suggests is that training a large model not to convergence can be better than training a small model to convergence, even with equal compute. This gives rise to another practice of training networks:

![[bair_compress_training.png|center|512]]

And this can work quite well:

![[roberta_pruning.png|center|384]]

## Chinchilla Scaling Laws

They didn't investigate various regimes, e.g. if you have small amounts of data or huge models. Another main complaint is that they didn't tune hyperparameters fully or investigate techniques like regularization or data augmentations.

DeepMind's paper [Chinchilla](https://arxiv.org/abs/2203.15556) highlighted some of these issues, and developed their own set of scaling laws. While Kaplan et al. suggested that scaling model size and keeping data size fixed would be optimal for performance, Chinchilla suggests that they should actually scale in equal proportion.

> [!idea]
> One of the main reasons for the discrepancy of findings is that Chinchilla **tuned their learning rates**. This gives a different power law with different predictions.

The findings here are that it is still optimal to not train to minimum loss, but you should train on many more tokens and steps for given model size. In particular, they found that $N$ and $D$ should scale **equally** with $C$.

> [!warning]
> Perhaps these scaling laws will change as we move to higher orders of magnitudes or different training techniques.

## Why Scaling Laws?

One idea is by modeling neural networks as performing regression on an embedded data manifold, as presented in [Scaling Laws from the Data Manifold Dimension](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://jmlr.csail.mit.edu/papers/volume23/20-1111/20-1111.pdf).

If $d$ is the *intrinsic dimensionality* of the data manifold, then we get the scaling law
$$
\mathcal{L}(N)\propto \frac{1}{N^{\alpha}},
$$
where $\alpha \approx 4 / d$ via something like the toy [[Universal Approximation#Main Theorem|Lipschitz model]] we had before in terms of total error in terms of dimensionality when fitting with ReLU functions. 

There is some empirical evidence that supports this as well.

## Breaking Scaling Laws

One way of doing this is via effective *data pruning*, which leads to better scaling. In particular, normally each next token in the scaling model we had before gives diminishing returns in terms of performance, suggesting that our original training examples are highly redundant.

## Correct Batch Size

Bigger batch sizes are more costly but provide better estimates of the full-batch gradient. The TL;DR of [An Empirical Model of Large-Batch Training](https://arxiv.org/abs/1812.06162) is that one step of SGD can make progress proportional to
$$
\frac{1}{1+\mathcal{B}_{\text{noise}}/B},
$$
where $\mathcal{B}_{\text{noise}}$ is a measure of how much noise there is between gradient estimates of different batches. This gives the following plot, where you get good scaling in terms of batch size for a while, before it becomes ineffective.

![[batch_size_scaling.png|center|384]]

## Representational Convergence

The topic of [The Platonic Representation Hypothesis](https://arxiv.org/abs/2405.07987), a work in Isola's lab. And actually what my final project is about!

---

**Next:** [[Metrized Deep Learning]]