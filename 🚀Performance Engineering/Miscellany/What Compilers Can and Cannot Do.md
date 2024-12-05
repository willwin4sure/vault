In the lecture [[C to Assembly]], we discussed the basic compiler pipeline and how C gets translated into LLVM IR into x86-64. 

In this lecture, we discuss the LLVM IR optimizer. This is actually used as the middle layer for all sorts of front ends (programming languages like C/C++ and Rust) and back ends (assembly languages like x86-64 and ARM).

## Compiler Optimizations

Here's a slide about the various compiler optimizations, adapted from a slide from the lecture on [[Bentley Rules|Bentley rules]]. 

![[compiler_optimization.png|center|512]]

Do note that compiler developers implement new optimizations over time.

---

**Next:** [[The Travelling Salesrep Problem]]