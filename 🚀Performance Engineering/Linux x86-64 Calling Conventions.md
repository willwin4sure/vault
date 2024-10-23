> Allows functions to coordinate across object files. The stack has a regulated usage patterns and registers are stored by the appropriate person.
## Program Layout

First, we need to talk about the layout of a program in its [[Virtual Memory|virtual memory]] space.

![[program_vmem_layout.png|center|384]]

The operating systems organizes virtual addresses into ==segments==. For example, the code is stored in the `text` segment and static data is stored in `bss` and `data`. Finally, there is a `stack` segment that grows downward and a `heap` segment that grows upward. These never practically never collide since virtual address space is huge.

What data is stored on the stack?

1. **Return addresses** of functions calls.
2. **Register state** so different functions can use the same registers.
3. **Function arguments** and **local variables** that don't fit in registers.

## Linux x86-64 Calling Convention

> [!problem]
> How do functions in *different* object files (compiled separately) coordinate their use of the stack and of the registers, when linked together?

The answer is via a ==calling convention==. The Linux x86-64 calling convention organizes the stack into ==frames==.

The ==base pointer== `%rbp` points to the *top* of the current stack frame, while the ==stack pointer== `%rsp` points to the *bottom* of the current stack frame. Recall that the stack segment grows *down*, towards lower virtual memory addresses. 

> [!example] (Managing return addresses)
> The `call` and `ret` instructions manage return addresses using the stack and the instruction pointer `%rip`, by pushing/popping it on/off the stack.

> [!example] (Managing registers across calls)
> Some registers are ==caller-saved registers==, while others are ==callee-saved registers==. Both might induce wasted work. A few registers are designated to be callee-saved, while the rest are caller-saved.

Here are the designated roles for various general purpose registers in x86-64:

![[linkage_x86_gprs.png|center|512]]

The important ones are:

* `%rax` for return values.
* `%rdi` for the first integer argument.
* `%rbp` for the base pointer, `%rsp` for the stack pointer.

Here is what the stack might look like when `A` calls `B` which calls `C`:

![[stack_state.png|center|192]]

---

**Return:** [[C to Assembly#^527466]]