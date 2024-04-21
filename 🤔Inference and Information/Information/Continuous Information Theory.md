So far, we've been working in the case of discrete random variables. Let's extend our work to the continuous domain. It turns out that some things will break, unfortunately. 

## Information Measures

> [!definition] (Differential entropy)
> This is our replacement for entropy. It is an integral instead of a summation:
> $$
> h(\mathsf{x})=-\int_{-\infty}^{\infty} p_{\mathsf{x}}(x)\log p_{\mathsf{x}}(x) \, dx.
> $$

> [!warning]
> Differential entropy is no longer nonnegative in general! For example, consider a uniform distribution on $(0,\Delta)$. The entropy is $\log\Delta$, which is negative when $\Delta<1$.

Further, differential entropy is also not invariant to coordinate transformations. This might hint at the fact that it is not that fundamental of a quantity.

> [!definition] (Conditional differential entropy)
> It's what you expect, i.e.
> $$
> h(\mathsf{x}|\mathsf{y})=\int_{-\infty}^{\infty} p_{\mathsf{y}}(y)h(\mathsf{x}|\mathsf{y}=y) \, dy,
> $$
> where
> $$
> h(\mathsf{x}|\mathsf{y}=y)=-\int_{-\infty}^{\infty} p_{\mathsf{x}|\mathsf{y}}(x|y)\log p_{\mathsf{x}|\mathsf{y}}(x|y) \, dx .
> $$

However, the following quantity *is* still always nonnegative, and also *is* invariant to coordinate transformations!

> [!definition] (Mutual information)
> Also extends directly from the discrete case via
> $$
> I(\mathsf{x};\mathsf{y})=h(\mathsf{x})-h(\mathsf{x}|\mathsf{y}).
> $$

## Information Divergence

> [!definition] (KL divergence)
> The divergence between two continuous distributions is given by
> $$
> D(p\parallel q)=\int_{-\infty}^{\infty} p(x)\log \frac{p(x)}{q(x)} \, dx. 
> $$

By an extension of [[Some Useful Inequalities#^e52776|Gibbs' inequality]] to the continuous case, this is still nonnegative. This actually shows that the mutual information is positive, since recall that
$$
I(\mathsf{x};\mathsf{y})=D(p_{\mathsf{xy}}\parallel p_{\mathsf{x}}p_{\mathsf{y}}),
$$
i.e. the mutual information is the divergence between the joint distribution of $\mathsf{x}$ and $\mathsf{y}$ and what you would expect if you just considered them independent with their marginals.

Note that this implies that conditioning, in aggregate, can never increase entropy, i.e. $h(\mathsf{x})\geq h(\mathsf{x}|\mathsf{y})$. 

Finally, information divergence is also coordinate-free.

## Mixture Modeling

When we extend our results on modeling to continuous $x$, the model takes the form
$$
q_{w}(y)=\int_{x \in \mathcal{X}} w(x) p_{\mathsf{y}}(y;x) \, dx.
$$
The model capacity remains as $C=\max_{p_{\mathsf{x}}}I(\mathsf{x};\mathsf{y})$, which means that it is also unaffected by reparameterization. This is intuitively pleasing: the metric is unchanged if we index our candidate models differently.

---

**Next:** [[Information Measures for Gaussians]]


