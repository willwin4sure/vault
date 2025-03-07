==Linear image processing== is a fundamental toolbox for image editing and understanding. It includes techniques like:

* Edge detection.
* Blurring.
* Frequency analysis.
* Scale spaces.

## Images as Functions

> [!definition]
> An ==image== is a function $f: \mathscr{R}\to \mathbb{R}^{3}$, that maps rays (origin + direction) $\mathbf{r}\in \mathscr{R}$ to colors $\mathbf{c}\in \mathbb{R}^{3}$.

Of course, you can also identify rays $\mathscr{R}$ with 2D coordinates in $\mathbb{R}^{2}$ via pixel coordinates, assuming some image plane. We view $f$ as some continuous function, but it obviously has to be discretized, which you can then flatten into some vector
$$
\mathbf{f} \in \mathbb{R}^{H\times W\times C}.
$$
Nevertheless, you should think of images as *functions* over pixel coordinates. Something something covector.

2D greyscale images live in an [[L2 as a Hilbert Space#^f07987|inner product space]] called $\mathcal{L}^{2}(\mathbb{R}^{2})$. Indeed, you can sum images, you can scale them, you can compute inner products, and you can compute norms.

## Function Bases for Images

If we pick a basis for this vector space, we can express any element as a weighted sum of basis functions
$$
\mathbf{f}=\sum_{k}^{}c_{k}\phi_{k}.
$$
You can also change function bases. The basis in the $\mathbb{R}^{H\times W\times C}$ representation is the ==pixel basis==, i.e. one-hot on each pixel-channel combination. 

## The Fourier Basis

Another natural basis the is ==Fourier basis==, where the coefficients are the magnitudes of certain frequencies in the signal.

If you only allow sine functions, you have an issue where different frequencies want to have different phases. But by combining sine and cosine functions, you can achieve this phase shift with just linear combinations! We call this the ==translation-steerability== of sine. Further, sine and cosine are orthogonal so they are fine for a basis.

> [!idea]
> The Fourier basis is given by: for every frequency, include a pair of a sine and a cosine!

The math is more beautiful with complex-valued signals, since e.g. the Fourier basis will diagonalize convolutions. But most things work about the same in real numbers if you consider sine and cosine together (they form an 2-dimensional eigenspaces for the Fourier operator).

In the discrete case with $N$ samples, you only need $N$ basis functions. This also assumes circular boundary conditions, which means our image is actually on the torus $\mathbb{T}^{2}$, not $\mathbb{R}^{2}$.

## Fourier Transform

After taking the Fourier transform of an image, you can plot the magnitude and phase components.

![[dft_magnitude_phase.png|center|512]]

In the magnitude plot, the central area corresponds to low frequency filters, while further out corresponds to high frequency. The area on the top and bottom corresponds to the presence of horizontal lines while the sides correspond to the presence of vertical lines.

## Linear Operators

The reason we care about bases are because they are another form of image representation, making some information about the image easily accessible. Certain operations are simpler, more efficient, or more interpretable.

In fact, any convolution is expressed nicely as a pointwise multiplication in the frequency space. For the real-valued signal case, they actually become block diagonal into 2x2 blocks for the sine and cosine components.

If you consider something like a Gaussian blur kernel, this perspective allows you to see how it sets amplitudes of higher frequencies to zero!

![[gaussian_blur_ft.png|center|512]]

This perspective also tells you that you should [use Gaussians for blurring](https://www.crisluengo.net/archives/22/), not something like a uniform kernel, since its Fourier transform behaves better.

> [!idea]
> All circulant matrices are just shift operators, and they commute with each other. All convolution matrices are just sums of shift operators, so they still commute. This means they are all mutually diagonalizable. We will see this idea in more detail next lecture, where we derive the Fourier transform from some desired steerability properties.

## Sampling and Reconstruction

Here's something you learned in IMS:

> [!theorem] (Nyquist-Shannon sampling theorem)
> When sampling a signal at discrete intervals, the sampling frequency must be at least *twice* the maximum frequency of the input signal to allow us to reconstruct the original perfectly from the sampled version.

Note that you should drop the higher frequency terms first to prevent aliasing, something you've seen in IMS and graphics, where a downsampled high frequency signal looks like a low frequency signal and causes artifacts in, e.g. tiled textures.

The sampling theorem above is why audio is sampled at over 40kHz, since human hearing tops out at around 20kHz.

## Subsampling Images

The solution to aliasing is to first kill high frequency signals with Gaussian blurs before downsampling. You've seen similar things when talking about mipmaps for graphics.

Subsampling is also useful for detection of templates across shift and scale. The ==Gaussian pyramid== simply takes and image and Gaussian blurs and downsamples it repeatedly. Then you can drag your template across all levels of the pyramid.

The ==Laplacian pyramid== has layers that consider the difference between the upsampled Gaussian pyramid level $k+1$ and the Gaussian pyramid level $k$ (basically a Laplace operator at each level, you consider the difference between each point and an average of its neighbors).

![[gaussian_laplacian_pyramids.png|center|512]]

Upsampling can be done by adding zeros and then convolving with an appropriately designed filter, e.g.
$$
\begin{bmatrix}
0.25 & 0.5 & 0.25 \\
0.5 & 1.0 & 0.5 \\
0.25 & 0.5 & 0.25
\end{bmatrix}.
$$
Note that if you double the size of the image, then this filter preserves weights of each output pixel properly (check this yourself!).

One intuitive way of viewing the Laplacian pyramid is from a frequency perspective: the higher level capture the high frequency features while the lower levels capture the low frequency features (you can imagine splitting up the Fourier transformed image into rings). This isn't exactly true, but it's nice intuition.

Do note that you can also invert the Laplacian pyramid to reconstruct the original image.

---

**Next:** [[Representation Theory in Computer Vision]]


