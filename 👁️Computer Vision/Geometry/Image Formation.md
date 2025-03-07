This is the process of mapping a 3D scene into a 2D representation. Think [[👾Computer Graphics Portal|graphics]]. 

> [!definition] (What is a 3D scene?)
> It is a function from points in 3D space to various properties, e.g. material, light sources, shape, color, weight, density, etc.

There is a ton of light bouncing around objects in a scene. But if you just hold up a piece of paper, it just looks diffuse (you don't get an image). This is because you're getting light from every object hitting every part of the paper.

## Pinhole Camera

This blocks most light rays so you actually get an image on the film, since only one part of the object can bounce light rays onto any one part of the film.

![[pinhole_camera.png|center|512]]

This idea has been known for centuries.

In general, you can actually model most cameras as just a pinhole camera, where you have a single center for the camera that projects onto some 2D plane.

## Perspective Projection

Say the $xy$-plane is the image plane, and the $z$-axis points into the scene. We place the origin at the pinhole camera center. Here's what it looks like:

![[projection.png|center|512]]

You've seen this sort of picture from graphics (though maybe they used $uv$ for the image plane). The math is pretty simple when things are aligned like this:

![[perspective_transformation.png|center|512]]

You can just do the similar triangles. 

> [!claim] (Preserved properties)
> * Straight lines, incidence.
> * Cross ratios.

You know this from projective geometry. Of course, angles and lengths are not preserved. Further, recall that all parallel lines in 3D intersect at the same vanishing point (for any given viewing angle).

Getting vanishing points correct is what made paintings look a lot more realistic. It's also something diffusion models struggle with.

## Homogenous Coordinates

This is the work we did in graphics to make the transformation a linear operator (right now it involves a division which is awkward).

![[homogenous_coordinates.png|center|512]]

You slap another $1$ at the end of your vector to get homogenous coordinates. These are also equivalent under rescaling.

Now you just get a single linear operator for your perspective projection:

$$
\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1/f & 0
\end{bmatrix}
\begin{bmatrix}
X \\
Y \\
Z \\
1
\end{bmatrix}
=
\begin{bmatrix}
X \\
Y \\
Z/f
\end{bmatrix},
$$
since now rescaling will give $(x,y)=(X,Y) \frac{f}{Z}$ as we desired.

The $3\times 4$ matrix is our projection matrix. Usually it is also multiplied by $f$. You can also nicely encode a shift of the center:

![[center_shift.png|center|512]]

We can actually decompose the matrix into two pieces:

$$
\begin{bmatrix}
f & 0 & p_{x} & 0 \\
0 & f & p_{y} & 0 \\
0 & 0 & 1 & 0
\end{bmatrix}
=
\underbrace{\begin{bmatrix}
f & 0 & p_{x} \\
0 & f & p_{y} \\
0 & 0 & 1
\end{bmatrix}}_{\mathbf{K}}
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0
\end{bmatrix}
$$
The $\mathbf{K}$ matrix encodes the focal length and origin shift, while the $\begin{bmatrix}\mathbf{I} \mid \mathbf{0}\end{bmatrix}$ matrix is a perspective projection assuming image plane at $z=1$ and a shared camera/world origin.

Another nice thing you can do with $\mathbf{K}$ is decouple the projection from the image size:

![[decouple_projection_size.png|center|512]]

Then, if you want to crop an image and scale it up, you can just multiply $\mathbf{K}_{\text{cam}\to \text{img}}$ by $2$.

You can also go back from pixel location + depth to a 3D location via the $\mathbf{K}$ matrix, since
$$
\begin{bmatrix}
X \\
Y \\
Z
\end{bmatrix}
=
Z \cdot \mathbf{K}^{-1}
\begin{bmatrix}
\frac{f\cdot X}{Z} + p_{x} \\
\frac{f\cdot Y}{Z} + p_{y} \\
1
\end{bmatrix}
=
Z \cdot \mathbf{K}^{-1}
\begin{bmatrix}
x_{\text{pix}} \\
y_{\text{pix}} \\
1
\end{bmatrix}.
$$
## Camera Parameters

Now we need to model what happens when the camera and world coordinates aren't aligned. Here's what happens when you translate and rotate your camera by an element of $\text{SE}(3)$.

![[camera_parameters.png|center|512]]

You've seen these 4-by-4 matrices in graphics for changing coordinates. They encode a 3-by–3 rotation matrix in the top left and a 3-by-1 translation in the top right. We call the matrix
$$
\begin{bmatrix}
\mathbf{R} & -\mathbf{R}\mathbf{t} \\
\mathbf{0} & 1
\end{bmatrix}
$$
the World2Cam matrix and its inverse
$$
\begin{bmatrix}
\mathbf{R}^{T} & \mathbf{t} \\
\mathbf{0} & 1
\end{bmatrix}
$$the Cam2World matrix. For these matrices, it is useful to think about where the origin $\begin{bmatrix}0 & 0 & 0 & 1\end{bmatrix}^{T}$ and unit vectors $\begin{bmatrix}1&0&0&0\end{bmatrix}^{T}$ get mapped to.

## Camera Matrix

Now you can combine the two equations
$$
\tilde{\mathbf{X}}_{c}=
\begin{bmatrix}
\mathbf{R} & -\mathbf{R}\mathbf{t} \\
\mathbf{0} & 1
\end{bmatrix}
\tilde{\mathbf{X}}_{w}
\quad
\text{and}
\quad
\tilde{\mathbf{x}}=
\begin{bmatrix}
f & 0 & p_{x} & 0 \\
0 & f & p_{y} & 0 \\
0 & 0 & 1 & 0
\end{bmatrix}
\tilde{\mathbf{X}}_{c}
$$
in order to get the full translation from world coordinates to pixel coordinates:
$$
\tilde{\mathbf{x}}=
\begin{bmatrix}
f & 0 & p_{x} & 0 \\
0 & f & p_{y} & 0 \\
0 & 0 & 1 & 0
\end{bmatrix}
\begin{bmatrix}
\mathbf{R} & -\mathbf{R}\mathbf{t} \\
\mathbf{0} & 1
\end{bmatrix}
\tilde{\mathbf{X}}_{w}.
$$
The left matrix encodes the ==intrinsic parameters== of the camera, while the right matrix encodes the ==extrinsic parameters== of the camera. Essentially, you first transform the world coordinates into the local frame of the camera, and then do the standard projection.

> [!warning]
> Beware of coordinate conventions that differ between various well-known systems:
> 
> ![[coordinate_conventions.png|center|512]]

## Stereo

Say you have two cameras and want to measure how far away some point is. It's just similar triangles:

![[stereo.png|center|512]]

So using a disparity map from comparing the same object in two photos (where you know how the camera moved in the 3D world), you can push it through the formula to get a depth map.

In the more general case (e.g. where your translation isn't parallel to your image plane) you can use the theory of homography (apparently).

---

**Next:** [[Linear Image Processing and Transformations]]