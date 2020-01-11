---
title: "Integer overflow behavior in C"
date: 2020-01-02T19:42:29-06:00
tags: ["C", "Security"]
draft: false
---
As I continue to explore the C language, I realize that this is way different
than anything else I have come across. When it comes down to what the rules are,
many behaviours are machine dependent (size of data types for example).
Surprisingly often, I also see the phrase *undefined behavior*. This means that
the language does not define what happens should a specific situation occurs and
it is upto the compiler to decide what should happen! This is very unsettling to
me (you say there are no rules for this??). One such thing I came across today
is integer overflow.

## What it Integer Overflow?
Regardless of what *type* of data we deal with, it is represented in computer
memory using finite number of bits. This means there is a maximum limit of a
quantity for each type that can be stored. For any system, these limits can be
seen by looking at the constants defined in `limits.h` header file as shown in
the example below. The question is what happens when an arithmatic operation on
a data pushes the result beyond what a specific type can accomodate. Guess what?
We don't see any error. Most compilers apparently do a *wrap around* behavior.
In the example below for intance, if `UCHAR_MAX` is 255 and you add 1 to it, you
get 0 as result. Similarly if we subreact 1 from 0 (underflow?), we get 255.

Since there are no errors generated (I did not see any warnings either), the
behavior can lead to bugs or vulnerabilities that might be difficult to pin
down.

## What I don't know (yet)
- Can I write a program that somehow checks for overflow and reports failure?
- Are there any tools out there already that can implement this check?
- Is there a way to check this in an existing program?

An interesting topic that needs some further thought!

## A Sample Program
```C
// int_overflow.c -- overflow, underflow with some typical integer types
#include <stdio.h>
#include <limits.h>
int main(void)
{
    printf("int min and max values are: %d, %d\n\n", INT_MIN, INT_MAX);
    printf("short min and max values are: %d, %d\n\n", SHRT_MIN, SHRT_MAX);
    printf("char min and max values are: %d, %d\n\n", CHAR_MIN, CHAR_MAX);
    printf("unsigned max value is: %d\n\n", UCHAR_MAX);

    int a = INT_MAX;
    printf("a is %14d\n", a);
    a++;
    printf("a is %14d\n", a);
    a--;
    printf("a is %14d\n\n", a);

    short b = SHRT_MAX;
    printf("b is %14hd\n", b);
    b++;
    printf("b is %14hd\n", b);
    b--;
    printf("b is %14hd\n\n", b);

    char c = CHAR_MAX;
    printf("c is %14hd\n", c);
    c++;
    printf("c is %14hd\n", c);
    c--;
    printf("c is %14hd\n\n", c);

    unsigned char d = UCHAR_MAX;
    printf("d is %14hd\n", d);
    d++;
    printf("d is %14hd\n", d);
    d--;
    printf("d is %14hd\n\n", d);


    return 0;
}

```
## Results
```C
int min and max values are: -2147483648, 2147483647

short min and max values are: -32768, 32767

char min and max values are: -128, 127

unsigned max value is: 255

a is     2147483647
a is    -2147483648
a is     2147483647

b is          32767
b is         -32768
b is          32767

c is            127
c is           -128
c is            127

d is            255
d is              0
d is            255
```
