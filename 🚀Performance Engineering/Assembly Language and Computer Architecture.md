gYour `C` source code is preprocessed, compiled, assembled, and then linked into machine code. This is then interpreted by the hardware when executed.

These can be manually invoked with `clang` using the flags: `-E`, `-S`, `-c`, and then `ld`. The intermediate files are `.c`, `.i`, `.s`, and then `.o`, which are linked.

==Assembly language== is a convenient symbolic representation of machine code.

Why look at assembly? Well, you can get a sense of what the compiler is actually doing. This lets you ensure your optimizations are working, or to find bugs. You can also manually modify the assembly (hopefully not necessary in modern day).

## The Instruction Set Architecture

```x86-64
movq 
```

