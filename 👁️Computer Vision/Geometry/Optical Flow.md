> Flow field giving the correspondence of pixels of one frame of video to the next.

This is one of the oldest and most fundamental vision problems. The goal is to start understanding motion in 2D video.

Some motivation: motion is a great cue to identify objects even if they are camouflaged against a background!

![[optical_flow.png|center|512]]

This is generally how optical flow is denoted, using a color map to represent the vector field.

> [!idea] (Color constancy assumption)
> Assume a pixel $(x,y,t)$ with intensity $I(x,y,t)$ has moved by $(\Delta x,\Delta y)$ during a timestep $\Delta t$, then
> $$
> I(x+\Delta x,y+\Delta y,t+\Delta t)=I(x,y,t).
> $$

## Horn and Schunck Optical Flow

We already mentioned this algorithm in the introduction. It is an optimization problem for optical flow that includes a regularization term for "reasonable" vector fields. We'll go over its derivation here.

Taylor expand
$$
I(x+\Delta x,y+\Delta y,t+\Delta t)\approx I(x,y,t)+\frac{ \partial I }{ \partial x } \Delta x+\frac{ \partial I }{ \partial y } \Delta y+\frac{ \partial I }{ \partial t } \Delta t.
$$
The color constancy assumption implies that
$$
\frac{ \partial I }{ \partial x } \frac{\Delta x}{\Delta t}+\frac{ \partial I }{ \partial y } \frac{\Delta y}{\Delta t}+\frac{ \partial I }{ \partial t } =0.
$$
If we write $\nabla I=\begin{bmatrix}\frac{ \partial I }{ \partial x } & \frac{ \partial I }{ \partial y }\end{bmatrix}^{T}$ as the gradient of $I$ and $\mu=\begin{bmatrix} \frac{\Delta x}{\Delta t} & \frac{\Delta y}{\Delta t} \end{bmatrix}^{T}$ as the velocity of the pixel...

> [!claim] (Motion constraint equation)
> $$
> \nabla I\cdot \mu=-\frac{ \partial I }{ \partial t }.
> $$

To use this for optical flow estimation, we can construct the optimization problem
$$
E=\min_{\mu}\left[ (\nabla^{T}I_{1}\mu-(I_{2}-I_{1}))^{2} \right],
$$
where $I_{1}$ and $I_{2}$ are our two images. This is a convex objective and can be solved via gradient descent or closed-form.

The raw estimate you get from this is pretty noisy. We can add a penalty term saying that two nearby flow vectors are similar:
$$
E=\min_{\mu}\left[ (\nabla^{T}I_{1}\mu-(I_{2}-I_{1}))^{2}+\alpha(\| \mu_{x} \|^{2}+\| \mu_{y} \|^{2}) \right],
$$
where $\mu_{x}$ and $\mu_{y}$ are finite differences in the $x$ and $y$ directions. This gives a much nicer image.

![[horn_schunck_algo.png|center|512]]

Here's it applied to a more modern example.

![[horn_schunck_example.png|center|512]]

One main issue is that it seems to believe that the road under the car is also moving. So certainly can be improved.

The main problems are that:

* We only assume that pixels translate. You get different equations assuming infinitesimal rotation, homography, scale, etc.
* From a gradient perspective, we assume that the motion is miniscule.

## Matching-Based Motion Estimation

Here's a naive idea: what if you just matched each pixel to the one closest in color?

![[pixel_color_matching.png|center|384]]

It doesn't work very well. One way to resolve this is to use larger patches and correlate them together. This is a very common approach in computer vision, called a correlation volume:

![[correlation_volume.png|center|512]]

The patches give smoother results, but you'll lose some granularity.

![[patch_match.png|center|512]]

But this is quite expensive! For small motions, you'd imagine that you should only have to compare patches that are close to each other, anyway.

This is the idea behind coarse-to-fine iterative estimation. Construct a [[Linear Image Processing and Transformations#Subsampling Images|Gaussian pyramid]] of the images. At the lowest resolution, you can compare all the patches together to detect large flows. At higher resolutions, you should only compare patches that are local to each other.

![[coarse_to_fine.png|center|512]]

We start from the lowest resolution. To use these results lower in the pyramid, you can first upsample the flow field that you've gotten so far and warp the initial image using this field. This factors out the large-scale motions, and the later layers can just work on predicting the residuals.

> [!warning] (Issues with matching-based optical flow)
> 1. At occlusion boundaries, you might not have any pixels to match against, since new things get revealed!
> 2. In "flat" parts of the image, it is underspecified. The movement could be in any direction!
> 3. Brightness changes ruin matching approaches. One solution is to match gradients of the image instead (edges tend to be preserved even when brightness changes).

## Supervised Optical Flow

Stick the images in a CNN and output optical flow! We've [[Various Computer Vision Topics#Optical Flow|talked about this already]]. Generate a large synthetic dataset of flying chairs against various backgrounds (with moving cameras), and train a CNN. This (somewhat surprisingly) generalizes well, likely since optical flow is a low-level vision problem.

Two common benchmarks include Sintel (already discussed) and KITTI, which is generated from LiDAR and optical cameras from driving a car around (use depth maps to estimate ground truth optical flow).

## RAFT

Not the [[Raft|distributed consensus protocol]]. [RAFT](https://arxiv.org/abs/2003.12039) stands for Recurrent All-Pairs Field Transforms for optical flow. This paper had significant improvement over the previous state-of-the-art.

![[raft_architecture.png|center|512]]

RAFT first extracts deep CNN features, and then builds a cross-correlation volume. This is done multi-scale on various resolutions. The lowest resolution will have correlation across the entire image; higher resolutions only include correlations with neighbors.

For each pixel you retrieve the neighboring costs and then flatten it as a retrieved feature vector. This might be able to tell you where some pixel has moved (which of the neighbors has high correlation).

Then the feature vector is given to an RNN (or any neural network) to predict the optical flow. To zeroth order, the RNN just has to learn these indicator functions for which neighbors have high correlation. To first order, it will combine the estimates from all the different scales to get a more robust estimate.

## Diffusion Models

See [this paper](https://arxiv.org/abs/2306.01923) for more recent results. The basic idea thrust is that all this hand-crafted structure with cost-volumes and unrolled optimizations isn't necessary.

The idea is to take your two images, stick them into a vision transformer, and denoise the optical flow. The problem is that now you need to have much better training data to avoid overfitting and/or bad outputs.

The key contributions of the paper:

1. Merge four large synthetic flow datasets for pretraining: FlyingThings3D, AutoFlow, Kubric, and TartanAir.
2. Training with real data is tricky, since it has holes (e.g. in KITTI frames you don't have flow for the sky since LiDAR doesn't go there)! The diffusion model would generate optical flow maps with holes as well. One way to fix this is to just try to patch the holes via interpolation. Or you can generate pseudoground truths by adding noise and running the denoiser for one step.

This paper outperforms RAFT! However, empirically, the problem with these models is that they are still overfit on their training distribution. The RAFT model will perform better on videos that are way out-of-distribution.

## Self-Supervised Optical Flow

How to train a neural network to predict optical flow without ground truth. You can define a loss function with regularization (like things used for classical optical flow), or train off of the classical optical flow itself.

But now you again have no way of handling things like occlusions.

---

**Next:** [[Point Tracking, Scene Flow, and Feature Matching]]