Operating systems **enforce modularity** on a single machine. A few things need to happen:

1. Programs shouldn't be able to refer to (and corrupt) each other's **memory**.
2. Programs should be able to **communicate** with each other.
3. Programs should be able to **share a CPU** without one program halting the progress of others.

The primary technique that an OS uses to enforce modularity is called ==virtualization==. We want each program to *think* it has full access to the hardware, when it actually doesn't.

We investigate each of these topics in the following sections:

1. [[Virtual Memory]]