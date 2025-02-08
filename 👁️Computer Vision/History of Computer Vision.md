This is a brief history of computer vision.
## Overview

* 1960-1970: Blocks, edges, and model fitting
* 1970-1981: Low-level vision: stereo, flow, shape-from-x
* 1985-1988: Neural networks, backprop., self-driving
* 1990-2000: Dense stereo and multi-view stereo, MRFs
* 2000-2010: Features, descriptors, structure-from-motion
* 2010-20??: Learning, deep learning, large datasets, rapid growth

Pre-2019, it was the era of supervised learning. More recently, there has been a push towards self-supervised learning and generative modeling.

## 1957: Computational Stereo(photogrammetry)

Suppose you have two images of the same scene from slightly different locations. How would you display them to a human so that they can perceive depth?

These sorts of techniques allow you to take many images from an airplane of the ground and then reconstruct an elevation map. Gilbert Hobrough had an analog implementation of stereo image correlation.

## 1958-1962: Rosenblatt's Perceptron

A basic single feed-forward layer, together with a way of training it. Also implemented in analog fashion.

## 1963: Larry Roberts' Blocks World

Tries to reconstruct a 3D scene of blocks from images. First extracts edges from the picture, then infers the 3D structure using the topological structure of the edges. Relies on the assumption that they are working with images of blocks.

## 1966: MIT Summer Vision Project

The idea back then was that computer vision was a simple enough problem for a graduate student to solve in a summer. Obviously, it was not so simple! 

## 1969: Perceptrons Book

Minsky and Papert publish several discouraging results about perceptrons, e.g. that a single layer can't even learn an XOR function. Largely contributed to the first "AI winter".

## 1970: MIT Copy Demo

Combines vision and robotics. A robot is tasked with reconstructing a scene comprised of blocks, and has to plan robot movement to copy the arrangement. However, it only worked in ideal conditions. This highlighted the importance of robustness for low-level vision tasks.

## 1980s: Advances in ML

For example, the popularization of back-propagation and its application to neural networks. In 1989, LeCun introduced the first CNN for digit recognition.

## 1990s: Geometry

A lot of words I don't understand: fundamental matrix, normalized 8-point algorithm, RANSAC, bundle adjustment, projective structure from motion.

---

**Next:** [[Tracking, Optical Flow, and Correspondence]]
