> Tricks to manipulate bits. Less relevant now in the days of good compilers.

This section will be presented in a long sequence of example problems.

## Problems

Signed integers are represented in two's complement (in particular, `~x + x = -1`). This is chosen since it allows the hardware to treat them identically to unsigned integers for various operations.

> [!example] ($\nu_{2}(x)$)
> Compute the largest power of `2` that divides `x`.

> [!proof]- Solution
>  `r = x & -x`. This is because `-x` flips all the bits in `x` and then adds `1`, so then the only bit that shows up in the `&` is the least significant bit set in `x`.

This also gives an efficient way to check if `x` is a power of `2` (see if it is equal to the largest power of `2` that divides `x`). There is an edge case when `x == 0`, however.

> [!example] ($\log_{2}(x)$)
> Compute `lg x`, where `x` is a power of `2`.

> [!proof]- Solution
> The solution uses ==deBruijn sequences==.
> 
> ```c
> const uint64_t deBruijn = 0x022fdd63cc95386d;
> const int convert[64] = {
>      0,  1,  2, 53,  3,  7, 54, 27,
>      4, 38, 41,  8, 34, 55, 48, 28,
>     62,  5, 39, 46, 44, 42, 22,  9,
>     24, 35, 59, 56, 49, 18, 29, 11,
>     63, 52,  6, 26, 37, 40, 33, 47,
>     61, 45, 43, 21, 23, 58, 17, 10,
>     51, 25, 36, 32, 60, 20, 57, 16,
>     50, 31, 19, 15, 30, 14, 13, 12,
> };
> r = convert[(x * deBruijn) >> 58]; 
> ```
> 
> A deBruijn sequence `s` of length $2^{k}$ is a cyclic bitstring such that every string of length `k` appears exactly once as a substring of `s`. The particular sequence in the code above must start with `k` zeros so that shifting to the left by multiplying with `x` is equivalent to the cyclic rotation supported by the deBruijn sequence.
> 
> All we do in `r` is extract the appropriate substring of length `6` and then index it through the lookup table.

> [!remark]
> In reality, you should use the built-in `__builtin_ctz(int x)`, which counts the number of trailing zeros of `x`.

> [!example] ($2^{\lceil \log_{2}(x) \rceil}$)
> Compute the smallest power of `2` that is at least `n`.

> [!proof]- Solution
> Here is some code:
> 
> ```c
> uint64_t n;
> 
> --n;
> n |= n >> 1;
> n |= n >> 2;
> n |= n >> 4;
> n |= n >> 8;
> n |= n >> 16;
> n |= n >> 32;
> ++n;
> ```
> 
> Essentially what this does is flood all the bits of `n` to the right, and then we add one to 
> get to the power of two. The decrement exists for the edge case when `n` is a power of two.

> [!example] (XOR trick)
> Swap two variables without a temporary.

> [!proof]- Solution
> This is standard:
> 
> ```c
> x = x ^ y;
> y = x ^ y;
> x = x ^ y;
> ```
> 
> This actually performs badly since it can't exploit instruction-level parallelism as it introduces a dependency. However, this idea often reappears when swapping larger groups of bits between words.

> [!example] (Branchless minimum)
> Find the minimum `r` of two integers `x` and `y`.

> [!proof]- Solution
> The trivial method is via `r = (x < y) ? x : y;`, but mispredicting the branch can be expensive. Here is the trick:
> 
> ```c
> y ^ ((x ^ y) & -(x < y));
> ```
> 
> This is because `x < y` ends up being either `0` or `1`, which will result in a mask of all `0`s or all `1`s when negated. 
> 
> No need to actually bother with this these days, compilers are very smart.

> [!example] (Merging with branchless minimum)
> Merge two sorted arrays.

> [!proof]- Solution
> Here is some code.
> 
> ```c
> static void merge(int64_t* restrict C,
> 				  int64_t* restrict A,
> 				  int64_t* restrict B,
> 				  size_t na, size_t nb) {
> 
> 	while (na > 0 && nb > 0) {  // (1)
> 		if (*A <= *B) {         // (2)
> 			*C++ = *A++; na--;
> 		} else {
> 			*C++ = *B++; nb--;
> 		}
> 	}
> 
> 	while (na > 0) {            // (3)
> 		*C++ = *A++;
> 		na--;
> 	}
> 
> 	while (nb > 0) {            // (4)
> 		*C++ = *B++;
> 		nb--;
> 	}
> }
> ```
> 
> The `restrict` keywords say that the pointers don't alias, meaning they don't overlap in memory. This allows further compiler optimization.
> 
> A few labels have been placed on lines of code with branches. Note that `(1)`, `(3)`, and `(4)` are predictable branches, so they don't need to be eliminated. Yet `(2)` can be optimized out using a branchless minimum, i.e.
> 
> ```c
> long cmp = (*A <= *B);
> long min = *B ^ ((*B ^ *A) & (-cmp));
> *C++ = min;
> 
> A +=  cmp; na -=  cmp;
> B += !cmp; na -= !cmp;
> ```

> [!remark]
> These days, compilers perform better when you index into arrays rather than doing raw pointer arithmetic. Further, compilers will be better at optimizing the above than you can.

> [!example] (Addition in $\mathbb{Z}/n\mathbb{Z}$)
> Compute `(x + y) % n` assuming that `x` and `y` are both in the interval $[0, n)$.

> [!proof]- Solution
> Similar trick to the branchless minimum. Note that division is expensive.
> 
> ```c
> z = x + y;
> r = z - (n & -(z >= n));
> ```
> 
> Once again, compilers should be smart enough to do these optimizations now.

> [!example] (Population count)
> Compute the population count of `x`, i.e. the number of set bits.

> [!proof]- Solution
> There is a compiler built-in nowadays. There is a fast divide-and-conquer technique though.

> [!question]
> How can you use the built-in pop count to determine the `lg` of a power of `2`?
## Backtracking

> [!example] (Queens problem)
> Place eight Queens in a chessboard such that no two Queens attack each other.

The basic algorithm uses recursive backtracking search by attempting to place a queen on each row. This is a simple recursive procedure, but how should we represent the board to facilitate Queen placement?

Let's make our algorithm generalizable for arbitrary board widths up to `32`. The basic idea is to just have a bitmask of length `n` for the columns and two bitmasks of length `2 * n - 1` for the diagonals.

```c
int32_t queens(uint32_t mask,
			   uint32_t down,
			   uint32_t left,
			   uint32_t right) {

	int32_t possible, place;
	int32_t count = 0;
	
	if (down == mask) return 1;
	
	for (possible = ~(down | left | right) & mask;
	     possible != 0;
	     possible &= ~place) {

		place = possible & -possible;
		count += queens(mask,
						down | place,
						((left | place) << 1) & mask,
						(right | place) >> 1);
	 }
	 
	 return count;
}
```

The root call to use this is `queens((1 << n) - 1, 0, 0, 0);`. 

---

**Next:** [[Bentley Rules]]