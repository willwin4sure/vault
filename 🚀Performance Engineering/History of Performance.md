## Hardware Dominance

In the past, machines were extremely slow so performance was vital. However, a la Moore's law and Dennard scaling, the capability of computers grew exponentially.

> [!idea]
> In this period, hardware performance ruled, rather than software performance. Why write good programs when you can just wait a couple years?

> Premature optimization is the root of all evil.
> — Donald Knuth

Knuth was actually *defending* software performance optimization, but only doing the practice of doing so *late* in the process of development, so you know where the hot paths are.

## Multicore Computing

Eventually, the power of a single core stopped improving. The silicon lattice constant is around 0.543 nanometers, and transistors will likely stop scaling around 5 nanometers.

Now, manufacturers have turned to multicore designs. This involves complex cache hierarchies and wide vector units. There are even more customized accelerators such as GPUs and FPGAs.

However, this requires *rewriting* of software. In this class, we will learn the principles and practice of writing fast code.

---

**Next:** [[Matrix Multiplication]]