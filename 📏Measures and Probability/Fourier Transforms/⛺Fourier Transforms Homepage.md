#MIT #math #probability #measure #func-analysis 

*From the [[📏Measures and Probability Portal]].*

In this section, we learn about the beautiful Fourier transform, fundamental to analysis, probability, physics, signal/image processing, and music.

In probability, Fourier transforms are particularly important: they show up as *characteristic functions* for random variables. Since adding independent random variables is equivalent to convolving their densities, the Fourier transform allows us to convert this into direct pointwise multiplication.

## Main Sequence

First, learn about a broad overview of the Fourier transform, introducing many definitions and theorems. Fourier transforms allow us to turn convolutions into pointwise multiplication.

1. [[Fourier Transform Definitions]]
2. [[Convolutions]]

Then, in order to prove several important theorems, we will need to specialize to the case of Gaussians. We prove nice properties about Gaussians (they Fourier transform into other Gaussians), and then show that functions can be well-approximated by convolving them with tighter and tighter Gaussians. This allows us to prove inversion in the general case.

3. [[Gaussian Fourier Transforms]]
4. [[Gaussian Convolutions]]
5. [[Uniqueness and Inversion]]

Afterwards, we define the leveled up Fourier transform that gives us a Hilbert automorphism on all of $\mathcal{L}^{2}$ (it's a unitary operator!).

6. [[Fourier Transform in L2]]

Finally, we examine an application to probability through a concept called weak convergence. 

7. [[Weak Convergence and Characteristic Functions]]

---

**Next:** [[⛺Ergodic Theory Homepage]]