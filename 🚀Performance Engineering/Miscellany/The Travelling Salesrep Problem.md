This guest lecture is by Jon Bentley, of [[Bentley Rules|Bentley rules]] fame. He worked at Bell Labs research and invented computer science delights such as k-d trees and the [[Master's Theorem|master theorem]]. 

> [!question] (Warmup 1)
> Generate all permutations.

> [!question] (Warmup 2)
> Generate all permutations of $1$ through $9$ such that all initial substrings of length $m$ are divisible by $m$ when read in base $10$.

There's actually only one solution, `381654729`. 

## The Problem

You are given $n$ cities on a map and need to find the shortest length cycle through them. Unfortunately, this problem is NP-hard. You can't solve it exactly unless the problem is small, but at least you can try to approximate it anyhow.

In this lecture, we attack this problem together. We will use all possible computing tools and ignore a century of TSP research. This is a beautiful case study in performance engineering. We will explore how to solve impossible problems, and learn how big is "small enough".

## Algorithmic Ideas

You can start with some approximative solution using 2-opt or chained 3-opt. Then, as you generate your permutations recursively, prune aggressively by taking your current distance and adding the MST of the remaining nodes plus your two endpoints; cache this MST computation in a hash set. If you encounter a 2-opt error, prune as well.