> A distributed computing programming model and associated implementation for processing large datasets through a *map* and *reduce* operation. The key contributions are the scalability and fault-tolerance of the system.

There are only two Level 11 engineers at Google: Jeffrey Dean and Sanjay Ghemawat. Together, they developed [MapReduce](https://research.google/pubs/mapreduce-simplified-data-processing-on-large-clusters/), which was used internally for many years. You've already met Ghemawat when we discussed the [[Google File System (GFS)]].

## Interface

Users just need to specify two functions:

* `map: (k1, v1) -> list((k2, v2))`,
* `reduce: (k2, list(v2)) -> list(v2)`.

All input key/value pairs `(k1, v1)` must be able to be processed in parallel to generate a list of intermediate key/value pairs `list((k2, v2))`. Then, all of these intermediate values are collated by `k2` and then reduced together into a smaller set of values (usually just zero or one).

The goal of MapReduce is to abstract away the messy details of parallelization, fault-tolerance, data distribution, and load balancing required in implementing this at scale. 

> [!example] (Word counts)
> Suppose we wanted to count the number of times each word appears in a large collection of documents. Here is a basic example of possible map and reduce functions:
> ```python
> def mp(k1: str, v1: str) -> List[Tuple[str, int]]:
> 	# k1: document name
> 	# v1: document contents
> 	return [(word, 1) for word in v1.split(" ")]
> 
> def rd(k2: str, lv2: List[int]) -> int:
> 	# k2: a word
> 	# lv2: a list of counts
> 	return sum(lv2)
> ```
> 

^46d2ae

## Implementation

This implementation targets the computing environment at Google, which is a large cluster of commodity machines and networking. Machine failures are assumed to be common, though a distributed file system is assumed which ensures availability and reliability despite unreliable hardware.

![[mapreduce.png|center|512]]

The input data is partitioned into $M$ splits, which are processed in parallel by different workers. Each worker writes the results into $R$ pieces on their local disks, based on `hash(key) % R`, i.e. you hash the intermediate key and place the key/value pair in the corresponding modulo $R$ spot. Then, $R$ reduce workers can read from these intermediate files, sort by key, and then reduce each group, resulting in $R$ output files.

There is one special controller task that assigns idle workers map and reduce tasks as well as transmitting the locations of the intermediate files. The controller also stores the state of each task and detects worker failure through a heartbeat. Worker failure results in rescheduling; note that even completed map tasks must be rescheduled since their output is on local disk (in contrast to the reduce output files, which are in some global file system).

The controller could checkpoint itself in case of failure, but we assume that this is rare. We can actually just abort the task if the controller fails.

Since the cluster holds data in the local storage of the composite machines via GFS, the MapReduce tries to ensure locality by assigning map tasks to machines with a replica (out of a typical 3 copies) of the corresponding data. This limits usage of the network bandwidth.

$M$ is chosen so that each individual task uses between 16MB and 64MB of input data, while $R$ is chosen to be a small multiple of the number of worker machines. Note that the controller must make $\mathcal{O}(M+R)$ scheduling decisions and hold $\mathcal{O}(M\cdot R)$ state.

One main issue is stragglers: some machines, for various reasons, can take orders of magnitude longer to execute! We solve this by scheduling backup executions for tasks when we are near completion, which only increases computational resources by a few percent, while decreasing runtime by up to 30%!

## Refinements

You can specify your own `groupby` function so that the partitioning into reduce tasks follows `hash(groupby(key)) % R`. This way you can ensure certain entries end up in the same output file.

You can also specify an optional combiner function to run at the end of each map task to do partial reduction of the intermediate key/value pairs. For example, for the [[MapReduce#^46d2ae|word count task]], you probably want the map task to sum up the counts for each word before passing it off to reduce.

