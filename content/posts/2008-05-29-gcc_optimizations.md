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

In this howto I suppose that you are using the standard gcc compiler available on any Unix system for C code or his brother g++ for C++ code and the standard make utility. Other compilers have similar options but can be slightly different for names or use. I haven't tried this options on Fortran code yet, but I suppose the gfortran compiler to have the same options too. Last I suppose you are using a i386 or a x86-64 family of computers. In these category enter almost all the cpu normally found on desktops and laptops.

## `-O` optimization flag

This is a make option that tells the compiler to take actions to reduce the executable size and to speed up its execution at the expense of more compilation time and resource. The default behaviour is to reduce compilation time.

There are 3 main levels of optimization. The lower level is the 1, the highest is the 3. Each level of optimization turn on all the optimization flags of the lower level and some more. There are also 2 particular levels of optimization. The `-Os` optimize for the size of the executables and is similar to the `-O2` level but disables some flags. The `-Ofast` disregard standards compliance enabling all the `-O3` optimizations plus `-fast-math` flag.

## `-mtune` and `-march` parameters

These are two options not related to make but present inside the gcc compiler. For all i386 and x86-64 cpu a whole set of `-m` options is defined. The most important is the `-mtune` option that tells the compiler for which processor optimize the binary.\\
There are many processors to select. The most common are

| Option                                     | Use with                                                                                                      |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `generic`                                  | optimized for all the most common IA32/AMD64/EM64T processors. Useful if don't know the processor to be used. |
| `native`                                   | selects the right flag determining the processor on the compiling machine.                                    |
| `i386`                                     | old i386 cpu.                                                                                                 |
| `pentiumpro`                               | PentiumPro cpu and i686 cpu if used as architecture.                                                          |
| `pentium4`                                 | Pentium4 with MMX, SSE and SSE2 instruction set.                                                              |
| `core2`                                    | Core2 with 64-bit extensions, MMX, SSE, SSE2, SSE3 and SSSE3 instruction set.                                 |
| `corei7`                                   | Core i3/i5/i7 with 64-bit extensions, MMX, SSE, SSE2, SSE3, SSSE3, SSE4.1 and SSE4.2 instruction set.         |
| `atom`                                     | Atom with 64-bit extensions, MMX, SSE, SSE2, SSE3 and SSSE3 instruction set.                                  |
| `k8`, `opteron`, `athlon64`                | K8 with x86-64, MMX, SSE, SSE2, 3DNow!, enhanced 3DNow! instruction set.                                      |
| `k8-sse3`, `opteron-sse3`, `athlon64-sse3` | As above but with SSE3 instruction set.                                                                       |

These options can be inserted in a Makefile using `CFLAGS="-mtune=corei7 -march=corei7"`\\
For more information read the GNU gcc manual.

## `-j` option

Also compiling can be done using many cores at the same time. While years ago was done only on big parallelized servers, now with the spread of multicores cpu can be done on desktops and laptops too. Choosing carefully how to compile on multicores cpu brings to improvement in compilig speed. This is not a compiler option but is a GNU Make option you can enable inside a Makefile. The use is

```bash
$ make -jn
```

where `n` is the number of gcc jobs you want to start. Usually the best improvement is when `n = (cores +1)` but you may try different numbers. Also on single core cpu use `-j2` can bring to a speed improvement. To introduce this option inside a Makefile you can use

```bash
MAKEOPTS="-j5"
```
