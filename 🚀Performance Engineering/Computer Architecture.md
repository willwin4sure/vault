Back in 6.191 you learn and build a simple 5-stage pipelined processor:

![[5stage.png|center|512]]

In this class, we will work with an Intel Haswell microarchitecture, which looks like this:

![[haswell.png|center|512]]

It's a much larger beast:

* Many more pipeline stages.
* Superscalar processing.
* Sophisticated branch prediction.
* Vector hardware.
* Out-of-order execution.

> [!idea]
> Computer architects have historically aimed to improve processor performance by two means:
> 
> * Exploit ==parallelism== by executing multiple instructions simultaneously (e.g. ILP, vectorization, multicore).
> * Exploit ==locality== to minimize data movement (e.g. caching).

## Pipelining and Hazards

As you know, processor hardware is ==pipelined== to increase throughput: intermediate registers are placed between various stages of the computation so more operations can be progressing every clock cycle.

> [!definition] (Types of hazards)
> Hazards may prevent an instruction from executing during its designated clock cycle.
> 
> * ==Structural hazards==: two instructions attempt to use the same functional unit at the same time.
> * ==Control hazards==: fetching and decoding is delayed due to branches.
> * ==Data hazards==: an instruction depends on a result of a prior instruction.

## Handling Structural Hazards

Modern processors have *multiple hardware units* to handle different operations. For example, there are separate functional units for floating point arithmetic, which take multiple cycles to complete.

We can also fetch and issue multiple instructions per cycle to keep all these units busy. This is the principle of ==superscalar== processing.

The Intel Haswell architecture actually splits up x86-64 instructions into simpler so-called ==micro-ops==, many of which are issued per cycle.

## Handling Control Hazards

You only know the result of a branch after the execute stage. But rather than stall, we can ==speculatively execute== more instructions into the pipeline. If we predict the branch correctly, we continue as usual; else, we undo the operation (haven't yet hit the commit point for these operations).

Mispredicting branches is rather expensive; on Haswell, it wastes around 15-20 cycles. Modern processors have very strong ==branch predictors== that are accurate over 95% of the time. A basic idea is for each instruction address where you see a branch, you keep a rough ==saturating counter== for how often it gets taken. Evidence suggests Haswell uses some variant of the [TAGE branch predictor](https://jilp.org/vol8/v8paper1.pdf).

## Handling Data Hazards

> [!definition] (Types of data dependence)
> Instruction `j` can depend on an earlier instruction `i` in a few ways:
> 
> * ==True dependence (RAW)==. `j` reads what `i` writes.
> * ==Anti dependence (WAR)==. `j` writes over what `i` reads.
> * ==Output dependence (WAW==; `j` writes over what `i` writes.

The data dependencies between instructions in a program. can be modeled with a ==data-flow graph==. This is exactly what you expect, with three types of directed edges, one per type of dependence.

RAW can be solved by ==bypassing==, which allows you not to wait until the end of the writeback stage.

WAR and WAW can be solved by ==register renaming== if we have some spare. We just write to a different destination than what we need to read from/write to first.

Haswell has 16 distinct integer registers but 168 physical registers it can pick between.

---

**Next:** [[Vectorized Instructions]]
