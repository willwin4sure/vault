Suppose the complete data comes from a canonical $K$-dimensional exponential family
$$
p_{\boldsymbol{\mathsf{z}}}(\mathbf{z};\mathbf{x})=\exp \left\{ \mathbf{x}^{T}\mathbf{t}(\mathbf{z})-\alpha(\mathbf{x})+\beta(\mathbf{z}) \right\}. 
$$
Then, the E-step is the computation
$$
U(\mathbf{x},\mathbf{x}')=\mathbf{x}^{T}\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y};\mathbf{x}')}[\mathbf{t}(\boldsymbol{\mathsf{z}})]-\alpha(\mathbf{x})+\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y};\mathbf{x}')}[\beta(\boldsymbol{\mathsf{z}})].
$$
The M-step will consider the gradient of $U$ with respect to $\mathbf{x}$: for each component, we ahve that
$$
0=\frac{ \partial }{ \partial x_{k} } U(\mathbf{x};\mathbf{x}')=\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y};\mathbf{x}')}[t_{k}(\boldsymbol{\mathsf{z}})]-\frac{ \partial }{ \partial x_{k} }\alpha(\mathbf{x})=0. 
$$
Yet we know from [[Multi-Parameter Exponential Families#Properties that Carry Over|properties of exponential families]] that
$$
\frac{ \partial }{ \partial x_{k} } \alpha(\mathbf{x})=\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}}(\bullet;\mathbf{x})}[t_{k}(\boldsymbol{\mathsf{z}})].
$$
Therefore, the M-step chooses the parameter update in order to match the moments
$$
\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}}(\bullet;\hat{\mathbf{x}}^{(l+1)})}[t_{k}(\boldsymbol{\mathsf{z}})]=\mathbb{E}_{p_{\boldsymbol{\mathsf{z}}|\boldsymbol{\mathsf{y}}}(\bullet|\mathbf{y};\hat{\mathbf{x}}^{(l)})}[t_{k}(\boldsymbol{\mathsf{z}})].
$$
> [!question]
> Check that any fixed point of this update is a stationary point of $\ell_{\boldsymbol{\mathsf{y}}}(\bullet;\mathbf{y})$.
