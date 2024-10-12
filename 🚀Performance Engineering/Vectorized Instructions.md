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

### Arithmetic and Logic Instructions

Addition is just done lane-wise:

```c
__m128i _mm_add_epi32(__mm128i a, __mm128i b) {
	for j := 0 to 3 {
		i := j * 32
		dst[i+31:i] := a[i+31:i] + b[i+31:i]
	}
	
	return dst;
}
```

Multiplication only takes the lower half of the bits per range and then expands:

```c
__m256i _mm_mul_epi32(__m256i a, __m256i b) {
	for j := 0 to 3 {
		i := j * 64
		dst[i+63:i] := a[i+31:i] * b[i+31:i]
	}
	
	return dst;
}
```

### Comparison Instructions

How do we do vectorized control flow? One way is to do the comparison to get a mask register that specifies whether you are in the computation or not (doing things not in computation is a bit wasteful, of course).

```c
__m128 _mm_cmp_ps(__m128 a, __m128 b, const int imm8) {
	switch (imm8[4:0]) {
		case 0: OP := _CMP_EQ_OQ
		case 1: OP := _CMP_LT_OS
		(...)
		case 31: OP := _CMP_TRUE_US
	}
	
	for j := 0 to 3 {
		i := j * 32
		dst[i+31:i] := (a[i+31:i] OP b[i+31:i]) ? 0xFFFFFFFF : 0
	}
	
	return dst;
}
```

This masking might be better than having to extract relevant bits into a scalar register and perform the operations there, however.

### Loads/Stores

There are both aligned and unaligned loads/stores (distinguished with an `a` or a `u`). Aligned operations used to be faster (maybe still are!).

```c
__m256 _mm256_loadu_ps(float const* mem_addr) {
	dst[255:0] := MEM[mem_addr+255:mem_addr]
}
```

### Shuffle Instructions

One such instruction is blending: you can use a mask to select from two incoming vector registers `a` and `b`.

Another is an all-to-all shuffle where for each chunk you can specify where to take the bits from, or zero it out.

Another is a pack/unpack instruction where you take the top half of the lanes of two vector registers and zip them together in alternating fashion in the destination.

### Non-SIMD Instructions

Consider [`_mm_maddubs`](https://www.felixcloutier.com/x86/pmaddubsw), a rather complicated operation like a dot product.

Or [`_mm_addsub`](https://www.felixcloutier.com/x86/addsubps), which adds the even lanes and subtracts the odd lanes (used for complex numbers?)

---

**Next:** [[Vectorizing Compilers]]