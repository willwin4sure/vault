Vectorization exploits ==single instruction multiple data (SIMD)==. One instruction affects multiple pieces of adjacent data.

The first version of this was called multimedia extensions in the 90's. Over the years, longer and longer registers have been added. Now, we use AVX512 registers.

![[vectorization_history.png|center|256]]

When operating on vector registers, multiple ALUs perform operations elementwise in lock-step, using the same instruction and control signals. This often requires data to be contiguous.

There are some cross-lane operations such as inserting, extracting, permuting, or scatter/gather.

Recall that [[x86-64 Primer#^09df1a|registers are aliased]] in x86-64. Here is how vector registers are aliased:

![[vector_register_aliases.png|center|512]]

## Floating Point Arithmetic

While vectorization now supports integer operations, it was originally created for floating point operations. However, floats have some annoying issues!

> [!warning]
> Floating point addition and multiplication are not necessarily commutative nor associative!

This is due to precision issues. Therefore, if you try to vectorize code with floating point arithmetic, compilers are very conservative and do not change the actual output. If you enable the `-ffast-math` flag however, it allows the compiler to make such optimizations at slight precision error.

## x86 Multimedia Instructions

The general format for such instructions is `_mm<vec-width>_<op>_<scalar-ty>`. The vector width is some power of two from 64 to 512. The scalar type can one of:

* `ss`: one single precision floating point (scalar).
* `sd`: one double precision floating point (scalar).
* `ps`: packed single precision floating point (vector).
* `pd`: packed double precision floating point (vector).
* `epi8/epi16/epi32/epi64`: packed signed integers (vector).

> [!example] (Some multimedia instructions)
> 8-wide float addition is `_mm256_add_ps` while 16-wide absolute value of 16-bit ints is `_mm256_abs_epi16`.

