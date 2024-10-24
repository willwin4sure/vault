> Piecewise polynomial curves that allow design of smooth shapes using few control points.

Freya Holmér has a [great video](https://www.youtube.com/watch?v=jvPPXbo87ds) on the topic.

## Basic Motivation

Computer are great at rendering triangles and lines. But piecewise linear functions are terrible for design: you have to manually drag every point around!

Instead, we should specify a parametric curve $\boldsymbol{\gamma}(t)=(\gamma_{x}(t),\gamma_{y}(t))$ and then slice it up into segments afterwards. Parametric surfaces can be similarly chopped up into triangles.

The way we choose to represent curves is a trade-off between two design goals:
* **Expressiveness:** ability to approximate most shapes,
* **Simplicity**: ease to deal with computationally and in UIs.

Splines are a great balance that are used in 2D illustration, fonts, 3D modeling, and animation! The user will specify some small set of control points and then the spline will interpolate between them!

## Basis Functions

Splines are coordinate-wise piecewise polynomial in $t$. 