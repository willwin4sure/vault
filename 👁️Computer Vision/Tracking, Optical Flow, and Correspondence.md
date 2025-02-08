You have a video of a soccer player running around a field. Can you determine where it is at any point in time?

## Template Tracking

Suppose you have a template image that you are trying to pick out of each frame. Then you can just compute patch-wise similarities against the template to find the most likely locations. 

## Optical Flow

This is an idea developed at MIT where you imagine a dense flow field transporting every pixel in the image over time. The seminal paper introduced the Horn-Schunck algorithm, which enforces some smoothness over the flow.

One way of training optical flow models that has stood the test of time is generating synthetic data (e.g. Flying Things) of moving objects and then training a CNN on it. This generalizes well to real data, perhaps because optical flow is a low-level vision problem, and therefore [[Simulators|sim2real]] works well.