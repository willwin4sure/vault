We now see the role that information measures play in inference from a non-Bayesian perspective in the problem of ==modeling==.

In [[⛺Estimation Homepage|estimation]], our goal was to determine the unknown parameter $x \in \mathcal{X}$. Now, our goal is to determine a good approximation $q(\bullet)$ to the true, unknown distribution for $\mathsf{y}$.

> [!example] (Estimation approach)
> One simple approach would be to use our tools of estimation to get a good guess $\hat{x}$, and simply use the distribution $p_{\mathsf{y}}(\bullet;\hat{x})$.

Once again, we restrict to the case of discrete $\mathcal{X}$ and $\mathcal{Y}$.

> [!definition]
> An approximating distribution is called ==admissible== if it does not depend on $x$ (and only on $\mathsf{y}$), analogous to valid estimators. 

---

**Next:** [[Mixture Models]]