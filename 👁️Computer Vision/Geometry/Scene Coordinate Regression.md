> This is a guest lecture by Eric Brachmann, a researcher at Niantic (known for Pokemon Go) on the cutting-edge of this field.

This is a technique that reimagines [[Multi-View Geometry|SfM]] without image-to-image matching.

> [!problem] (Camera relocalization)
> Given an image of a (known) scene, determine the pose of the camera that took the image.

A scene may be known since there are existing images of it, with known camera poses attached (but maybe you have to do some work to infer these camera poses).

This is useful for Niantic since they are trying to build AR experiences, where you can go to a location and pull out your phone, and virtual objects will be infused into the camera feed. To do this, it is useful to determine where the phone is.

## Classical Approach

Use existing [[Multi-View Geometry|classical approaches]] to construct a point cloud of a scene. This involves [[Multi-View Geometry#Epipolar Geometry|epipolar geometry]] and [[Point Tracking, Scene Flow, and Feature Matching#Feature Matching|feature matching]] between pairs of images of the scene, and then jointly optimizing poses (think [[Multi-View Geometry#^c99965|COLMAP]]).

> [!warning]
> One main issue here is the quadratic dependence on the number of images in order to match all pairs (there are even more pairs to check, if you count all the features within each image!).
> 
> ![[quadratic_dependence_matching.png|center|512]]
> 
> Ideally, you have some pruning algorithm to decide whether to match things (e.g. leverage sequential information that your images are frames in a video and then fight drift with some loop closure techniques, or use image retrieval with top-K matching).

Now, given a new image, we want to know its pose. To do this, we use [[Point Tracking, Scene Flow, and Feature Matching#Feature Matching|feature matching]], e.g. extracting SIFT features and matching them against known images. These correspond to 3D points in the scene, and we can use [PnP](https://en.wikipedia.org/wiki/Perspective-n-Point) with [[Point Tracking, Scene Flow, and Feature Matching#^39a9e3|RANSAC]] to determine the camera pose.

## Scene Coordinate Regression (SCR)

> Encode a scene with a NN that predicts global scene coordinates from pixels.

The idea is to avoid the quadratic dependence by directly optimizing 2D to 3D correspondences, rather than 2D to 2D correspondences within pairs of images.

For each individual scene, we will represent it using a neural network. This neural network is trained on images of the scene with known camera poses, and must predict for each pixel which 3D scene coordinate (in the global frame) that it corresponds to.

![[scr_relocalization.png|center|512]]

At a patch level, this actually looks a lot like classical [[Point Tracking, Scene Flow, and Feature Matching#Feature Matching|feature matching]]. You have to take in a patch and identify unique and invariant features about that area of the scene, so that you can predict the same scene coordinates regardless of the camera angle.

Now with a new query image, you push it through the network and then use RANSAC + PnP again.

> [!question]
> How do we train the SCR network?

There are two cases:

1. You have RGB-D images or you have a 3D scene. Then you can supervise directly on the ground truth scene coordinates.
2. You only have RGB images. Then, you can actually proceed with a ==reprojection loss==, meaning you use your known camera pose to reproject your scene coordinate prediction back into the image, and see how far it is from the actual pixel.

> [!warning]
> The network could be degenerate, and just predict scene coordinates that are a constant depth away from each camera pose. This would be 0 loss, and massively incorrect.

However, it turns out if you just train the network, it doesn't actually do this empirically! This is probably because it's actually more efficient to just learn the scene, rather than overfit to matching each image to its camera pose and then outputting the degenerate scene coordinates.

The main issue with this approach is that the training takes a very long time.

## Accelerated Coordinate Encoding (ACE)

> Speeding up SCR training.

Traditionally, you would train your network by repeatedly computing the reprojection loss on a full image and then performing a gradient update. However, these updates are very coarse and correlated (since neighboring pixels will have very similar losses and gradient updates). This leads to very shaky training.

Here is the CNN architecture for SCR prediction:

![[scr_architecture.png|center|512]]

Note that the MLP regression head does not require spatial context: it can be run per feature of each pixel. It turns out that it is more efficient to just have a pretrained CNN feature backbone (think of this as some smart pretrained model that behaves like extracting [[Point Tracking, Scene Flow, and Feature Matching#Feature Matching|SIFT-like features]]), and then you can train an MLP regression head per scene.

Since the pretrained CNN feature backbone is frozen, we can pass all images through it a single time. Then, we can decorrelate our gradient updates by shuffling the pixel-wise features (each pixel will have to be attached to the camera pose of the image it came from still, for proper reprojection loss computation) so that each gradient update is smoother.

This lets you aggressively increase the learning rate. You are also training a smaller MLP regression model. This lets you go from mapping a scene in 15 hours to 5 minutes.

Note the pretraining of the CNN feature backbone was done by jointly optimizing on 100 scenes with the same backbone and 100 different MLP heads, with the slow image-wise training method. This took a long time but only has to be performed once.

> [!idea]
> The ideal CNN backbone feature would be a dense feature (unlike SIFT) that changes smoothly as you slide the patch over the image, but be invariant to any camera movement for the projection. But it should change across the image.

Another issue is scalability. Usually the best fix is to train an ensemble model for different parts of the scene.

## ACE Zero

So far, we've only discussed relocalization where you have known camera poses. What if all you have is a collection of images without poses?

The idea is to start with a single image and the identity pose and then use an off-the-shelf depth estimation network to get our first ground truth point cloud. Then incrementally use the existing model to predict the camera pose of the next image, and then repeatedly train a new SCP model off of it.

The important point is to identify the correct next image to pick. For example, after picking the first image, the second image should be from a relatively close viewpoint. You can check this by making sure the inlier count isn't too low when performing RANSAC + PnP.

You can also seed ACE Zero with COLMAP.