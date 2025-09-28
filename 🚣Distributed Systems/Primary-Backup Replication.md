> Associated paper: [VM-FT](https://pdos.csail.mit.edu/6.824/papers/vm-ft.pdf), though the ideas apply to later papers.

There are two main approaches:

1. State transfer. Send checkpointed state to the backup.
2. Replicated state machine. Send log of operations to the backup.

Our case study today uses a replicated state machine, which is generally more performant.

> [!question]
> What types of failures do we handle?

We deal with ==fail-stop failures==, i.e. assume that the computer is working properly but then fails immediately. This doesn't cover all types of failures, but you can do stuff like occasionally run checksums on your data and terminate if there are errors. This won't cover logic bugs, configuration errors, or malicious attackers. 

> [!question]
> What are some challenges with replication?

* Has the primary actually failed? Could just have a network partition. You don't want to end up with two primaries, called a ==split-brain syndrome==.
* How do you keep the backup in-sync with the primary? You have to apply all changes in the right order and deal with non-determinism.
* How do you know which backup to use? Which has the most recent state?

> [!question]
> What level of operations should we replicate?

One option is to replicate at the application-level, e.g. file appends or writes. This is a higher level of abstraction and requires more work. But it is done, e.g., in [[Google File System (GFS)]].

Another approach is to replicate at the machine-level. Indeed, the VM-FT case study replicates actual assembly instructions. This also means that its behavior is totally transparent, and you don't need to manually modify application-level code to achieve fault tolerance.

> [!idea]
> One key observation of VM-FT is instead of doing replication at a hardware level, e.g. running three processors in lock step with the same instructions, you can replicate via virtual machines!

## VM-FT Overview

At the boundary of the hardware and virtual machines is a ==virtual machine monitor==, or ==hypervisor==. It supervises all the virtual machines, which can run various operating systems or applications. 

VM-FT replaces the hypervisor layer. This is useful since it receives hardware interrupts before the OS. This is a powerful tool for ensuring determinism, especially when having to deal with external events.

In particular, it has to send all interrupts through a logging channel to the VM-FT on the backup machine. This allows the system to ensure that interrupts occur at the same time (i.e. between the same instructions) on the two virtual machines.

There is also a storage server sitting on the same network as the primary and backup, which coordinates failovers. If heartbeats are interrupted, the system promptly detects the failure. To avoid a split-brain problem, there is a flag (set to 0) in the storage server that is atomically [[Synchronization Without Locks#Compare and Swap Operations|CAS'ed]] into (from 0 to 1) by the primary and backup, and the single winner promotes itself. The other server kills itself. Then, a repair protocol is undergone which clones the new primary as a backup, hooks up the logging channel, and resets the flag to 0.

> [!question]
> How do the primary and backup behave as a single machine?

You can just apply the same assembly instructions in the same order (both the primary and backup have these files). However, there are a few sources of divergence:

* Non-deterministic instructions, e.g. getting the time or sampling a random number.
* Input, e.g. network packets, and timer interrupts.
* Multicore. This is disallowed by the VM-FT paper. You could imagine some extra machinery where the hypervisor layer controls the order of thread execution carefully, e.g. make sure the same thread wins any race for a lock. More recent systems do handle multicore successfully.

For interrupts, the hypervisor has to deliver the instruction point that the interrupt occurs as well as some associated data over the logging channel.

> [!idea]
> This means that the backup lags behind one message in the logging channel, so it is actually able to execute the right number of instructions before the next interrupt.

Non-deterministic instructions are handled similarly by passing through the logging channel (the hypervisor traps the non-deterministic instruction and emulates it itself). The backup has to be waiting for the message.

> [!warning] (The output rule)
> Before you can output anything to the outside world, you must ensure that all preceding messages sent to the backup were actually received by the backup.

This prevents inconsistent state from being exposed to the outside world. This means that clients won't observe old values, but they might have to resend operations.

---

**Next:** [[Linearizability]]