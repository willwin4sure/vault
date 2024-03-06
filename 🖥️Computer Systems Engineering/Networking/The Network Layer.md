This section covers the network layer, which is responsible for naming, addressing, and most importantly, **routing**.

## Routing Protocol Goals

> [!definition] (Routing protocol)
> The goal of a ==routing protocol== is to allow each switch to know, for every node `dst` in the network, a **minimum-cost** route to `dst`.

Every link can have a nonnegative cost associated with it that represents the time it takes to send a packet across. This could depend on the length of the cable, the amount of congestion, etc. These do not need to obey the triangle inequality. 

> [!idea]
> One of the most important issues we need to tackle is the fact that costs can be **dynamic**: they might change over time based on usage patterns, etc.

A ==routing table== can encode all the information a switch needs to know: for every destination `dst`, it tells the switch what the next edge to send the packet along should be. This *next step* is called the ==route==, while the full trajectory to the destination is called the ==path==. 

## Distributed Routing

You can also do centralized routing, but this isn't a great system, as it introduces a single point of failure and also scales badly.

1. Nodes learn about their neighbors via the ==`HELLO` protocol==. 
2. Nodes learn about other reachable nodes via packets called ==advertisements==.
3. Nodes ==integrate== the advertisements to determine the minimum-cost routes.

All of these steps happen **periodically**, which allows the routing protocol to detect and respond to failures, and adapt to other changes in the network. For example, `HELLO` packets get sent every few seconds or so to make sure your neighbors still exist. The other two periods can be much longer.

The exact details about advertisement and integration are unique to each distributed routing protocol. Now we will discuss some examples.

## Link-State Routing

> [!definition] (Link-state routing)
> The goal of ==link-state routing== is to disseminate information about the full topology of the graph so that nodes can run a shortest-path algorithm.

* Each advertisement contains a node's link costs to each of its neighbors. 
* Every other node gets the advertisement, via flooding. Each advertisement can have an ID so nodes can make sure they don't re-forward. Obviously they don't send anything back either.
* Now that every node knows the full graph, they can integrate by running Dijkstra's.

The flooding protocol makes link-state routing very resilient to failure.

Unfortunately, the main downside of link-state routing is that it does not scale, due to the massive **overhead** of flooding: there is far too much traffic on the network. It can run on a network the size of MIT's, but definitely not the full internet.

## Distance-Vector Routing

> [!definition] (Distance-vector routing)
> The goal of ==distance-vector routing== is to disseminate information about the current min costs to each node, rather than the actual topology.

* Each advertisement contains that node's current costs to every node it is aware of.
* The advertisements are only sent to the neighbors and are not forwarded any further.
* Integration is done by just taking the minimum of everything you see. If you see that the current route you are using has increased the cost to a destination, you need to increase your cost.

> [!remark]
> This behaves in a similar way to Bellman-Ford. Any min-cost route will eventually be found since it will propagate in a number of iterations equal to its length.

Failures can be complicated because of timing. A switch can claim it has a route to somewhere, but it might lose that connection. However, it already told everyone else that it has a route, and they will believe it! 

Here is an example:

![[countinginf.png|center|512]]

INFINITY is a relatively small constant here (maybe 32). Therefore, these updates will terminate in a finite amount of time, but it will certainly take a while.

One new strategy is called ==split horizon==, which says to not send advertisements about a route to the node providing the route, i.e. A shouldn't tell B that it has a route to C, because it is going through B. This will entirely fix the issue above.

Another idea you could've had is to have B propagate the fact that it disconnected from C immediately to its neighbors.

However, you can still have issues even if you implement **both** of the ideas above! Here is an example using a more complicated network topology.

![[countinginf2.png|center|512]]

> [!warning]
> Neither of the algorithms discussed above can scale to the size of the internet. They also cannot support policy routing!

For example, INFINITY is rather small above. If any actual route has a cost above that value, then it will think it is disconnected.

---

**Next:** [[Border Gateway Protocol (BGP)]]