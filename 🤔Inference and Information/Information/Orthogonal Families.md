It turns out that linear and exponential families are orthogonal to each other, in a certain sense.

> [!theorem]
> Let $p^{*}\in \mathcal{P}^{\mathcal{Y}}$ be an arbitrary distribution. Then $p^{*}$ is the I-projection of a distribution $q\in \mathcal{P}^{\mathcal{Y}}$ onto the linear family
> $$
> \mathcal{L}_{\mathbf{t}}(p^{*})=\{ p \in \mathcal{P}^{\mathcal{Y}} : \mathbb{E}_{p}[\mathbf{t}(\mathsf{y})]=\mathbb{E}_{p^{*}}[\mathbf{t}(\mathsf{y})] \}
> $$
> for any choice of $\mathbf{t}$, if and only if $q$ is in the exponential family
> $$
> \mathcal{E}_{\mathbf{t}}(p^{*})=\{ q\in \mathcal{P}^{\mathcal{Y}} : q(y)=p^{*}(y)\exp \{ \mathbf{x}^{T}\mathbf{t}(y)-\alpha(\mathbf{x}) \},y\in \mathcal{Y},\mathbf{x}\in \mathcal{X} \},
> $$
> where $\mathcal{X}$ is the natural parameter space. We refer to this exponential family as the ==orthogonal family== to $\mathcal{L}_{\mathbf{t}}(p^{*})$ at $p^{*}$.

^db7aaf

We can prove this by just staring at the KL divergence $D(p\parallel q)$. 

> [!idea]
> This theorem gives an efficient way of computing an I-projection.

Indeed, we can just take the corresponding exponential family passing through your $q$ and then intersect it with the linear family.

---

**Next:** [[Modeling as Inference]]

