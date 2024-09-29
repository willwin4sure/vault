Suppose you have some distribution $P$ and you want to fit it with some model $Q_{\theta}$.

You learned in inference that there are two possible ways to do this: either an [[Alternating Projections#^c8c715|M-projection]] $\min_{\theta}D_{KL}(P\parallel Q_{\theta})$, which we call ==forward KL==, or an [[Information Projection and Pythagoras' Theorem#^936604|I-projection]] $\min_{\theta}D_{KL}(Q_{\theta}\parallel P)$, which we call ==reverse KL==.

Suppose $P$ was some bimodal distribution yet all models $Q_{\theta}$ are unimodal. Then, intuitively, the M-projection will tend to pick some distribution that averages between the two peaks (mean-seeking), while the I-projection will tend to just pick the larger peak (mode-seeking).

This is because the M-projection is trying to minimize surprise of $P$ given $Q_{\theta}$, so you need to be relatively balanced. Meanwhile, the I-projection is trying to minimize surprise of $Q_{\theta}$ given $P$, so you can just send it at the mode.

> [!idea]
> Minimizing the forward KL is equivalent to supervised learning, while minimizing the reverse KL is equivalent to reinforcement learning (with max-entropy).
> 
> The former is clear; M-projections extract the MLE. The latter is true because in RL, your own policy determines what sort of data you collect.





