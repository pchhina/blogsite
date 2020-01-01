---
title: "Array name is pointer to first element"
date: 2019-12-22T06:48:23-06:00
tags: ["C", "Array"]
draft: false
---

Arrays in C provide a mechanism to group multiple data points of the same type
into a single variable. Two noteworthy points about C arrays are:

1. Array names store the address of the first element (see the first section of
   the code below). A simple implication of this is that when arrays are passed
to functions (as arguments), and the fact that arguments are passed by values in
C,  we essentially are passing the address where the data is stored and
not copy of the data. So any change made to the array inside the called function will
change the original array.

2. Array bounds are not automatically checked. So you can access memory that is
   not part of the declared array. This is where power and danger intersect! At
the moment just make sure to check the bounds explicitly before using the array.
In the program below, this is done by passing the size of the array to the
function that uses it and use the size argument when looping through the array.

## Program

```c
// array.c -- showing that array name is a pointer to first element
#include <stdio.h>
void add10(int a[], int l);

int main(void)
{
    int nums[5] = {10, 20, 30, 40, 50};
    int nums_size;
    printf("Is array name the address? %d\n", nums == &nums[0]);

    nums_size = sizeof nums / sizeof nums[0];
    printf("\n");

    for (int i = 0; i < nums_size; i++)
        printf("nums[%d] = %d\n", i, nums[i]);

    printf("\n");
    add10(nums, nums_size);
    printf("after calling add10 on nums...\n");
    printf("\n");

    for (int i = 0; i < nums_size; i++)
        printf("nums[%d] = %d\n", i, nums[i]);

    return 0;
}

// add10: Function to add 10 to each element of the array
void add10(int a[], int l)
{
    for (int i = 0; i < l; i++)
        a[i] += 10;
}
```

## Output

```c
Is array name the address? 1

nums[0] = 10
nums[1] = 20
nums[2] = 30
nums[3] = 40
nums[4] = 50

after calling add10 on nums...

nums[0] = 20
nums[1] = 30
nums[2] = 40
nums[3] = 50
nums[4] = 60
```
