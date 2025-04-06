---
title: "GCC optimizations"
date: 2008-05-29
modified: 2011-08-30
tags:
  - c
  - gcc
  - optimizations
categories:
  - programming
author_profile: true
draft: false
comments: false
---

There are many optimization parameters you can use to reduce the binary size and speed up its compilation and execution. Here there are the simplest to use and probably the more effective.

In this howto is assumed the use of the standard `gcc` compiler available on any Unix system for C code, or his brother `g++` for C++ code, and the standard `make` utility. Other compilers have similar options but can be slightly different for names or use. I haven't tried this options on Fortran code yet, but I assume the `gfortran` compiler has similar options. Last, I assume a i386 or a x86-64 family of computers. In these categories enter all CPUs normally found on desktops and laptops.

## `-O` optimization flag

This is an option of `make` that tells the compiler to take actions to reduce the executable size and speed up execution at the expense of more compilation time and higher resource comsumption.

There are 3 levels of optimization. The lowest is 1, the highest is 3. Each level of optimization turns on all the optimization flags of the lower level and some more. There are also 2 additional specialized levels of optimization. The `-Os` optimizes for the size of the executable and is similar to the `-O2` level, although it disables some flags. The `-Ofast` disregards standards compliance enabling all the `-O3` optimizations plus the `-fast-math` flag.

## `-mtune` and `-march` parameters

These are two options available in the `gcc` compilera and part of a whole set of `-m` options for i386 and x86-64 CPUs. The `-mtune` option tells the compiler for which processor optimize the binary, while `-march` specifies the architecture.\
Common choices are

| Option                                     | Use with                                                                                                      |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `generic`                                  | optimized for all the most common IA32/AMD64/EM64T processors. Useful if the processor is unknown. |
| `native`                                   | selects the right flag determining the processor on the compiling machine.                                    |
| `i386`                                     | old i386 cpu.                                                                                                 |
| `pentiumpro`                               | PentiumPro cpu and i686 cpu if used as architecture.                                                          |
| `pentium4`                                 | Pentium4 with MMX, SSE and SSE2 instruction set.                                                              |
| `core2`                                    | Core2 with 64-bit extensions, MMX, SSE, SSE2, SSE3 and SSSE3 instruction set.                                 |
| `corei7`                                   | Core i3/i5/i7 with 64-bit extensions, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1 and SSE4.2 instruction set.         |
| `atom`                                     | Atom with 64-bit extensions, MMX, SSE, SSE2, SSE3 and SSSE3 instruction set.                                  |
| `k8`, `opteron`, `athlon64`                | K8 with x86-64, MMX, SSE, SSE2, 3DNow!, enhanced 3DNow! instruction set.                                      |
| `k8-sse3`, `opteron-sse3`, `athlon64-sse3` | As above but with SSE3 instruction set.                                                                       |

These options can be passed in a Makefile as `CFLAGS="-mtune=corei7 -march=corei7"`\
For more information read the GNU gcc manual.

## `-j` option

with the spread of multicores CPUs  on desktops and laptops, compiling can easily be parallelized vastly improving compilation speed. This is a GNU Make option you can enable inside a Makefile.

```bash
make -jN
```

where `N` is the number of `gcc` jobs to start. Usually, the highest improvement is found for `N = (cores +1)`. The use of `-j2` may bring speed advatages even on single core CPUs. In a Makefile use for example

```bash
MAKEOPTS="-j5"
```
