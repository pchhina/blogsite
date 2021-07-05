---
title: "Function design process"
date: 2021-07-04T20:29:32-05:00
draft: false
tags: ["Design", "BSL"]
---
Systematic design of functions is at the core of program design philosophy. Here I summarize the steps that are involved in designing a function. While Step 1 (Coming up with data definitions) is important, I don't quite see it's context of use in every function. Maybe its because I am working with atomic data to learn design which makes data definitions obvious or I don't have full understanding yet. Nevertheless, I lay it out here so I can continue to consider it and see if I understand the broader context in the future. Other steps are fairly clear on what they mean. 

## The Process

### Step 1: Data Definitions
While programs work exclusively with data, they represent some information about the domain of a particular problem. We must be able to clearly map what data is used to represent the input information, and conversely, how is the output data from a program to be interpreted as the information about the domain.

Let's start with a simple example to illustrate the steps:

*Design a program to convert temperature in degree Celcius to Kelvin scale*

So the data definition becomes something like this:

```
;; Input: We use numbers to represent temperature
;; Interpretation: Output is the temperature in K
```  
Here, temperature is the information that we can speak of without thinking about programs and data. In this example, since we use numbers to represent temperature in the real world too, there is not much ambiguity and our job is easy - every language provide some kind of numbers as one of the atomic data types.

### Step 2: Write function signature
What kind of data the function is going to consume and produce.
```
;; Number -> Number
```
Here we are saying that our function consumes only one piece of data, which is `Number` and also produces one piece of data, which again is a `Number`. In some languages (like Haskell), it is customary to write this signature as a part of function definition, while many languages don't care. Statically typed languages do convey the signature in the function definition itself, but I find the above syntax really clear. Even in those languages, one can add a comment like this.

### Step 3: Write purpose statement
Purpose statement tells the reader *what* does the function do.
```
;; Converts temperature in degree C to temperature in K
```

### Step 4: Write stub
Stub is the simplest structure of the function. No computation is performed. Function body is an arbirary element from the output class.
```lisp
;; Stub
(define (c2k c)
 0)
```
This step helps define the name of the function (`c2k` in this case), as well as high level starting point.

### Step 5: Revise the purpose statement to include parameter names from stub.
```
;; Converts temperature, c, in degree C to temperature in K
```
Here we modify the purpose statement to include parameter names to make the purpose more clear.

### Step 6: Write unit tests
List few examples in the form of unit tests before the stub:
```lisp
(check-expect (c2k 0) 273.15 )
(check-expect (c2k -273) 0.15)
```

### Step 7: Write function template
Here we add a comment in the body (or other means provided by a language) to list parameter names and other constants defined elsewhere (that may be useful to define our function) so that we can look at those collectively as ingredients that we could use to write function body. Here is what we get so far:
```lisp
;; Input: We use numbers to represent temperature
;; Interpretation: Output is the temperature in K

;; Number -> Number
;; Converts temperature, c, in degree C to temperature in K
(check-expect (c2k 0) 273.15 )
(check-expect (c2k -273) 0.15)
(define (c2k c)
 ... c ...)
```
### Step 8: Define the function
This becomes the last step, where we replace the template with expression that can compute the expected result. So our final program becomes:

```lisp
;; Input: We use numbers to represent temperature
;; Interpretation: Output is the temperature in K

;; Number -> Number
;; Converts temperature, c, in degree C to temperature in K
(check-expect (c2k 0) 273.15 )
(check-expect (c2k -273) 0.15)
(define (c2k c)
 (+ c 273.15))
```
Now we should be able to run our function to check whether the tests pass or not and start to debug it if needed.

The challenge now is to adapt and practice this mindset!
