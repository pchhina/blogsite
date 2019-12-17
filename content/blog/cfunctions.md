---
title: "C Functions"
date: 2019-12-16T18:22:54-06:00
draft: false
---
Functions in C are set up a bit differently than I have seen other languages so it's
worth taking a note. Basically, to use a function, we need to do three things:

1. **Declare prototype**: This is like the header line in function definition but
   with a semi-colon at the end. This will come before main() and is a way of
telling the compiler what to expect when this function will be called - return
type, name of the function and argument types, if any. If the function do not
take any arguments or return any value, *void* keyword can be used.

2. **Function call**: This is standard stuff - call the function wherever you want
   to use it. 

3. **Function definition**: This is where the function is actually implemented.
   Typically either after the main() or in a separate file altogether for larger
programs.

## Calculate Integer Powers

```c
// fun.c -- demo of function in C
#include <stdio.h>

int power(int x, int n); // function prototype

int main(void)
{
    printf("3 raise to the power 5 is %d\n", power(3, 5)); // function call
    printf("2 raise to the power 10 is %d\n", power(2, 10)); 

    return 0;
}

// **** Function Definition ****
// power() calculates x^n where both x and n are integers
// Arguments: x, n (both are integers)
// Return value: x^n
int power(int x, int n) // function definition
{
    int ans = 1;
    for (int i = 0; i < n; i++)
    {
        ans *= x;
    }
    return ans;
}


```
```c
3 raise to the power 5 is 243
2 raise to the power 10 is 1024
```
