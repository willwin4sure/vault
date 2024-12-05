> Performance is all about the data, not the compute.

One part of growing up is learning that memory bandwidth restricts modern hardware far more than compute speed. In particular, in your model of computation, it is probably useful to *only consider communication and completely ignore compute*!

## Structured Data

The idea is that often, real-world data has repeated, sparse, or symmetric values. For example, if you store an hour long HD video naively, it takes 1.2 TB! But they normally are only around 1.2 GB via compression.

### Zeros and Sparsity

Exploiting sparsity is important since often a lot of data is just filled with zeros. Not only does this reduce storage, but also compute, e.g. recall [[Bentley Rules#Sparsity|sparse matrix operations using CSR]]. Modern GPU hardware also supports sparse tensor computations!

One issue is how to parallelize sparse matrix by dense matrix multiply, since it is possible that if you naively split by rows, different threads will have vastly different amounts of work. One way is to fuse all the loops and split in the middle.

What about just implementing sparse matrix by sparse matrix multiply? Turns out that the best loop order is Gustavson's `i`, `k`, `j` order, which has complexity $\mathcal{O}(nnz^{2}/n)$ (unlike, say, `i`, `j`, `k` which has $\mathcal{O}(n\cdot nnz)$).

Another example is sampled dense-dense multiply, where you compute $B\odot(CD)$, where $B$ is a sparse matrix which we element-wise product onto a dense matrix product. $CD$ can be computed with very fast prewritten kernels, but the fact that we are multiplying by a sparse $B$ means that we waste a ton of work. One solution is to only do the vector-vector computation for nonzero values in $B$, which may be faster depending on sparsity. 

### Repeated Data

Sometimes you have repeated data that isn't just composed of zeros. You've seen ways of compressing this, e.g. [[Column Stores#^736daa|RLE]] from databases. You could also use CSR with a fill value if there's only one nonzero number that is repeated almost all the time.

Custom code must be written to support operations with these data formats. RLE operations tend to be very fast for various applications.

### Graphs

A lot of very important and large datasets can be represented using a graph. We could store things with an adjacency matrix. If edges are sparse, then the matrix is also sparse. Further, if the graph is undirected, it will be symmetric.

Recall that serial BFS can be implemented with a queue. How do we do it in parallel? One way is to keep a *frontier* of all the next vertices to process; each time this will just be all nodes of some depth from the root. We will have two arrays to store frontiers; when we are processing one in parallel we will write new items to the other array, and then swap buffers when we need to move on to the next depth.

You will need a lock or use a `CAS` to prevent races between your threads (the code is rather nice; all you need to do is compare against `-1` in your visited array to see if you've visited it, and then swap in `d` if you haven't... and if you succeed you can process it).

One issue is that this is work inefficient in that you have to process every outgoing edge of your frontier, and this leads to a lot of wasted work when things repeat. An alternative is the "pull" strategy, where you instead look at all unvisited vertices in parallel and add them to the frontier if any of their neighbors are in the current frontier. The best strategy is to push if the frontier is large and pull if the frontier is small.

> [!warning]
> One issue with sparsity is that *insertions* are terribly slow, requiring $\mathcal{O}(n)$.

## Summary

Data representation matters. A lot. It affects both communication and computation cost. There are many data representation choices, and you should choose wisely since it is hard to change and retrofit data structures afterwards.

---

**Next:** [[What Compilers Can and Cannot Do]]