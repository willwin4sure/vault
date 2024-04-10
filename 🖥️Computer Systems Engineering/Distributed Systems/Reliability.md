How to build reliable systems:

1. **Identify** all possible faults; decide which ones we're going to handle.
2. **Detect/contain** faults.
3. **Handle** faults ("recover").

How to measure success:

> [!definition] (Availability)
> We write $\text{MTTF}$ for the ==mean time to failure==, and $\text{MTTR}$ for the ==mean time to repair==. Then, we write the ==availability== as $\frac{\text{MTTF}}{(\text{MTTF}+\text{MTTR})}$.

## Disk Failures

Today, we'll focus on handling **disk failures**.

The first idea is to buy a second disk, which are complete copies of each other.

Ok, obviously we want to use error-correcting codes instead. A basic example is that we could have $N$ data disks and then a parity disk which is the bitwise XOR of them, pointwise. One downside is that you always have to always update the parity disk. One way to fix this is to spread out the parity sections across the disks. This is called ==RAID==. 



