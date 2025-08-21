---
title: "Code benchmarking functions"
date: 2012-12-09
tags:
  - c
  - c++
  - python
  - fortran
  - benckmarking
categories:
  - programming
author_profile: true
draft: false
comments: false
---

Each language has its own functions to do benchmarks on the code measuring the execution time of few lines of code or a single function. Here I collected snippets of code to perform delta-time measurements. To have detailed informations about single functions refer to the official documentation.

Keep in mind that benchmarking a code is a tricky business with many possible sources of problems:

1. We are usually interested in the time spent by the CPU only on our code, not the total time the CPU used to reach the second measurement point that will be longer due to the internal scheduling in the processes (the first called CPU time, the second Wall time).
2. Each function has a resolution so we can not measure intervals as short as we want. The closer we go with our measure to the resolution the more difficult is to know if the measure is correct.
3. The measure itself takes time that is added to the total time measured.
4. Not all functions are platform-indipendent.
5. Parallelized codes require more caution. The parallelization libraries used (if any) should provide functions created on purpouse.

Common problems of the timestamp:

1. Using the timestamp of the system is very prone to errors due to adjustments the system does on it for example syncing with a NTP server.
2. In a multicore CPU the timestamp might be out-of-sync between different cores.
3. The timestamp sometimes might update at different rates depending on the processor load.

## Content

