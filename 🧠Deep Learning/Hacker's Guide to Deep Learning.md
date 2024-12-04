> This is an opinionated lecture by Phillip Isola.

Part of the story of deep learning has been the (temporary) success of hacking over theory. Even if we don't yet have a systematic theory, things that work empirically are useful and propagate.

## Look at your data

> Become friends with every pixel.

You should look at your inputs.

> [!example] (Breast cancer classification)
> A model was trained on breast cancer scans and was able to achieve stunning 99% accuracies, but it failed in practice. Here is an image:
> 
> ![[deep_net_tissue_classification.png|center|256]]
> 
> It turns out that the model was able to learn some spurious correlation based on the letter "R" in the corner (maybe it identified the hospital, and maybe some hospitals screened women while anothers treated cancer patients).

You should also look at your outputs. The loss curve over epochs is not sufficient! For example, if you are training some generative image model, try sampling it and seeing how the images change over time.

> [!example] (Ants in predator prey game)
> Example from Isola where he was training ants to play a predator-prey game, and the rewards started plateauing after some time.
> 
> ![[predator_prey.png|center|256]]
> 
> Turns out the reason was that they were falling over, not because of task difficulty.

> [!idea] (Inspect the distribution of your inputs and targets)
> 1. Inspect random selections of inputs and targets to have a general sense.
> 2. Histogram input dimensions to see range and variability.
> 3. Histogram targets to see range and imbalance.

Another common bug is that when you preprocess your data, the way you are trying to load it does not agree with the way it is actually stored.

> [!idea] (Check your data pre-processing)
> 1. The most important data you need to inspect is as it is given to the network, at the time of the line `output = model(data)`.
> 2. Make sure the dimensionality and shape agree with what you expect.

Here is a helpful function you can call everywhere in your code:

```python
def inspect_data(X):
	print(f"type: {X.type()}")
	print(f"shape: {X.shape}")
	print(f"requires grad: {X.requires_grad}")
	print(f"numerical range: [{X.mean():.2f}, {X.max():.2f}]")
	print(f"mean and var: {X.mean}, {X.var}")
```

The type is important to check: often you could have type issues if you cast tensors improperly. For shapes, one good way of sanity checking your code is ensuring that you use different sizes for various dimensions.

> [!idea] (Standardize your features per dimension)
> 1. Often good if different features are wildly different scales, in order to make the network pay attention to all of them.
> 2. This does remove any inductive bias in case that is actually important.
> 3. Another technique is to subtract the min and divide by the new max so that all values are in the range $[0,1]$.

> [!warning] (Beware of low dimensions)
> Many normalization layers behave badly in low dimensions.
> 
> ![[low_dim_rms_layernorm.png|center|512]]
> 
> Batch norm is also bad if the batch dimensionality is too small. So generally, you want to keep all your tensor dimensions large in size.

> [!warning]
> A lot of your code is just reshaping data. This can be made more understandable via tools like `einops`.

> [!idea] (Apply data augmentation)
> From image processing, you can make your dataset larger and your model more robust by augmenting your training examples with operations such as mirroring, cropping, darkening, etc.

Here are some loss curves and some intuition about them. 

![[loss_curves_good_bad.png|center|512]]

You roughly want to select data and parameters via $\max_{\text{data}}\min_{\text{params}}\text{loss}(\text{data},\text{params})$. Usually in deep learning you first care about the inner parameter optimization, but afterwards you want to select your data so that the problem is nontrivial.

Domain randomization in robotics follows the same principle, in wanting your models to generalize to more cases. This way it can be robust to a real-world environment which may look different than any individual simulation scene.

> [!idea] (Data, data, data)
> In the academy, we often treat the data as *fixed* and design learning algorithms to produce the best results. In industry, however, we often just grab the latest learning algorithm and go *collect* data to instruct the algorithm as well as we can! This is often more powerful.

By throwing more and more information into the features $x$, the distribution on possible outputs $y$ becomes constrained, and the problem becomes easier. 

## Models

> [!idea] (Keep it simple!)
> 1. Easy to build, debug, and share.
> 2. Tractable to understand, more robust, can build theories around.
> 3. *Simple models* also work better (as long as they fit the data).
> 4. If you focus on simplicity you will have an unfair advantage.

Simplicity allows your contributions to stand the test of time. You should start with a standard and popular model (popularity matters more than performance).

> [!example] (Popular models)
> * ResNet for image classification problems.
> * Transformers for textual problems.

In general, stand on the shoulders of giants. Use pretrained models, but be aware of flaws and limitations.

> [!idea] (Transform your problem into a solved problem)
> For example, can you transform image *colorization* into image *classification*? Well you could quantize color space into classes. Then, you can output a whole image of distributions on these classes without flattening, allowing you to color per pixel.

