> Associated reading: [Testing Linearizability](https://anishathalye.com/testing-distributed-systems-for-linearizability/).

I don't have notes for this lecture, but linearizability itself seems relatively simple. It is a consistency model that some systems could guarantee: all operations occur in some global total order, and the linearization point must fall within the execution time of the operation.

---

**Next:** [[Raft]]