- [Content](#content)
- [Unix shell](#unix-shell)
- [Python](#python)
- [C](#c)
  - [`clock()`](#clock)
  - [`clock_gettime()`](#clock_gettime)
  - [`gettimeofday()`](#gettimeofday)
  - [`times()`](#times)
  - [`ftimes()`](#ftimes)
  - [`QueryPerformanceCounter()`](#queryperformancecounter)
  - [`rdtsc()`](#rdtsc)
- [C++](#c-1)
  - [`chrono`](#chrono)
- [Fortran](#fortran)

## Unix shell

The date command gives the current date and time but can also be used to measure time differences. However is highly discouraged since it has a terrible resolution (1 second), and use the timestamp to obtain the Wall time. Anyway it can be successfully used to benchmark long operations.

```bash
#!/bin/bash

START_TIME=$(date +'%s')
#
# Do something
#
END_TIME=$(date +'%s')

TIME_DIFF=$(( ${END_TIME}-${START_TIME} ))
echo $(date --date="@${TIME_DIFF}" +'%T')
```

In this case the output is in the human readable form `hh:mm:ss`.

The `time` unix command is designed to get execution times on a finer scale but can only be used to get the execution time of a whole script therefore is not suitable for our purposes here.

## Python

The simple way to get the execution time is

```python
import time

start_time = time.time()
#
# do something
#
elapsed = time.time() - start_time
```

This way we find the wall time using the timestamp, not the CPU time. For the CPU time use `clock()` instead

```python
from time import clock

start_time = time.clock()
#
# do something
#
elapsed = time.clock() - start_time
```

`clock()` is also reported to have a better resolution than `time()`.\\To output this number in a human readable form

```python
from datetime import timedelta

human_readable = datetime.timedelta(seconds=int(elapsed))
print 'Took: ',human_readable
```

This will output in the form `<D days, hh:mm:ss.uuuuuu>` where `uuuuuu` are microseconds.

## C

In C there are several ways to obtain time differences. Functions and structures are spread between few headers. Here an overview

| Function                    | Header        | Resolution  | Platform | Clock type    |
| --------------------------- | ------------- | ----------- | -------- | ------------- |
| `clock()`                   | `time.h`      | 10 - 50> ms | Unix/Win | CPU time      |
| `clock_gettime()`           | `time.h`      | < 1 us      | Unix/Win | CPU time      |
| `gettimeofday()`            | `sys/time.h`  | > 1 us      | Unix     | Wall time (?) |
| `times()`                   | `sys/times.h` | ?           | Unix     | ?             |
| `ftimes()`                  | `sys/timeb.h` | 1 ms        | Unix     | Wall time     |
| `QueryPerformanceCounter()` | `windows.h`   | > 1 us (?)  | Win      | ?             |

### `clock()`

Header:

```c
#include <time.h>
```

A straightforward way to have a time difference is to use the `clock()` function

```c
clock_t start = clock();
/**
 * Do something
 */
clock_t end = clock();
int seconds_elapsed = ((double)(end - start)) / CLOCKS_PER_SEC;
```

This solution is not very reliable since the resolution of `clock()` is usually above 10ms and can be as bad as 50ms. The macro `CLOCKS_PER_SEC` (defined in `time.h`) that defines the number of clock ticks per second the system generates, gives the lower limit of the resolution i.e. the better result you should get.

### `clock_gettime()`

Header:

```c
#include <time.h>
```

On Linux there is a better choice for high resolution measurements. The function `clock_gettime()` stores the time in a timespec structure with a theorical resolution of the nanosecond. As far as I know it should be able to reach the sub-microsecond resolution.\
_Note_: to use `clock_gettime()` you must link against the real-time library with `-lrt` compiling your project.

To use this function we must choose between the clock available:

| Clock                      | Type of clock                                                                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLOCK_REALTIME`           | Wall clock                                                                                                                                              |
| `CLOCK_MONOTONIC`          | Wall clock measuring real time. However its disconnected from the time stamp and cannot tick backwards nor being adjusted by the system or NTP servers. |
| `CLOCK_PROCESS_CPUTIME_ID` | CPU time relative to the process                                                                                                                        |
| `CLOCK_THREAD_CPUTIME_ID`  | CPU time relative to the thread (_Note_: not supported in Linux!)                                                                                       |

```c
struct timespec ts_start;
struct timespec ts_end;

clock_gettime(CLOCK_MONOTONIC, &ts_start);
/**
 * Do something
 */
clock_gettime(CLOCK_MONOTONIC, &ts_end);

/* Calculate the difference */
struct timeval elapsed = clockDiff(&start,&end);
```

Where a naive `clockDiff()` functions might be

```c
/* Return the difference as a timespec structure */
timespec clockDiff(const timespec *t1, const timespec *t2)
{
    struct timespec res;

    long long usec = (t2->tv_sec - t1->tv_sec)*1000000000 + (t2->tv_nsec - t1->tv_nsec);
    res.tv_sec = usec/1000000000;
    res.tv_nsec = usec%1000000000;
    
    return res;
}
 
/* Return the difference as a integer number of nanoseconds */
long long clockDiff(const timespec *t1, const timespec *t2)
{
    return (t2->tv_sec - t1->tv_sec)*1000000000LL + (t2->tv_nsec - t1->tv_nsec);
}
 
/* Another solution */
void clockDiff(const timespec *t1, const timespec *t2, timespec *res)
{
    res->tv_sec = t2->tv_sec - t1->tv_sec;
    res->tv_nsec = t2->tv_nsec - t1->tv_nsec;
    while ( res->tv_nsec < 0 )
    {
        res->tv_sec -= 1;
        res->tv_nsec += 1000000000;
    }
}
```

This functions will not work properly when t2 is before t1. For a bullet proof solution look at the `sys/time.h` official documentation.

### `gettimeofday()`

Header:

```c
#include <sys/time.h>
```

This header is present in Unix systems and in Mac, NOT in Windows. Here the function `gettimeofday()` and the structure `timeval` are defined. `timeval` has a theorical resolution of the microsecond, the same as `gettimeofday()`. `gettimeofday()` is considered deprecated since POSIX2008.

```c
struct timeval start;
struct timeval end;

gettimeofday(&start,NULL);
/**
 * Do something
 */
gettimeofday(&end,NULL);

/* Calculate the difference */
struct timeval elapsed = clockDiff(&start,&end);
```

Where a simple `clockDiff()` function would be like the previous one

```c
void clockDiff(const timeval *t1, const timeval *t2, timeval *res)
{
    res->tv_sec = t2->tv_sec - t1->tv_sec;
    res->tv_usec = t2->tv_usec - t1->tv_usec;

    while ( res->tv_usec < 0 )
    {
        res->tv_sec -= 1;
        res->tv_usec += 1000000;
    }
}
```

Simple conversion functions between `timeval` and `timespec`

```c
timeval tspec2tval( const timespec *t)
{
    timeval tval;
    tval.tv_sec = t->tv_sec;
    tval.tv_usec = t->tv_nsec / 1000;
    return tval;
}

timespec tval2tspec(const timeval *t)
{
    timespec tspec;
    tspec.tv_sec = t->tv_sec;
    tspec.tv_nsec = t->tv_usec * 1000;
    return tspec;
}
```

### `times()`

Header:

```c
#include <sys/times.h>
```

This header defines a `tms` structure usable through the `times()` function.

### `ftimes()`

Header:

```c
#include <sys/timeb.h>
```

This header define a `timeb` structure and the `ftimes()` function. This function returns the timestamp inside the `timeb` structure with the precision of the millisecond.

```c
struct timeb start;
struct timeb end;

ftime(&start);
/**
 * Do something
 */
ftime(&end);

/* Calculate the difference */
struct timeb elapsed = clockDiff(&start, &end)
```

A possible `clockDiff()` function

```c
timeb clockDiff(const timeb* start, const timeb* end)
{
    struct timeb ret;
    ret->time = end->time - start->time;
    ret->millitm = end->millitm - start->millitm;

    while ( res->millitm < 0 )
    {
        res->time -= 1;
        res->millitm += 1000;
    }
    
    return ret;
}
```

### `QueryPerformanceCounter()`

On Windows platform-dependent functions are defined in the `windows.h` header

```c
#include <windows.h>
```

_Note_: I'm not a Windows fan and I never had the necessity to program on Windows so I never used these functions or even tested them. Therefore ALWAYS check the documentation!

```c
size_t tick, start, end;
 
QueryPerformanceFrequency(&tick);
 
QueryPerformanceCounter(&start);
/**
 * Do something
 */
QueryPerformanceCounter(&end);
 
double elapsed = (end.QuadPart - start.QuadPart) / tick.QuadPart;
```

### `rdtsc()`

This solution uses inline assembly to read the timestap of the machine. The advantage is its high speed.

```c
__inline__ uint64_t rdtsc(void)
{
    uint32_t lo, hi;
    __asm__ __volatile__ ( "xorl `eax,`eax \n        cpuid"::: "%rax", "%rbx", "%rcx", "%rdx");
    /* We cannot use "=A", since this would use %rax on x86_64 and return only the lower 32bits of the TSC */
    __asm__ __volatile__ ("rdtsc" : "=a" (lo), "=d" (hi));
    return (uint64_t)hi << 32 | lo;
}
```

This code has been taken from an answer on stackoverflow.

## C++

It is possible to create code compatible with both C and C++ by choosing the right header to include. The C++ compilers define the `__cplusplus` variable that can be used to determine the compilation language like in the following example

```cpp
#ifdef __cpluplus
#include <ctime>
#else
#include <time.h>
#endif
```

### `chrono`

C++11 has introduced the `chrono` header with time related functions

```cpp
#include <chrono>
```

The high resolution clock has a very good accuracy and can be used to reach the microsecond

```cpp
typedef std::chrono::high_resolution_clock clock;
typedef std::chrono::duration_cast d_cast;
typedef std::chrono::microseconds microseconds;
typedef std::chrono::duration duration;

clock::time_point start = clock::now();
//
// Do something
//
clock::time_point end = clock::now();

duration<double,microseconds> time_us = d_cast<double,microseconds>(end - start);
std::cout << "Elapsed: " << time_us.count() << std::endl;
```

Durations in the `chrono` header are complex objects. Refer to the documentation for the details on how to use them.

## Fortran

Fortran changed many times in its history and so the way to obtain time informations. In Fortran90 a wall time clock was available

```fortran
  real start, end

  call system_clock(start, clock_rate, clock_max)
! Do something
  call system_clock(end, clock_rate, clock_max)
  elapsed = real(end - start) / real(clock_rate)
```

The CPU time clock was added in Fortran95

```fortran
  real start, end

  call cpu_time(start)
! Do something
  call cpu_time(end)
  elapsed = real(end - start)
```
