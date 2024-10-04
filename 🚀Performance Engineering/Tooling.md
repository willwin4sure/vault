> Notes on various tooling we use for 6.106.

## Compilation

We use `clang` for compilation of C files. For example:

```bash
clang-6106 -E -DNDEBUG preprocess.c
```

As for the flags:

* `-E` tells GCC to stop after the preprocessing stage.
* `-DNDEBUG` means to `#define` the variable `NDEBUG`.

## Make Files

Instead of running build commands manually, we have a `Makefile` with build targets. This specifies the compiler, flags, and allows various options such as `DEBUG=1` or `LOCAL=1`.

It also provides a nice `make clean` command to clean up all temporary files.

## Telerun

6.106 provides dedicated machines which we can run programs on using `telerun <command>`. This ensures more reproducible results and doesn't depend on the architecture or capacity of your own virtual machine.

## Debugging

Resolve segmentation faults.

```bash
make clean
make DEBUG=1
telerun gdb -ex run -ex bt -batch ./matrix_multiply
```

`tbassert` is a useful package for assert statements.

## Sanitizers and Valgrind Leak Check

Look for memory leaks and other bugs.

The first can sanitize both addresses and undefined behaviors.

```bash
make clean
make ASAN=1 UBSAN=1
telerun ./matrix_multiply
```

The second uses Valgrind to check for memory leaks.

```bash
make clean
make DEBUG=1
telerun valgrind --leak-check=full ./matrix_multiply -p
```

## Code Coverage and Linting

```bash
llvm-cov gcov testbed.c
```

```bash
python clint.py path-to-directory/*
```

## Perf

Performance profiler for Linux systems. First compile with `DEBUG=1` and then run

```bash
make isort DEBUG=1
telerun perf record ./isort 10000 10
perf report
```

You will need a local `perf` to read the report.

## Cachegrind

Cache and branch-prediction profiler within the Valgrind tool suite.

```bash
make sum LOCAL=1
valgrind --tool=cachegrind --cache-sim=yes --branch-sim=yes ./sum
```

This may depend on the size of the cache of your virtual machine.

