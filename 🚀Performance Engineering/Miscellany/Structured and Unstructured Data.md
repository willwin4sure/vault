> Performance is all about the data, not the compute.

One part of growing up is learning that memory bandwidth restricts modern hardware far more than compute speed. In particular, in your model of computation, it is probably useful to *only consider communication and completely ignore compute*!

## Arrays

Very simple, powerful, and fast data structures.

## Structured Data

The idea is that often, real-world data has repeated, sparse, or symmetric values. For example, if you store an hour long HD video naively, it takes 1.2 TB! But they normally are only around 1.2 GB.