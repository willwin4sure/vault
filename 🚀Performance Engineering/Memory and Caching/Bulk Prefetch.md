Sometimes you have to contend with code that features unavoidable pointer-chasing, which has no memory-level parallelism. 

	Consider an allocator that takes in both a number of bytes, and a tag, where the allocator will try to group together allocations with the same tag. For example, in market making, you might want to tag using specific symbols:

![[market_making_tags.png|center|512]]

Now, suppose you get a new message regarding `AAPL`. Then, the idea is to ==bulk prefetch== the *entire segment for AAPL* first. This allows you do utilize the parallelism of the LFBs immediately.

This can take a trading system from 20 microseconds to 2 microseconds.

> [!idea]
> Many competitive high-frequency trading strategies take the higher-latency software and precompute a FPGA lookup table for what to do in various situations. You use the inter-packet gap time to repopulate this table; you need to be fast in this time.

---

**Return:** [[Memory-Level Parallelism#^533543]]