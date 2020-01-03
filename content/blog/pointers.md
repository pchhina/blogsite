---
title: "Point me to Pointers"
date: 2019-12-17T18:09:59-06:00
tags: ["C", "Basics"]
draft: false
---

Alright, the fun stuff - C pointers!

Working with variables is good but having access to the memory where these
variables are stored is awesome. C allows us to work directly with the memory.
Pointer is a data type that stores the address of a particular memory location.
What do I need to do to use pointers?

1. **Create a pointer** of appropriate data type: `char *pc` or `int *pi`.
2. **Initialize the pointer** to appropriate data type variable, for example, if `i`
   is a variable, its address will be `&i`. So initializing the pointer will
look like `pi = &i;`.
3. **Use the pointers** In order to access the value corresponding to a pointer
   address, you can use `*<pointer_name>` in any expression just like you would
use the variable `i`.

## Passing pointers to function arguments

Functions in C pass arguments by value. This means if a variable `a` is passed
to a function, a *copy* of the variable is passed and any changes made to this
variable inside the function will not be saved/reflected in the calling function. One way to manipulate
the variables inside a function is to pass the address (pointer) of the variable
instead. How do we do this? Let's see a typical example where we want to swap
the values stored in let's say two integer variables.

```c
// swap.c -- swap two integers (demo use of pointers)
#include <stdio.h>

void swap(int *a, int *b); // swap take two int pointers as arguments

int main(void)
{
    int a, b;
    a = 12;
    b = 29;
    printf("a = %d, b = %d\n", a, b);

    swap(&a, &b); // passing address of a and b to swap()
    printf("a = %d, b = %d\n", a, b);

    return 0;
}

// swap() swaps the two integers
void swap(int *a, int *b)
{
    int tmp;
    tmp = *a; // a is the address, *a gets the value from that address
    *a = *b;
    *b = tmp;
}
```
```c
a = 12, b = 29
a = 29, b = 12
```
