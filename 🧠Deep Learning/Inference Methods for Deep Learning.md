Here are some [speculations on test-time scaling](https://github.com/srush/awesome-o1).

This "inference" is not the same definition as you learned in [[🤔Inference and Information Portal|inference class]]. In machine learning, model ==inference== refers to making predictions about new datapoints using a trained model, i.e. "prediction" in statistics.

Here is a slide with an overview of the current state of affairs:

![[training_inference.png|center|640]]

Recall Rich Sutton's [[Scaling Laws#^989179|bitter lesson]]. The two methods that scale arbitrarily in this way are *learning* and *search*. 

Here's an example of how OpenAI O1 scales with training and inference time compute costs:

![[o1_aime_scaling.png|center|512]]

Here are some [Scaling scaling laws with board games](https://arxiv.org/abs/2104.03113). Note that you can achieve the same ELO performance by trading off train-time compute with test-time compute.

![[scaling_laws_board_games.png|center|512]]

## Beating Greedy Sampling

In an autoregressive model, greedily sampling next tokens does not maximize likelihood! Therefore, to get higher probability samples, there are a few approaches:

1. ==Best-of-N==: sample N i.i.d. sequences and pick the sequence with highest likelihood.
2. ==Beam search==: keep a tree of samples where you always sample the top-k greedy completions on each step. Then pick the sequence with highest likelihood.
3. ==Chain-of-thought==: in some sense, this is emergent/learned search where you prompt the LLM into performing reasoning steps.

## Other Scoring Functions

You can decide not to just optimize likelihood, but other scoring functions, e.g. "fun-ness" or scientific accuracy. Another option is to have a verifier (Python runtime, Lean kernel) that the LLM output is pushed through.

RLHF also falls into this camp, where you train a reward model on human preference feedback, and your LLM optimizes for this reward. This is valid because you can backprop through it. There are more examples of this, e.g. [GANalyze](http://ganalyze.csail.mit.edu/) from Isola's lab.

## In-Context Learning

Apparently, [Transformers learn in-context by gradient descent](https://arxiv.org/abs/2212.07677). Let $f_{\theta}$ be a transformer and suppose $x$ is a query and $\mathbf{c}$ is the context. Then, we want to demonstrate some equivalence $f_{\theta}(x,\mathbf{c})=f_{\theta'}(x)$. What is the relationship between $\theta$ and $\theta'$?

Well, this paper shows a case where $\theta'=\theta+\lambda \nabla_{\theta}\mathcal{L}(f_{\theta}(x),\mathbf{c})$. So, in-context learning can be expressed as gradient descent over the context examples to update the final mapping from query to answer.

## Test-Time Training

[The surprising effectiveness of test-time training for abstract reasoning](https://arxiv.org/abs/2411.07279). Gradient descent at test time is rather effective, even for solving ARC-AGI prize problems.

## Search to Improve Learning

Like MCTS, I suppose. Once you have searched for a stronger solution based on your verifier, you can add it back to your training data for supervised learning.

One example is called [STaR](https://arxiv.org/abs/2203.14465). 