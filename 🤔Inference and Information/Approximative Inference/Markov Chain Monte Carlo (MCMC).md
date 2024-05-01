> A tool to sample from distributions by constructing an ergodic Markov chain with appropriate stationary distribution and running it.

This is a topic from 18.615 and 6.1220, so you should already know it pretty well.

We skip the basics of Markov chains. Recall that any irreducible Markov chain has a unique stationary distribution, and if the Markov chain is aperiodic, then the convergence theorem says that the limiting distribution from running the Markov chain started at any initial distribution is the stationary distribution. Also recall that if a distribution reverses a Markov chain, then it is stationary.

The mixing time of a Markov chain is characterized by the second largest eigenvalue of its transition matrix (the largest eigenvalue is $1$, corresponding to the stationary distribution). In particular, the mixing time is $\frac{1}{\log\lambda_{2}}$. 

## Metropolis-Hastings

We want to sample from some distribution $p$ over $\mathcal{X}$, but we only know $p_{\circ}$ so that $p\propto p_{\circ}$ but not the normalization factor. So we construct a Markov chain on the elements of $\mathcal{X}$, and then reweight it to be reversible.

To do this, we just balance the flows. In particular, we want to force that
$$
p(x)v^*(x'|x)=p(x')v^*(x|x'),
$$
where $v^*$ are our modified transition probabilities. In order to get this done at each node, simply set
$$
p(x)v^{*}(x'|x)=\min\{ p(x)v(x'|x), p(x')v(x|x') \}
$$
for all connected $x\to x'$. This is commonly rewritten as an acceptance probability on the transition, i.e.
$$
a(x\to x')=\min\left\{ 1, \frac{p_{\circ}(x')v(x|x')}{p_{\circ}(x)v(x'|x)} \right\}.
$$
> [!theorem] (Metropolis-Hastings)
> If $v(\bullet|\bullet)$ is a Markov chain on $\mathcal{X}$, then the construction $v^{*}(\bullet|\bullet)$ as above determine transition probabilities of a Markov chain on $\mathcal{X}$ that is reversible with respect to the target distribution $p$.

In the case that the Metropolis-Hastings Markov chain is irreducible and aperiodic, convergence theorem applies. Note that aperiodicity is easy to achieve by adding laziness.

Clearly M-H is optimal: theoretically you could scale each pair of acceptances down, but this just makes the chain more lazy, causing it to converge slower.

---




