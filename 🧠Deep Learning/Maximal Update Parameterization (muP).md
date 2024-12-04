> From [this paper](https://arxiv.org/abs/2203.03466).

The main content is in Table 3 of the paper, which contains the parameterizations for standard layers. Note that for transformer architectures we also use $\frac{1}{d}$ inside the softmax instead of the [[Transformers#^0e3e26|usual]] $\frac{1}{\sqrt{ d }}$.

This allows hyperparameters to transfer across network sizes, which is very useful when training large models.