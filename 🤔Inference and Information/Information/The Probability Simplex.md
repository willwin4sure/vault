> The spatial metaphor is perhaps *the* most important thinking tool of the 20th century. It was then that physicists learned to draw phase spaces; ecologists, population spaces; roboticists, configuration spaces; statisticians, parameter spaces. Regarding collections as spaces gives us context to better compare collection members, and the common interface of "space" allows us to import and export intuitions from adjacent fields. Plus, we get to employ our visual cortex against problems, a boon as great as that of GPUs over CPUs.
> 
> — Useless philosophy of space from TA Samuel Tenka

## The Probability Simplex

If our alphabet is $\mathcal{Y}=\{ 1,2,\dots,M \}$, then our *space* of distributions takes the form of an $(M-1)$-dimensional simplex. We will refer to this ==probability simplex== as $\mathcal{P}^{\mathcal{Y}}$.

Here's an image of a single-parameter exponential family lying on a probability simplex over $3$ variables:

![[exponential_family_simplex.png|center|384]]

This corresponds to weighted *geometric* means of two base distributions. We can also consider the weighted *arithmetic* means of two base distributions, which will linearly interpolate between two points:

![[linear_family_simplex.png|center|384]]

The KL divergence is a warped measure of distance on this simplex (also, don't forget that it isn't symmetric!).

> [!idea]
> It turns out that KL divergence is analogous to the *square* of Euclidean distance. For example, recall that only the second order term in $\delta$ exists in its [[KL Divergence#Relationship to Fisher Information|Taylor expansion]].

Near the boundary of the simplex, movements parallel to the boundary correspond to much smaller "distances" than movements towards/away from the boundary. Therefore, "circles" under KL divergence get squashed into ellipses near boundaries.

Also, don't forget that if you actually are on the boundary, your support changes. If $p$ is a distribution on the boundary and $q$ is a distribution in the interior, then $D(p\parallel q)$ is finite, but $D(q\parallel p)$ is infinite!

---

**Next:** [[Information Projection and Pythagoras' Theorem]]



