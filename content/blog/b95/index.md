---
title: "Function Design Revisited"
date: 2022-03-19T11:31:04-05:00
draft: false
tags: ["Racket"]
---
[Earlier](../b84) I wrote about the key steps in a function design. Now after some experience writing functions using the design recipe, I want to revise the recipe based on what I learnt and extend that to Racket.

## Recipe Revision
**Step 1: Signature, Purpose and Stub**
- Signature clarifies types of function parameters and return values
- Purpose is one line describing what the function produces
- Stub helps to think about function name, parameter names and allows to run tests from next step

**Step 2: Examples as Tests**
- Thinking about concrete examples is easier than finding general solution. This enables us to think about the computation that we want to perform on input data in a very specific case. At this point we can run and check if our tests are well-formed (no syntax errors).

**Step 3: Template and Inventory**
- This is an important piece for non-primitive data. For primitive data we just want to list the parameter names that we have to work with.

**Step 4: Write the Function Body**
- At this point, bulk of the design work is complete on which we can rely on to write the function body. We can look at the signature, purpose and examples together and see how we can compute the required result.

**Step 5: Test and Iterate until all tests pass**
- Run the tests and iterate to fix problems you find.

**Step 6: Review and Clean**
- Remove any redundant comments, templates, stubs
- Can the signature be more specific?
- Do we have enough tests?

Let's try to look at these steps through a simple design exercise - Design a function to check if length of a string is less than 5.



## Racket Example
So we need a function that consumes a string and produces a boolean. So following will be the signature:

```racket
;; String -> Boolean
```
Adding a one line purpose, our code becomes:

```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
```
Now let's think of stub, `less-than-5?` is a good function name for this problem and `str` as the parameter. Note that we arbitrarily picked `false` as return value - it just has to match the type of return while the actual value is unimportant.

```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
(define (less-than-5? str) false)
```
Now let's think about some examples. Since it returns `boolean`, we want at least two examples - one with `false` and other with `true` as return value:

```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
(define (less-than-5? str) false)

;; Tests
(check-true (less-than-5? "hi") "string length less than 5")
(check-false (less-than-5? "hi there!")"string length is not less than 5")
```
We run our tests to make sure they are well formed. These are expected to fail (see below) since we have not coded the body of our function.
```
--------------------
. FAILURE
name:       check-true
location:   htdf-test.rkt:11:0
params:     '(#f)
message:    "string length less than 5"
--------------------
```
Ok, now we write the template and comment the stub:
```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
;(define (less-than-5? str) false)
(define (less-than-5? str)
  (... str))

;; Tests
(check-true (less-than-5? "hi") "string length less than 5")
(check-false (less-than-5? "hi there!")"string length is not less than 5")
```
Now we can think about what should go in `...` place in the template that likely uses `str` parameter. In this case the body is simple:
```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
;(define (less-than-5? str) false)
(define (less-than-5? str)
  (< (string-length str) 5)

;; Tests
(check-true (less-than-5? "hi") "string length less than 5")
(check-false (less-than-5? "hi there!")"string length is not less than 5")
```

Our tests pass but in the last step we review our code for readability and see if it can be improved. Observing the purpose, we note that an edge case is not tested where string length is equal to 5. We should produce false in that case. We have also removed the stub now.

```racket
;; String -> Boolean
;; Produce true if given string length is less than 5
(define (less-than-5? str)
  (< (string-length str) 5)

;; Tests
(check-true (less-than-5? "hi") "string length less than 5")
(check-false (less-than-5? "hello") "string length is equal to 5")
(check-false (less-than-5? "hi there!")"string length is not less than 5")
```

All tests pass!


