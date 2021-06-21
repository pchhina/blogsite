---
title: "To program is to write arithmatic expressions"
date: 2021-06-04T23:58:36-05:00
draft: false
tags: ["Design", "BSL"]
---
One perspective to program is simply to write arithmatic expressions. Some key observations:

- Arithmatic here refers to combining data using operations that can be performed on the data.
- Primitive data such as numbers, strings, booleans etc. is called *atomic data* and is provided by any language as a jumping off point.

## An example using numbers
```lisp 
> (+ 1 2 3)
6
> (modulo 10 3)
1
> (floor 3.6)
3
```
In the above example, the first arithmatic expression has `+` as the operator and `1 2 3` as the number arguments to the operator. In the second example, we have `modulo` as the operator and `10 3` as arguments and finally we have `floor` as the operator and `3.6` as the single argument.
## An example using strings
```lisp
> (string-append "hello" " " "world!")
"hello world!"
> (string-length "hello world!")
12
```
Now we have `string-append` as the operator, which is like addition of strings (instead of numbers) and `"hello" " " "world!"` as three arguments. Then, there is `string-length` as the operator with a single argument, `hello world!`. Note as illustrated in this last example, that the value of the expresssion can be of different type than the arguments. In this particular case, the arguments are `string`s while the result is a `number`.

## An example using booleans
```lisp
> (or #true #false)
#true
> (and (> 1 1) (string=? "hello" "hi"))
#false
```
In the first example here, we have two boolean arguments - `#true` and `#false` and `or` as the operator. In the second example, there are two sub-expressions - one comparing `number`s and other comparing `string`s, the values of which becomes arguments to the `and` operator.

## Checking for kind of data before doing arithmatic
In a dynamically typed language, mismatch between the data and the operator will result in an error:

```lisp
> (+ "hello" "world")
+: expects a number, given "hello"
```
It is thus important to **check the data type** before doing arithmatic on the data. This is done by using **predicates**:

```lisp
> (define x 2)
> (define y "hello")
> (define z #false)
> (if (number? x) (sqr x) 0)
4
> (if (number? y) (sqr y) 0) 
0
> (if (string? y) (string-length y) 0)
5
> (if (string? x) (string-length x) 0) 
0
> (if (boolean? z) (not z) 0)
#true
> (if (boolean? y) (not y) 0)
```
In the above examples, instead of failing on getting incorrect kind of data, we are producing 0 as a result. Of course, this may not be ideal design either, but it shows a way of guarding an expression against incorrect data type.