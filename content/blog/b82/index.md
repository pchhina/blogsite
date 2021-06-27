---
title: "Syntax and Semantics of BSL (so far)"
date: 2021-06-27T07:43:14-05:00
draft: false
tags: ["Design", "BSL"]
---

[BSL](https://docs.racket-lang.org/htdp-langs/beginner.html?q=bsl) is a beginner student language provided in racket as a starting point language to learn program design. It has a very simple syntax and thus easy to remember. This aids in reasoning about the computation. Here are some notes on what I know so far.

## Definitions
### Constant definition
```lisp
(define name expr)
; for example
(define WIDTH 100)
```
`define` is the keyword which is followed by the name of the constant (in capitals for readability) which is followed by an expression. Expression can be a function application, cond, if, and, not or literals (number, boolean, string or character). In the above example, it is a number.

### Function definition
```lisp
(define (name variable variable ...) expr)
; for example
(define (c2k c) (+ c 273.15))
```
Again, `define` is a keyword followed by parenthesis begin followed by name of the function, which is *c2k* in our example. This is followed by one or more variable names. In our example, we have one variable, *c*. Then we close the parenthesis followed by an expression and closing the parenthesis again. Our expression adds 273.15 to c.

## Syntax and Semantics of expressions
Since expressions are at the core of computation, let's understand how different expressions are evaluated.
### Literals
These are the values provided by the language and are the simplest form of expressions. Since these are already values, no *evalutation* is needed. 
### Function application
```lisp
(name expr expr ...)
; for example
(c2k 30) ; will result in 303.15
```
Inside parenthesis, we write the name of the function followed by one or more expressions. It is evaluated in three steps:

1. All expressions are evaluated to their final values.
2. The number of values are matched with the number of parameters (from function definition).
3. In the body of the function definition, all the variable names are replaced by the values computed in step 2 and the body is evaluated using the symantics of the expressions thus formed.

In the above example of the function that convert temperature in celcius to Kelvin, when we apply the function, 30 is evaluated to 30 in step 1. Then in step 2, number of values are checked agains number of parameters which in this case is 1 for both and thus the check passes. Then in step 3, the body of the function is evaluated after replacing c with 30. So now we are left with evaluation of (+ 30 273.15). This is an expression of function application type and there if evaluated with the same rules as described in this section.

### if expression
```lisp
(if expr expr expr)
; for example
(if (> t -273.15) (c2k t) "non-physical input")
```
`if` expression contains exactly three sub-expression. Here is how the evaluation takes place:
1. First sub-expression is evaluated, which *must* have a boolean value (i.e. either #true or #false).
2. If the value of the first sub-expression is `#true`, then the second sub-expression is evaluated whose value becomes the value of the entire if expression. Note in this case that third sub-expression is never evaluated.
3. If the value of the first sub-expression is `#false`, then the third sub-expression is evaluated whose value becomes the value of the entire if expression. Note in this case that second sub-expression is never evaluated.

### cond expression
```lisp
(cond [b1 e1] ... [bn en])
; for example
(cond [(< n 0) "neg"] [(= n 0) "zero] [(> n 0) "pos"])
```
`cond` can be seen as a more general form of if which consists of pairs of expressions. Within each pair, first expression must evaluate to a boolean value. If `b1` evaluates to `#true` then `e1` is evaluated and becomes the value of the entire cond expression. Otherwise, this is repeated for `b2`, ..., `bn`. If none of the boolean expressions are evaluated to `#true`, then error is reported.