Here are some good default choices circa 2024:

1. Transform your data into numbers (one-hot vectors).
2. Transform your goal into a numerical measure (cross-entropy loss).
3. Use a generic optimizer (Adam) and a standard architecture (transformer) to solve the learning problem.

> [!warning] (Don't use batch normalization)
> * It introduces a strong dependence on batch size.
> * It behaves differently at train and test time.
> * It makes distributed computing hard, since it requires communication between all elements in a batch.
> Use layer norm instead!
> 

> [!idea] (Scale, scale, scale)
> The easiest way to get better performance is:
> 1. Scale your data: more (diverse) training examples.
> 2. Scale your model: more layers, more channels.
> 3. Scale your compute: train for longer.

> Perfection is finally attained not when there is no longer anything to add, but when there is no longer anything to take away.
> 
> — Antoine de Saint Exupéry

> [!idea] (Clean up after yourself)
> Once you get your model to work, you're only halfway done! The second half is to remove everything nonessential to the performance.

## Optimization

The general procedure for optimization should be to fit on one/few/many datapoints, in that order. You should overfit to a single data point, then to a batch, then to the train dataset. Afterwards, you can consider generalization to the test set.

> [!idea] (Sanity check the loss)
> For classification with cross-entropy loss, compare with uniform distribution values. For regression with squared loss, make sure it is positive. If you loss is constant, check for zero-initialization of your weights.

> [!idea] (Tune hyperparameters)
> The most important are **learning rate** and **batch size**. First use a fixed learning rate before scheduling. Use the biggest batch size that can fit in memory. Always *return* learning rate whenever anything changes about your model (these will change scale of gradients).

> [!warning] (Be careful with concept of epochs)
> There are no epochs "in the wild". Trend towards single-epoch training of LLMs. Don't tie learning rate schedules to epochs; use SGD iterations instead for better comparison of learning curves between experiments.

> If you've never missed a flight, you're spending too much time in airports.

Sometimes, you should try at-the-edge settings: what learning rates cause the model to diverge?

> [!idea] (Checkpoint models)
> Trade space for time. Checkpoint features and gradients, so you can resume training if your computer crashes. This also gives a paper trail to debug.

> [!idea] (Use EMAs)
> This is common in most popular optimizers, via $\theta_{\text{EMA}}^{t}\leftarrow \beta\theta_{\text{EMA}}^{t-1}+(1-\beta)\theta^{t-1}$. This average over time can often achieve a similar effect as averaging over "space" (i.e. gradient totaled over the batch). Useful for many quantities in deep learning, including gradients, weights, data, activations, targets, etc.

Adam or AdamW is good for prototyping (generally just works). SGD could do better but requires more tuning of hyperparameters. Finally, clip gradients to improve stability. 

## Common Bugs

* `RuntimeError: a view of a leaf Variable that requires grad is being used in an in-place operation.`

The leaf variables will be the weights of your network and the inputs into the network in most settings.

You can't use in-place operations on leaf variables in your computation graph if they require gradients. This is because developers couldn't agree on how such operations should behave (do they break the computation graph?). 

* `out of memory`

At inference time, you don't need to store gradients:

```python
with torch.no_grad():
	y = model.forward(X)
```

You should also clear memory whenever appropriate:

```python
torch.cuda.empty_cache()
del variable_name
```

* `Timing your code`

GPU calls may run asynchronously, so if you want to time an operation, make sure to call synchronize first.

```python
torch.cuda.synchronize()

timer.start()
y = model.forward(X)
timer.stop()
```

* `RuntimeError: Trying to backward through the graph a second time, but the buffers have already been freed. Specify retain_graph=True when calling backward the first time.`

PyTorch frees the computation graph after calling `backward()`. This error occurs because you usually don't want to backward twice. But if you really do, the `retain_graph` solution exists.

## Compute

> [!warning] (More hardware, more problems)
> Don't parallelize immediately!
> 
> * Make your model work on a single device first.
> * Attempt to parallelize on a single machine.
> * Only then go to a multi-machine set.
> * Check that iterations/wall clock time actually improves!
>   
> More advice can be found in [this paper](https://arxiv.org/abs/1706.02677).

> [!idea] (Saturate your GPUs)
> * Check GPU utilization (memory and flops): `nvtop` and `nvidia-smi`.
> * Increase batch size until ~100% utilization.

> [!idea] (Benchmarking)
> You can include `torch.cudnn.benchmark = True` at the top of your scripts. You can also try [AMP](https://developer.nvidia.com/automatic-mixed-precision).

---

**Next:** [[Architectures for Memory]]