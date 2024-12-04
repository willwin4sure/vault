The guest lecturer is Tao B. Schardl, who is the author and chief architect of [[Cilk#^c5a16a|OpenCilk]]. Today's lecture is on how C code gets translated into [[x86-64 Primer|x86-64 assembly]].

The Clang/LLVM compiler has two main steps in the translation process:

* The Clang front-end translates C source code into a pseudoassembly called ==LLVM IR== (intermediate representation).
* The LLVM back-end translates LLVM IR to assembly.

## C to LLVM IR

Here is some basic C code for recursively computing Fibonacci numbers.

```c
int64_t fib(int64_t n) {
	if (n < 2) return n;
	return fib(n-1) + fib(n-2);
}
```

Here is the corresponding LLVM.

```llvm
define dso_local i64 @fib(i64 noundef %n) local_unnamed_addr {
entry:
	%cmp = icmp slt i64 %n, 2
	br i1 %cmp, label %return, label %if.end

if.end:
	%sub = add nsw i64 %n, -1
	%call = call i64 @fib(i64 noundef %sub)
	%sub1 = add nsw i64 %n, -2
	%call2 = call i64 @fib(i64 noundef %sub1)
	%add = add nsw i64 %call2, %call
	br label %return

return:
	%retval.0 = phi i64 [ %add, %if.end ], [ %n, %entry ]
	ret i64 %retval.0
}
```

Some things to notice:

1. Every C function corresponds to an LLVM IR function. Notice the corresponding function name `@fib`, the return type `i64` on the left and the single parameter `i64 %n`.
2. There is some more metadata around the function. For example, `define` signifies a function *definition*, not just a *declaration* (the body of the function follows). Other stuff like `dso_local`, `noundef`, and `local_unnamed_addr` can be ignored; they deal with linking and compiler optimizations.

3. The instructions in LLVM IR look like assembly instructions, but with more C-like syntax, e.g. assignment via `=`.
4. Symbols prepended with `%` are LLVM IR registers. These are **idealized**: there are infinitely many of them and each is [[The Power of Immutability|immutable]] and *never reused as an l–value*. (We call this invariant ==static single assignment form==.) We can treat them as local variables without dealing with [[Linux x86-64 Calling Conventions|calling conventions]].

5. The `br` instructions connect ==basic blocks== via control flow. Every basic block has a label (e.g. `entry:`), and must have no control flow within it.
6. The `phi` instruction allows us to select a variable based on what the preceding basic block was. This is a quirk necessary due to the static single assignment form.

You can picture these basic blocks and their connections in a ==control flow graph==:

![[llvm_control_flow_graph.png|center|384]]

This LLVM IR control flow graph will look a lot like the control flow graph of the final generated x86-64 assembly. For this Fibonacci example, they have an identical branching pattern.

![[x86_control_flow_graph.png|center|256]]

## LLVM IR to x86-64 Assembly

The compiler must perform three main tasks for final translation:

1. Select assembly instructions to implement LLVM IR instructions.
2. Allocate assembly registers to hold values in LLVM IR registers.
3. Coordinate function calls.

First, go learn about calling conventions. ^527466

1. [[Linux x86-64 Calling Conventions]]

Now, here is the final assembly for the Fibonacci example we've been following.

```x86-64
fib:
	pushq   %rbp
	movq    %rsp, %rbp
	pushq   %r14
	pushq   %rbx
	movq    %rdi, %rbx
	
	cmpq    $2, %rdi
	jl      .LBB0_2
	
	leaq    -1(%rbx), %rdi
	callq   fib
	movq    %rax, %r14
	addq    $-2, %rbx
	movq    %rbx, %rdi
	callq   fib
	movq    %rax, %rbx
	
	addq    %r14, %rbx

.LBB0_2:
	movq    %rbx, %rax
	popq    %rbx
	popq    %r14
	popq    %rbp
	retq
```

The first five lines are the function prologue in the calling convention: we push the old base pointer onto the stack and advance the new one, then push the callee-saved registers onto the stack.

`%rdi` contains the first integer argument `n`, so we compare it with the immediate `$2` and use that to jump to `.LBB0_2` if `n < 2`. Note that we've also copied `%rdi` into `%rbx`.

We see two distinct ways of computing the inductive steps:

* `leaq` *computes* the effective address `-1(%rbx)`, which is just `n-1`, and then stores the result (just the address, not what's at that address) into `%rdi` for the recursive `fib` call, whose result gets placed into `%r14`.
* `addq` directly adds the immediate `$-2` to `%rbx`, which is just `n-2`, and then stores the result into `%rdi` for the recursive `fib` call, whose result gets placed into `%rbx`.
* Then `%r14` is added into `%rbx`.

At the end, anything in `%rbx` is returned inside of `%rax`. We execute the function epilogue on the stack and return.

> [!idea]
> The compiler might be using two different ways to compute `n-1` and `n-2` in order to better utilize the hardware.

## How?

One lingering question: how does the compiler actually convert and optimize the code?

[Compiler Explorer](https://godbolt.org/) does have a good way of viewing this, through the Opt Pipeline Viewer. Each step with actual changes conveniently shows the diffs. The actual details are beyond the scope of this class, however. 

---

**Return:** [[⛺Low-Level Optimizations Homepage]]
