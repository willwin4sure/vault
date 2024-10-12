Last, we discussed compiler vectorization techniques and some of the difficulties. We also noted that compilers don't always vectorize code optimally, so manual work may help. 

Another reason why manual vectorization may perform better is because compilers are often not up-to-date with the latest intrinsic instructions for Intel, or may not be well tuned for the specifics of your architecture.

In this section, we show some examples of manual vectorization.

> [!example] (Vectorizing element-wise computation)
> Vectorize this program:
> 
> ```c
> void saxpy(int n, float a, float* restrict x, float* restrict y) {
> 	for (int i = 0; i < n; ++i) {
> 		y[i] = a * x[i] + y[i];
> 	}
> }
> ```

Here is a manual vectorization by 8. Note the initial cleanup loop for non-multiples of 8 lengths.

```c
void saxpy(int n, float a, float* restrict x, float* restrict y) {
	int vec_len = 8;
	for (int i = 0; i < n % vec_len; ++i)
		y[i] += a * x[i]

	__m256 va = _mm256_set1_ps(a);
	for (int i = n % vec_len; i < n; i += vec_len) {
		__m256 vx = _mm256_loadu_ps(x + i);
		__m256 vy = _mm256_loadu_ps(y + i);
		__m256 ax = _mm256_mul_ps(va, vx);
		__m256 axpy = _mm256_add_ps(ax, vy);
		_mm256_storeu_ps(y + i, axpy);
	}
}
```

For this particular piece of code, the compiler should be able to do this. Some statistics when $n=1024$:

1. Vectorized (256-bit): 494 cycles.
2. Scalar: 3309 cycles.

So about 6.7x faster. That's a large order of magnitude.

> [!example] (Vectorizing reduction)
> Vectorize this program:
> 
> ```c
> float sum(int n, float* restrict x) {
> 	for (int i = 0; i < n; ++i)
> 		s += x[i];
> 		
> 	return s;
> }
> ```

Here is a manual vectorization:

```c
float sum(int n, float* x) {
	int vec_len = 8;
	float s = 0.0;
	for (int i = 0; i < n % vec_len; ++i)
		s += x[i];

	__m256 v = _mm256_set_ps(0., 0., 0., 0., 0., 0., 0., s);
	for (int i = n % vec_len; i < n; i += vec_len) {
		__m256 vx = _mm256_loadu_ps(x + i);
		v = _mm256_add_ps(v, vx);
	}

	return reduce_vector(v);
}
```

Here, the reduce function can be implemented as

```c
float reduce_vector(__m256 v) {
	// { v3, v2, v1, v0 }
	__m128 v_0_4 = _mm256_extractf128_ps(v, 0);

	// { v7, v6, v5, v4 }
	__m128 v_4_8 = _mm256_extractf128_ps(v, 1);

	// { v3 + v7, v2 + v6, v1 + v5, v0 + v4 }
	__m128 t = _mm_add_ps(v_0_4, v_4_8);

	// { v1 + v5, v0 + v4, v3 + v7, v2 + v6 }
	__m128 t1 = _mm_shuffle_ps(t, t, _MM_SHUFFLE(2, 3, 0, 1));

	// { _, _, v1 + v3 + v5 + v7, v0 + v2 + v4 + v6 }
	__m128 t2 = _mm_add_ps(t, t1);

	// { _, _, _, v1 + v3 + v5 + v7 }
	__m128 t3 = _mm_shuffle_ps(t2, t2, _MM_SHUFFLE(0, 0, 0, 2))

	// { _, _, _, sum vi }
	__m256 t4 = _mm_add_ps(t2, t3);

	return _mm_cvtss_f32(t4);
}
```

> [!example] (Vectorizing control flow)
> Vectorize this program:
> 
> ```c
> void foo(float* restrict x, float* restrict y) {
> 	for (int i = 0; i < 8; ++i) {
> 		if (x[i] < y[i])
> 			x[i] += y[i];
> 			
> 		else
> 			x[i] -= y[i];
> 	}
> }
> ```

Compute the mask and use it:

```c
void foo(float* restrict x, float* restrict y) {
	__m256 x_vec = _mm256_loadu_ps(x);
	__m256 y_vec = _mm256_loadu_ps(y);
	__m256 x_lt_y = _mm256_cmp_ps(x_vec, y_vec, _CMP_LT_OS);
	__m256 if_true = _mm256_add_ps(x_vec, y_vec);
	__m256 if_false = _mm256_sub_ps(x_vec, y_vec);
	__m256 result = _mm256_blendv_ps(if_true, if_false, x_lt_y);
	
	_mm256_storeu_ps(x, result);
```

---

**Next:** [[Multicore Programming]]