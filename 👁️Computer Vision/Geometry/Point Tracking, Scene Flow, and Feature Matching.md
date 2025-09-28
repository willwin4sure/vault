## Point Tracking

In some sense point tracking might be more natural than [[Optical Flow|optical flow]]: you want to track the trajectories of points throughout an entire video.

We've already discussed the paper that revisited this: [Particle Video Revisited](https://arxiv.org/abs/2204.04153). This is how it is designed:

![[particle_video.png|center|512]]

At a high-level: you start by sampling some trajectories for these points (maybe based on integrating optical flow, or some other heuristics), and then you extract features from the frames at those locations/times. Then you correlate this feature with multi-scale neighborhoods in the precomputed feature map (so maybe you can figure out which point it should have came from), and concatenate all this data to send into a neural network to make a residual update. This is all trained on synthetic data.

This does better than integrating RAFT-based optical flow, especially at tracking through occlusions or lighting changes.

> [!remark]
> A cool application Sitzmann brought up was helping another faculty member track mice throughout a cage, which was built on this paper with a bunch of extra domain-specific improvements.

There are issues that shows up with this approach due to the fact that you are tracking all the points individually. For example, consider a scene where a car is moving and occluding the background:

1. Points on the car, when tracked, will wiggle relative to each other. This is probably not desired behavior.
2. When the car occludes the background, it will "push" the background points along with it.

