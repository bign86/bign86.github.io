---
title: "Pointer to void"
date: 2013-12-30
tags:
  - c
  - c++
categories:
  - programming
author_profile: true
draft: false
comments: false
---

In C is legal to declare a pointer to a void variable. A `void*` contains an address like a regular pointer, but the pointed memory area has no specific type. void pointers are declared like any other pointer

```c
void *ptr;
```

Note that is NOT legal to have a void variable. The declaration

```c
void a;
```

will give an error.

## Assigning a void pointer

Since `void*` has no specific type, it can be used to point to the memory address of any variable type. So

```c
void *ptr;
char *ch;
int *i;
double *dbl;
struct {
    int value;
} my_structure;
ptr = &ch;
ptr = &i;
ptr = &dbl;
ptr = &my_structure;
```

are all valid assignement.

## Dereference a void pointer

But to be dereferenced and hence used, it must be cast to some existing type. Casting is necessary so the compiler knows how to use the pointer. For example

```c
void *ptr;
*ptr = 1.234;          /* WRONG! */
*ptr = 'a';            /* WRONG! */
*(float*)ptr = 1.234;  /* cast to float* CORRECT! */
*(char*)ptr = 'a';     /* cast to char*  CORRECT! */
```

## Uses of a void pointer

### Generic structure

`void*` can be used to create a generic structure used to store any kind of data.

```c
struct {
    void *data;
} generic_str;
```

### Function input parameter

A function can take in a parameter of `void*` type

```c
void function(int p1, void *p2)
{
    /* ... */
}
```

To use `p2` we must cast it to the correct type inside the function. This way we can have functions having different behavior with different parameter type.
The function of course must know to which type cast. One way is to pass the type in another parameter. For example

```c
function(FLOAT, num);
```

Sometimes a function does not need to know the type of a variable to be able to work correctly. It is the case of a generic swap function. Such a function, just swapping the values of `a` and `b`, just need to know the size of the data type, but not the data type itself. Everything is cast to a `char` (which has size 1) and bytes of the input variables are taken and swapped one by one.

```c
void swap(void *a, void *b, size_t size)
{
    unsigned char *aa = a;
    unsigned char *bb = b;
    unsigned char *tmp;
    int i;
    for ( i=0; i<size; ++i ){
        tmp = aa[i];
        aa[i] = bb[i];
        bb[i] = tmp;
    }
}
```

called with

```c
double num1=1.43, num2=0.34;
swap(num1, num2, sizeof(double));
```

### Function output parameter

A function that return a `void*` can be defined as

```c
void* function(int p)
{
    /* ... */
}
```

The return type of such a function can be cast to any type. One example is `malloc()` that allocates memory and returns a `void*` pointer. The memory is still raw since it's allocated but nothing has been constructed in it.

```c
char *string;
string = (char*)malloc(100);
```

Note that this code is very old style and nowadays is better to use the implicit casting of void pointers

```c
char *string;
string = malloc(100);
```

## Pointer arithmetic

To perform pointer arithmetic on a `void*` casting is required. This is because the compiler must know the size of the data which is pointed to to be able of jump of the correct amount of bytes. For example

```c
void *ptr;
ptr = &var;
void *ptr_2 = ptr++;
```

This won't work since the size of `void*` is not known. Note that GNU C (and hence the gcc compiler) allows pointer arithmetic with `void*` pointers assuming a size of 1, like a `char*`. However this a non portable feature and its use should be avoided.

## Pointer to pointer to void

It is valid to declare pointers to pointers to void using `void**`. However, a double pointer has not the same property of a normal `void*`. A `void*` can point to any variable type, but a `void**` can point to a `void*` only. In other words it has not the special meaning that a normal pointer to void has and it is treated like any other pointer.

## Explicit casting of C++

In C++ it is forbidden to have an implicit casting when using a `void*` pointer. For example, in C the following is allowed

```c
int value;
void *ptr = &value;
int other = *ptr;
```

In C++ the following is valid instead

```cpp
int value;
void *ptr = &value;
int other = *static_cast<int*>ptr;
int *other2 = static_cast<int*>ptr;
```
