---
title: "Finding Two Larger Numbers Out of Three"
date: 2019-11-03T09:14:40-06:00
tags: ["Scheme", "Algorithms"]
draft: false
---

This appears to be a fairly simple task and indeed it is when you can use
`list` and `sort` functions in language like Python. However, I attempted to do
this in Scheme using only conditional and single numbers as data. It turned out
be a little tricky exercise for me. I had to explicitly define all the cases so
my solution is not easily extensible to more numbers.

**Problem:** Define a procedure that takes three numbers as arguments and
returns the sum of the squares of the two larger numbers.

**Input:** Three real numbers.

**Output:** Sum of squares of the two larger numbers.

## Pseudocode

$\mathbf {Step \,1:} \text{ Read the three numbers x, y and z.}$

$\mathbf {Step \,2:} \text{ If } x \gt y \gt z, \text{set }a \leftarrow x, \text{set }b \leftarrow y$ 

$\mathbf {Step \,3:} \text{ If } x \lt y \lt z, \text{set }a \leftarrow y, \text{set }b \leftarrow z$

$ \mathbf {Step \,4:} \text{ If } x \gt y \lt z, \text{set }a \leftarrow x, \text{set }b \leftarrow z $

$ \mathbf {Step \,5:} \text{ If } (x \lt y \gt z)\land (x \lt z), \text{set }a \leftarrow y, \text{set }b \leftarrow z \,\, \mathbf {else} \text{ set }a \leftarrow x, b \leftarrow y $

$ \mathbf {Step \, 6:}\, \text{Return } a^{2}+b^{2} $

## Scheme Implementation

```{scheme}
(define (square x) (* x x))

(define (squared-sum x y) (+ (square x) (square y)))

(define (squared-sumof-larger-two x y z)
  (cond ((and (> x y) (> y z)) (squared-sum x y))
        ((and (< x y) (< y z)) (squared-sum y z))
        ((and (> x y) (< y z)) (squared-sum x z))
        ((and (< x y) (> y z)) (if (< x z)
                                    (squared-sum z y)
                                    (squared-sum x y)
                                    )
                               )
        )
  )
```

## Test

```{scheme}
> (squared-sumof-larger-two 5 6 7)
85
> (squared-sumof-larger-two 9 -1 2)
85
> (squared-sumof-larger-two 2 3.2 -5)
14.240000000000002
> 
```
