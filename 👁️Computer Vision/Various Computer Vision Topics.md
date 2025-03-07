## Tracking and Optical Flow

> Motion estimation today (as of 2025) is via supervised learning on synthetic data.

You have a video of a soccer player running around a field. Can you determine where it is at any point in time?

### Template Tracking

Suppose you have a template image that you are trying to pick out of each frame. Then you can just compute patch-wise similarities against the template to find the most likely locations. 

### Optical Flow

This is an idea developed at MIT where you imagine a dense flow field transporting every pixel in the image over time. The seminal paper introduced the Horn-Schunck algorithm, which enforces some smoothness over the flow.

One way of training optical flow models that has stood the test of time is generating synthetic data (e.g. [Flying Things](https://arxiv.org/abs/1512.02134v1)) of moving objects and then training a CNN on it (to predict the flow of pixels). This generalizes well to real data, perhaps because optical flow is a low-level vision problem, and therefore [[Simulators|sim2real]] works well.

Another dataset is [Sintel](http://sintel.is.tue.mpg.de/) dataset, which was generated using the underlying assets of an animated movie for the ground truth (and its rendered result). 

To learn optical flow, we use a [[Inverse Models#^b1c6ad|Siamese architecture]] that takes in two frames and combines them in some way:

![[optical_flow_arch.png|center|768]]

Some papers doing this are [FlowNet](https://arxiv.org/abs/1504.06852) and [RAFT](https://arxiv.org/abs/2003.12039).

Recently, there have been some more papers revisiting point tracking; see [Particle Video Revisited](https://arxiv.org/abs/2204.04153). 

## Stereo

> Given multiple perspectives of a scene, infer its structure.

This is what you do with your eyes all the time. For example, here's a solution to [structure from motion](https://people.eecs.berkeley.edu/~yang/courses/cs294-6/papers/TomasiC_Shape%20and%20motion%20from%20image%20streams%20under%20orthography.pdf). 

### Iterative Closest Points

Match two point clouds by aligning them. See a [method of registering 3d shapes](https://ieeexplore.ieee.org/document/121791). 

### Large-Scale SfM

Some cool papers: [Photo Tourism](https://phototour.cs.washington.edu/Photo_Tourism.pdf) and [Building Rome in a Day](https://grail.cs.washington.edu/rome/rome_paper.pdf). This is pre-deep learning, but internet-scale data was used to reconstruct the Colosseum. 

Nowadays, people use a large-scale open source C++ framework called [COLMAP](https://colmap.github.io/), which is the defacto standard SfM pipeline.

### DUST3R

For a long time, SfM had no deep learning in its state-of-the-art algorithms. This has changed very recently with [DUST3R](https://arxiv.org/abs/2312.14132). Essentially the idea is to solve 3D reconstruction by framing it as a supervised learning problem: collect huge datasets with classical algorithms and then feed it through a massive transformer architecture.

## View Synthesis

> What do things look like?

### Graphics

From your [[👾Computer Graphics Portal|graphics]] class, e.g. the rendering equation, BRDFs, and ray tracing.

### Lightfield Arrays

Have many synchronized cameras to capture a scene from many angles simultaneously. Can make cool slow-motion scenes for films.

Nowadays, we see the impact in smartphone cameras, which use multiple cameras so you can use algorithmic improvements to make photos look better.

### Neural Fields

Recall this stuff from [[Architectures for Grids#Neural Fields|deep learning]], i.e. the SIREN model and NeRFs (which allow you to, given many views of a scene, render new views of it).

More recently, the trend is Gaussian splatting, which you should remember from graphics and work in robotics.

### Generative Modeling

Also recall from deep learning: [[Variational Autoencoders|VAEs]], [[Deep Generative Models#Generative-Adversarial Networks (GANs)|GANs]], and [[Deep Generative Models#Diffusion Models|diffusion]]. More recently, we can [generate video](https://arxiv.org/abs/2203.09481) too (e.g. SORA). 

## So What is Computer Vision?

In the past, computer vision was mostly framed as solving a collection of tasks. The hype has shifted across the years to various tasks such as image segmentation, video generation, 3D reconstruction, novel view synthesis, and point tracking.

However, maybe we want a more cohesive definition.

> [!idea]
> Vision is about modeling the world and yourself within it.

Vision is the ability for an agent to build a dynamic model of its environment by perceiving it through its senses. Vision is tightly linked to planning and intelligence.

---

**Next:** [[]]