MLPs are nice for various reasons: they're universal, simple, and embarrassingly parallel. However, they have weak inductive biases and are therefore sample inefficient. The dense layers also require a lot of time to compute.

This lecture begins to introduce different neural architectures. These add priors by restricting our hypothesis space, hopefully allowing us to find better solutions.

## Convolutions

CNNs are great for grids. They spawned out of image classification problems, and have similarities to the way our visual cortex works.

One of the main inductive biases introduced by CNNs is *translational invariance*.

You already know how CNNs work. A standard ==classification model== might have multiple layers of filter, max pool, and then downsamples. You can control the number of channels, the size of the filters and pooling layers, as well as the strides of the operations.

You can also have ==encoder-decoder models==, which would facilitate compression and regeneration of images. In similar vein, ==image-to-image models== often use the ==UNet architecture==, which looks like an encoder-decoder setup but with skip connections across the gap. This is often used for segmentation tasks.

If you have video data, you can have a data tensor for spacetime, and then convolve using a kernel through space and time. Can be used for action recognition tasks for video.

## Neural Fields

Sometimes the shift invariance is undesired. In this case, you can add a hand-crafted ==positional encoding== to the model. This idea appears in transformer architectures as well.

A related concept is that of a neural field, where you want to learn some function $(x,y)\mapsto \Phi(x,y)$ of the position by representing it as a neural network. The ==SIREN model== is rather good at modeling signals such as sounds and images, and also captures their derivatives well.

==NeRF== uses this method to represent scenes by training a model $(x,y,z,\theta,\phi)\mapsto (RGB\sigma)$ that maps from a spatial location and viewing angle to a color and density. This technique has mostly been subsumed by ==Gaussian splatting==.

---

**Next:** [[Architectures for Graphs]]