These issues can be solved by co-optimizing all the trajectories together; this is the content of the [CoTracker](https://arxiv.org/abs/2307.07635) paper. This is the go-to technique when you need motion tracking nowadays.

## Scene Flow

Like [[Optical Flow|optical flow]], but a flow field over the 3D points in a scene.

### Flow Map

We'll discuss the recent [FlowMap](https://arxiv.org/abs/2404.15259) paper out of Sitzmann's lab, which reconstructs depth and camera poses from an input video.

* RAFT and CoTracker are first used to extract the optical flow and point tracks.
* We form an optimization problem to solve for depth and camera poses.
* Now you can reconstruct point clouds, or perform downstream tasks like novel view synthesis with Gaussian splatting.

Recall from our work on [[Image Formation|image formation]] that given a depth map and camera intrinsics, we can unproject out the 3D coordinates of a point cloud. Now movement in the camera corresponds to some scene flow in the point cloud, which gets projected down into optical flow.

This gives us a loss function for our optimization problem! You can take any triple $(\mathbf{P},\mathbf{K},\mathbf{D})$ of poses, intrinsics, and depth, and then stick them through the "forward" model described above. Then, we can compare the result we end up getting with our ground truth from the optical flow or point tracking models. 

So randomly initialize $(\mathbf{P},\mathbf{K},\mathbf{D})$ and minimize it using gradient descent. Sadly this doesn't actually work very well... there's too much ambiguity in the forward model and you get stuck in local minima.

The key way of fixing this is to build a separate differentiable estimator for each component:

![[flowmap_arch.png|center|512]]

#### Pose Solver

Suppose you have depths of two frames and their optical flow. You also have the camera intrinsics. Then, you can get the scene flow as correspondence between the point clouds.

You can stick this into a differentiable least-squares solver in order to get the element of $\text{SE}(3)$ that corresponds to this point cloud mapping, i.e. get the poses.

![[pose_solve.png|center|512]]

#### Depth Neural Network

Parameterizing the depth directly as an image doesn't perform very well. For some reason, just parameterizing it as an output of a CNN makes it do better! This just acts as some regularization: patches that look about the same should have similar depth.

This can also be pretrained end-to-end on large video datasets for much faster convergence!

![[depth_nn.png|center|512]]

We won't discuss the differentiable intrinsics solver. The overall result isn't SOTA pose estimation but it is quite good for its simplicity.

### Parameterizing Scene Flow Fields

So far, we've been representing scene flow as per-point translations, i.e.
$$
\Phi:\mathbb{R}^{3}\to \mathbb{R}^{3},\quad \Phi(\mathbf{x})=\Delta \mathbf{x}.
$$
In general, points in the real world tend to move around in rigid clusters (as part of objects). This means that you probably want to also capture some rotational component if you plan on regularizing the flow field. So another option is to predict a pointwise translation *and* rotation:
$$
\Phi:\mathbb{R}^{3}\to \text{SE}(3),\quad \Phi(\mathbf{x})=\begin{bmatrix}
\mathbf{R} & \mathbf{t} \\
0 & 1
\end{bmatrix}.
$$
Now, this allows you to have the *same* prediction for *all* points within a rigid object! The rotation has to be relative to some centroid. This leads to an overparameterization but a more tractable problem.

### Multi-View Images

Now imagine instead of a video, you just have two shots of the same scene from different perspectives at different times. Now the main issue is that correspondences are no longer *local*. While this isn't an issue for robotics (you can just have a camera), it is relevant for unstructured data on the internet.

This is the subject of [Building Rome in a Day](https://grail.cs.washington.edu/rome/).

## Feature Matching

Panorama stitching was a popular problem in this field. 

![[feature_matching.png|center|512]]

In two images, you want to first find points that are identifiable (so, not the sky!), and then compute some features at each point in order to find a matching copy on the other image. 

Here's a harder example. Obviously optical flow will do nothing for this, but the out-of-the-box algorithms for feature mapping will just work!

![[stitching.png|center|512]]

> [!proof]- Answer
> The rocks in the top left of the left image match with the rocks in the bottom right of the right image.

### Invariant Local Features

We want nontrivial features of patches that are invariant to changes in the image:

* **Geometric invariance:** translation, rotation, scale.
* **Photometric invariance:** brightness, exposure, etc.

### Points of Interest

Intuitively, the goal is to find corners! Points on the sky, or points on an edge, aren't very descriptive, since there are a lot of potential matches. Corners change a lot when you wiggle your view of them.

In particular, you want to maximize your energy function in terms of a small wiggle $(u,v)$ of your window $W$, given by
$$
\begin{align}
E(u,v)&=\sum_{(x,y)\in W}^{}\left[ I(x+u,y+v)-I(x,y) \right]^{2} \\
&\approx\sum_{(x,y)\in W}^{}\left[ I_{x}u+I_{y}v \right]^{2},
\end{align}
$$
via a Taylor expansion on $I$. This is a surface locally approximated by a quadratic form
$$
E(u,v)\approx\begin{bmatrix}
u & v
\end{bmatrix}
\underbrace{\begin{bmatrix}
A & B \\
B & C
\end{bmatrix}}_{H}
\begin{bmatrix}
u \\
v
\end{bmatrix}.
$$
The property of being a corner can be encoded via some statistics of the eigenvalues of this matrix. When the minimum eigenvalue is large, that tells you there is a corner:

![[harris_ corner_detector.png|center|512]]

This is called the [Harris corner detector](https://en.wikipedia.org/wiki/Harris_corner_detector). You take image derivatives through a Gaussian filter, and then take points where both eigenvalues are strong (probably you want the points where the smaller eigenvalue is at a local maximum). Here is an example, where the points are highlighted in red.

![[harris_features_example.png|center|512]]

### Feature Descriptors

Again, we want these to be invariant to transformations. But we also what them to have good discriminability, i.e. they should be highly unique for each point.

You can find the dominant *orientation* of the image patch. One way is using the eigenvector of the larger eigenvalue for the matrix $H$ above. Another (better) way is just taking the orientation of the (smoothed) image gradient. Then rotate the patch according to this angle.

To handle intensity, you can normalize the window by subtracting the mean and dividing by the standard deviation.

For multiscale, you build a [[Linear Image Processing and Transformations#Subsampling Images|scale pyramid]] and extract features at all different scales.

> [!example] (SIFT)
> With a small patch around the detected feature, compute the edge orientation for each pixel with the gradient. Then throw out weak edges (by thresholding gradient magnitude), then create a histogram of the surviving edge orientations. Shift the bins so that the biggest one is first, and use that as your feature.

This is a really robust feature! It can handle changes in viewpoint, significant changes in illumination, and is pretty fast. There is lots of code for this available as well.

### Matching

For this part, you can just compare some norm on the features and then find the closest ones. However, the main problem you'll have to deal with is inliers v.s. outliers. Some of the matches will be correct, and others will be spurious.

![[feature_matching_outliers.png|center|512]]

Like the scene pose estimation from before, you want to fit some transformation matrix based on the correspondences (as a least-squares problem).

Unfortunately, the outliers really mess with the fit! One way to fix this is to find a line that has the most inliers as possible (where for any given line, you have some criteria for what an inlier is).

> [!example] (RANSAC)
> This is the Random Sample Consensus (RANSAC) algorithm: you randomly sample some subset of the points to fit a matrix, and then count how many of all the points are inliers. Do this a bunch of times and pick the one with the most inliers.

^39a9e3

This is *guaranteed* to converge as long as the number of outliers is at most $50\%$! There is also a guarantee on the convergence rate, which is logarithmic in the number of points.

---

**Next:** [[Multi-View Geometry]]