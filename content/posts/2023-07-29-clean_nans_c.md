---
title: "Clean NaNs in C"
date: 2023-07-29
tags:
  - c
  - nan
categories:
  - programming
author_profile: true
draft: false
comments: false
---

The C language has not the concept of NaN as a primitive. However, it is possible to introduce it through the header `stddef.h`, the definition being `NAN`.

A common problem with NaNs is the need to remove them from a given array such as

```c
// original array
double arr[] = {1.0, 3.2, 7.3, NAN, 9.1, 8.3};
size_t n = 6;
```

The easiest approach would be to have a secondary array where to copy the "good" values from the original array. First it is necessary to calculate the size of the new array by walking the original array and counting the valid values. Next, the new array can be allocated and the valid values copied in

```c
// walk the array and count
size_t new_n = 0;
for (size_t i = 0; i < n; ++i)
{
  if (!isnan(arr[i]) new_n++;
}

// allocate the new array
double new_arr[new_n];

// walk both arrays copying the valid values
size_t j = 0; 
for (size_t i = 0; i < n; ++i)
{
  if (!isnan(arr[i])
  {
    new_arr[j++] = arr[i];
  }
}
```

This solution has the merit of being simple, but it requires to walk the array twice, and to allocate extra memory for the secondary array.

A faster solution is to modify the array in place by having a fast pointer running in front and searching for the valid values, and a slower one lagging behind to reference the point where the valid values can be copied

```c
size_t j = 0;
for (size_t i = 0; i < n; ++i)
{
  if (!isnan(arr[i])
  {
    // copy the element from the fast pointer into the slow moving position
    arr[j++] = arr[i];
  }
}
```

This requires visiting the original array only once, but leaves `n - k` trailing redundant values at the end of the array, with `k` equal to the number of NaNs.
