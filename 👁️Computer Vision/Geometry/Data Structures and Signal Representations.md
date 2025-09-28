This is about representing signals (e.g. audio, images, 3D scenes) in a way that you can reason about them using neural networks.

## 2.5D Representations

> Data that lives on a 2D grid of pixels.

For example, ==depth maps== $D:\mathbb{R}^{2}\to \mathbb{R}^{+}$, which we have already seen. These are images but with also a piece of data on each pixel saying how far away it is from the camera.

Closely related are ==normal maps== $N:\mathbb{R}^{2}\to S^{2}$ which tell us the surface normal for any pixel. Note that you could obtain the normal map by taking the spatial derivative of the depth map.

We've already discussed how you can train a neural network in supervised fashion to predict depth maps from images. You can do the same for normal maps.

> [!idea]
> Neural networks perform better at predicting surface normals than depth.

This is because you don't require scale information.

But 2.5D representations are inherently limited. For real-world applications (e.g. robotics, self-driving) you often have to reason about 3D structure that isn't visible.

## Surface Representations

> Data that lives on the surfaces of objects.

For example, ==point clouds==. Note that these throw away some order information. Maybe you no longer know what points belong to what surfaces.

You can also have ==meshes==. These are points along with triangles, used for standard computer graphics rendering. You can also achieve a low-poly look by merging triangles with a clever algorithm.

Meshes establish a much stronger notion of neighborhoods and connectivity. So you can define, e.g. a convolutional filter.

> [!question]
> Given a "watertight" mesh, determine if a point is on its inside or outside.

If the mesh isn't watertight, you need further processing. You can get an output called a ==generalized winding number== which tells you how much any point is inside or outside the mesh.

Meshes are used in graphics since rendering triangles is efficient, as ray-triangle computations are easy with barycentric coordinates! You can also do this massively in parallel using rasterization approaches.

Recall that barycentric coordinates also allow you to interpolate properties at the vertices for a smoother look (e.g. color, lighting, surface normals, texture coordinates).

However, note that surface representations are limited since you can't represent the things inside! For example, for physics maybe you care if an object is hollow or not.

## Field Parameterizations

> Data that lives everywhere in space.

So how do we go about representing objects? Here are some ideas:

1. An ==occupancy field== $\text{OCC}:\mathbb{R}^{3}\to \mathbb{R}$ that is $1$ inside objects and $0$ outside objects. It can also be $0.5$ at boundaries.
2. A ==signed distance field== $\text{SDF}:\mathbb{R}^{3}\to \mathbb{R}$ whose magnitude is the distance to the nearest boundary, and whose sign is given by whether you are inside or outside an object (positive for outside).

The signed distance function is quite nice for physics systems in games, where you might want "softer" collisions.

> [!question]
> How do you extract the 2D surface from a signed distance field?

In particular, you want to find all the places with $\text{SDF}=0$.

> [!example] (Marching cubes)
> Sample on a cube grid. If this is dense enough, all surfaces will be indicated somewhere by a change of sign between two adjacent points. Further, the signed distance tells you where the surface should be placed.
> 
> Then, marching cubes performs casework for what surface to draw inside each cube:
> 
> ![[marching_cubes.png|center|512]]

Now let's dive into more detail into how to parameterize these fields.

### Discrete Parameterizations

> Voxel grid. Optimizations like octtrees.

Like an image of pixels, discrete parameterizations are just a voxel grid of the values. To query arbitrary values, you can make the signal continuous by interpolating, e.g. trilinearly.

But this is very expensive. So there are sparser representations, e.g. ==octtrees==. 

### Neural Fields

> Overfit a neural network to map coordinates to the field.

Parameterize a field with a neural network! For example, you can parameterize a signed distance function using a small ReLU MLP that takes three real numbers and returns a real number. This MLP can be very small relative to the original scene, so it is great for compression!

However, this tends to not work very well for large scenes. It also doesn't work too well with images or audio. Sitzmann wrote a paper called ==SIREN== (discussed in [[Architectures for Grids#Neural Fields|deep learning]]) where if you replace the ReLU functions with periodic activation functions (i.e. sines), you get a better result. Now with the same number of weights you can perfectly fit various audio, images, and scenes!

> [!idea]
> Some intuition on why this might be better: with ReLU activation each input neuron only gives you a single cut (along some hyperplane) of your input domain. However, sinusoidal activation allows you to achieve many more decision boundaries, which is useful for, e.g. fitting the occupancy field of a large scene.

Another really nice property is that now all derivatives exist (unlike ReLU networks which are piecewise linear functions), and are nonzero and bounded by $1$. This is very useful for physical quantities, so you can do differential equations on them (e.g. the heat equation).

For the application to differential equations, consider taking any diffeq that you need some signal to satisfy. Then, you can just write it down as a loss function on some neural network and ask PyTorch to backprop. Using SIREN as your activation performs better for various situations.

Some more intuition: if you analyze the neural tangent kernels of ReLU v.s. SIREN networks, the NTK of the ReLU network isn't shift invariant, but it is for sinusoidal networks.

In summary:

* Storage memory does not grow with number of spatial dimensions or spatial resolution.
* Sampling is very slow. You must run a forward pass of the network each time.
* It doesn't expose locality. You can't identify a subset of parameters that corresponds to a certain region.
* It has inconvenient processing, e.g. you can't run convolutions over it.
* But it does have adaptive resolution, and you can capture high-frequency details where you need it.

### Hybrid Representations

> Mix voxel grid with a smaller neural network.

You have a coarse 3D voxel grid with a feature vector at the corners. Then, to query a point, you trilinearly interpolate the features at the eight corners of the cube you are inside, and then run this feature through a smaller NN.

> [!idea]
> At two extremes:
> 
> 1. If the NN is just the identity and your voxel grid is very fine, this is just a discrete representation. 
> 2. If the voxel grid is just the eight corners of the entire scene and the features encode the three coordinates, then the NN behaves like the neural field representation.

There are other ideas, e.g. projecting onto the three planes of the axes and then running bilinear interpolation to get three feature vectors, concatenating and pushing through a NN. You can also have multi-scale voxel grids each with features that you concatenate into a NN. You can even just add a random hash function (normally has collisions but other voxel grids will allow the NN to resolve this). See [this paper](https://arxiv.org/abs/2201.05989).

See also [ReLU fields](https://arxiv.org/abs/2205.10824). Finally, for unbounded scenes you can perform compactification, i.e. within some inner radius everything looks normal, but outside you squash infinity to an outer radius.

![[compactification.png|center|384]]

---

**Next:** [[Rendering Algorithms]]