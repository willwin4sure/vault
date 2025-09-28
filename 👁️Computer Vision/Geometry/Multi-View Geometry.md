Given multiple views of a scene, infer details about its 3D structure.

See [MASt3R-SLAM](https://edexheim.github.io/mast3r-slam/) for some motivation. This is a very recent paper which exemplifies many important problems in multi-view geometry.

## Triangulation

Suppose you see the same point in two images from different points of view. How can you determine its location in 3D space?

You might know its the same point from [[Point Tracking, Scene Flow, and Feature Matching#Point Tracking|tracking it across frames of a video]] or from [[Point Tracking, Scene Flow, and Feature Matching#Feature Matching|matching SIFT features]].

For now, we will assume that we *know* the camera poses that took the images, i.e. $\mathbf{P}_{1}$ and $\mathbf{P}_{2}$.

### The Infinitesimal Perspective

Here you have access to infinitesimal movements, i.e. you can get a continuous signal.

Suppose you have a scene where every 3D point has a fixed color, regardless of viewing angle (we are ignoring specular effects at the moment). Then, you can construct something called the ==epipolar plane image==. 

![[epipolar_plane_image.png|center|512]]

Essentially, you cut a plane and consider varying two parameters $s$ and $t$ that control the viewing ray. Then you can plot a 2D image that is the color you see as a function of $(s,t)$. One way of reading the image is to note that horizontal slices correspond to what you would see if you were standing at a certain point along $\mathbf{a}(s)$. 

Now, for any fixed point $p$ on the scene, all viewing rays will lie on a line in this plane image! This is due to similar triangles; the points move with a linear relationship to each other.

> [!claim]
> The *slope* of this line encodes the distance of the point $p$ to the camera: shallower slopes correspond to closer points.

In the limit, a point on the $\mathbf{a}(s)$ line could only be seen at a single value of $s$, but any value of $t$. This is the same intuition for parallax: when you are driving down the road, closer objects appear to move more quickly in your field of view than further objects.

So taking the gradient of the epipolar plane image $\mathbf{c}(s,t)$ will give you the depths. You can actually do this by training a neural network on rays of light: see Sitzmann's paper on [Light Field Networks](https://arxiv.org/abs/2106.02634).

### The Finite Perspective

Okay, but usually you will only have a finite set of images. We'll focus on the case where you just have two images, e.g. your eyes in the real world. This is the focus of **epipolar geometry**.

![[triangulation_backprojection.png|center|512]]

In principle the problem is very easy. Since you know the poses, you can just backproject the two points in the image back onto lines, and then intersect them. Since you won't have a perfect match, do this for a lot of points and formulate a least-squares problem.

## Epipolar Geometry

Here is the picture you care about:

![[epipolar_geometry.png|center|512]]

> [!definition] (Epipolar line)
> The ==epipolar line== corresponds to the set of all points that a point seen at $\mathbf{X}$ by one camera *could be* on the other camera. This is because even if you see a point from one perspective, you don't know what depth it is at.

The rest of the definitions should be intuitive. Note that the epipolar line always passes through the epipole, i.e. the projection of the other camera. Epipolar lines appear all the time, even in modern papers.

How do we compute epipolar lines? One hacky way is to just project the camera origin and some random point to the other camera plane. But this is somewhat awkward.

### The Fundamental Matrix

To get a nicer expression for epipolar lines, we can use homogeneous coordinates. Recall that homogenous 2D points are represented in the form $\mathbf{x}=\begin{bmatrix}x & y & 1\end{bmatrix}^{T}$, which means that a homogenous 2D lines can be easily represented via $\mathbf{l}=\begin{bmatrix}a & b & c\end{bmatrix}^{T}$, meaning the line $ax+by+c=0$, i.e. $\mathbf{p}^{T}\mathbf{x}=0$.

Now, it turns out that points on one camera plane can be converted to lines on the other camera plane using a single matrix transformation:
$$
\mathbf{F}\mathbf{x}_{1}=\mathbf{l}_{2},
$$
where $\mathbf{F}$ is called the ==fundamental matrix==.

> [!theorem] (Fundamental matrix relationship)
> Equivalently, there exists some matrix $\mathbf{F}$ such that
> $$
> \mathbf{x}_{2}^{T}F\mathbf{x}_{1}=0
> $$
> if $\mathbf{x}_{1}$ and $\mathbf{x}_{2}$ are the projections of a single point $\mathbf{X}$.  

Now we will go over the derivation.

Given a point in the image plane $\mathbf{x}_{1}=\begin{bmatrix}x & y & 1\end{bmatrix}^{T}$, recall from [[Image Formation#Homogenous Coordinates|the image formation section]] that we can get the local ray direction via $\overline{\mathbf{x}}_{1}=\mathbf{K}^{-1}\mathbf{x}_{1}$, using the intrinsic parameters of the camera.

![[local_ray_relationship.png|center|512]]

Then, since we know the poses of the two cameras, if their relationship is via a rotation $\mathbf{R}$ and translation $\mathbf{t}$, then
$$
\overline{\mathbf{x}}_{2}=\mathbf{R}(\overline{\mathbf{x}}_{1}-\mathbf{t}).
$$
Further, we know that
$$
(\overline{\mathbf{x}}_{1}^{T}-\mathbf{t})^{T}(\mathbf{t}\times  \overline{\mathbf{x}}_{1})=0.
$$
Therefore,
$$
(\overline{\mathbf{x}}_{2}^{T}\mathbf{R})([\mathbf{t}]_{\times}\overline{\mathbf{x}}_{1})=0,
$$
where we have replaced the cross product with a matrix-vector product with the cross product matrix
$$
[\mathbf{t}]_{\times}=\begin{bmatrix}
0 & -t_{3} & t_{2} \\
t_{3} & 0 & -t_{1} \\
-t_{2} & t_{1} & 0
\end{bmatrix}.
$$
You can eventually rewrite the expression above as
$$
\mathbf{x}_{2}^{T}\underbrace{\mathbf{K_{2}}^{-T}(\mathbf{R}[\mathbf{t}]_{\times})\mathbf{K}_{1}^{-1}}_{\mathbf{F}}\mathbf{x_{1}}=0,
$$
as desired.

### Deriving Camera Poses

This has a nice application to determining the camera poses if you don't know them, and only have correspondences between the two images!

> [!example] (The Eight-Point Algorithm)
> You can solve for the matrix $\mathbf{F}$ via the equations $\mathbf{x}_{2}^{T}\mathbf{F}\mathbf{x}_{1}=0$ for each correspondence pair $(\mathbf{x}_{1},\mathbf{x}_{2})$. You need eight points to do this.
> 
> In particular, you can rewrite the problem into $\mathbf{w}\mathbf{f}=0$ where $\mathbf{w}$ is some function of the correspondence pair and $\mathbf{f}$ is a flattened version of $\mathbf{F}$. Stack the eight points into a matrix $\mathbf{W}$ and solve $\mathbf{W}\mathbf{f}=\mathbf{0}$ using homogenous least-squares (restrict $\| \mathbf{f} \|=1$).

You can also do a [[Point Tracking, Scene Flow, and Feature Matching#^39a9e3|RANSAC]] algorithm (with samples of eight points) to filter out outlier correspondences (something is an inlier if the point lies on the computed epipolar line).

Unfortunately, backing out $\mathbf{F}$ doesn't finish, since it is a weird combination of the camera intrinsics, and the element of $\text{SE}(3)$. If the $\mathbf{K}_{i}$ are known, we can backout $\mathbf{R}$ and $\mathbf{t}$. If not, we can exploit the fact that $\mathbf{K}$ has rank two by using SVD and throwing out the smallest eigenvalue.

## Many Views

The classic method is via something called ==bundle adjustment==. Suppose you have a lot of images of the same scene. Collect SIFT features for all of them. Randomly sample two images, and then estimate the fundamental matrix (rejecting outliers). Then you can write one big optimization problem to jointly optimize all camera parameters and the 3D point cloud.

==COLMAP== is a later algorithm for SfM that significantly improves accuracy and robustness. It features a second multi-view stereo stage to obtain dense geometry. ^c99965

![[sfm_pipeline.png|center|512]]

SfM pipelines work, but they are slow, not fully reliable, and complicated. What can our modern era of deep learning say about this?

[DUST3R](https://arxiv.org/abs/2312.14132) addresses this by framing the problem as a supervised learning problem. Here's the architecture:

![[dust3r_architecture.png|center|512]]

We encode a pair of images using a ViT and push both streams of tokens through decoders (which can see each other) to output pointmaps in the local frames of each camera. These were trained by running COLMAP on large datasets to generate supervised learning data.

Then once you have these predictions you can back out the poses and intrinsics. You can merge multiple views using a similar joint optimization method.

Here's an example of a problem where DUST3R has an edge over COLMAP.

![[dust3r_over_colmap.png|center|256]]

The problem with running a classical algorithm like COLMAP on this image is that it will immediately find a lot of false correspondences from the sign at the top and then die. 

Why can DUST3R work here? Isn't its training data constructed from COLMAP too? Well, you could imagine in its training set you would have had similar situations but with many, many perspectives of the sign, in which case COLMAP can figure out the right thing. Then the deep neural network can be trained in the correct way to handle the case above correctly (it learns something about how signs look in general)!

A follow-up work called [MASt3R](https://arxiv.org/abs/2406.09756) had another head that outputs feature matching correspondences.

## Summary

The classical techniques are often still state-of-the-art, but you can go really far by collecting a large supervised dataset and training a large transformer on it. The exact details of doing deep learning do matter a good amount, but it should still be okay even if you get things wrong.

---

**Next:** [[Differentiable Rendering]]