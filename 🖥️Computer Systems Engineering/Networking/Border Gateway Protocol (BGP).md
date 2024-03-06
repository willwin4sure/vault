Last time, we discussed the [[The Network Layer#Link-State Routing|link-state routing]] and [[The Network Layer#Distance-Vector Routing|distance-vector routing]] network protocols. However, these protocols cannot scale to the size of the entire internet, which is what the ==Border Gateway Protocol== will allows us to do.

As expected, the solution to scaling to the entire internet is using a *hierarchical system*. 

> [!definition] (Autonomous system)
> Local networks that are managed by a single entity or organization (e.g. MIT's network) are referred to as ==autonomous systems==.

Here are some properties:

1. **Hierarchical:** BGP is a protocol that handles routing between autonomous systems. 
2. **Path-vector routing:** This is similar to distance-vector routing, but the advertisements also include the path, to better detect routing loops.
3. **Topological addressing:** We assign addresses in contiguous blocks to make advertisements smaller. 

> [!warning] (BGP is routing meets capitalism)
> BGP becomes complicated because network providers want to make money from customers paying for their service.

