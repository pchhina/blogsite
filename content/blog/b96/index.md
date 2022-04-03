---
title: "Data Driven Templates"
date: 2022-03-26T21:30:44-05:00
draft: false
tags: ["Design"]
---
One key step in the design of a function is to use a template. Templates provide a scaffolding around which to build the function body. The structure of the template is strongly coupled with the type of data which is driven from the kind of information. So it is important to look at the structure of the information that we want to model because it eventually shows up in the structure of final program design. Here we are going to look at how the type of data determines the type of template we want to use. 

## Type of Data

- Atomic non-distinct
- One of
- Atomic distinct
- Compound
- Reference
- Self-reference

### Atomic Non-Distinct
Simple atomic data and intervals fall under this category. Examples include `Number`, `String`, `Boolean`, `Number[0,5]`.
```racket
;; Template for Atomic Non-Distinct Data
(def (fn-for-x x)
  (... x))

;; Cond question
(<type>? x)

;; Cond answer
(... x)
```

### One Of
This category of data include enumerations (fixed number of distinct items), itemizations (data containing multiple sub-classes at least one of which is not distinct). This category produces as many `cond` question/answer pairs as number of sub-classes. The question and answer parts in the template are filled using corresponding type's Cond question/answer. If sub-classes are of mixed types, these must be guarded with an appropriate type predicate.
```racket
;; Template for One Of Data
(def (fn-for-x x)
  (cond [Q A]
	    [Q A]
		...))
```

### Atomic Distinct
Atomic distinct refers to specific members of atomic classes. For example `2`, `true`, `march` etc. These occur as one of sub-classes of "One Of" data types.
```racket
;; Cond question
(<value>? x)

;; Cond answer
(...)
```

### Compound
Compound data is made up of multiple pieces (each piece can be any of the types being discussed here) that naturally belong together. A player, for example, can be represented by compound consisting of name, age, score. Lists are also compound data consisting of two pieces `(cons first rest)`. For compounds, the function body must deal with all pieces, so the template becomes:
```racket
;; Template (and Cond answer) for Compound x with two pieces p and q
(def (fn-for-x x)
  (... (x-p x)
	   (x-q x)))

;; Cond question, predicate to check if x is of type c
(c? x)
```

### Reference
Reference data is the non-primitive data, that is, the data that we (or someone else) designs from primitive data (data that a language provides by default). In an object oriented language, one can think of reference data as classes that we build. If we find a reference in our template, it should be wrapped inside a call to that type's template:
```racket
;; Cond question
(<type>? x)

;; Cond answer
(fn-for-x x)
```

### Self-Reference
Self-reference data is used to represent arbitrary amount of information. These are modeled using `List`. Almost all languages provide some kind of data structure to model self-references. Type comment for list must be a *well-formed self-reference*, meaning, it should have at least one sub-class which is non-self-referential and at least one sub-class which is self-referential. Therefore, the template comes from *One Of* template type. Cond question comes from compound template and cond answer comes from reference template.
```racket
;; Cond question
(cons? x)
```

### Mutual-Reference
Mutual references contain two or more non-primitive data types that refer to each other in their type comment. Reference rule is used (similar to self-reference) to form the template. Form and group all templates in mutual reference cycle together.


## Putting it together
One key observation from the structure of above templates is that a new data can start with any one of the following choices. Rest of the structure can be built by applying one or more rules described above repeatedly as needed.

1. Atomic Non-Distinct
2. One Of
3. Compound

### Example 1: Modeling a cartesian point in 3-dimensional space
A cartesian point in 3-d space is represented as (x, y, z) where x, y and z are Real Numbers. Looking at the structure of this information, it is apparent that it has 3 pieces that naturally belong together. So it is a compound. Let's write it's type comment:
```racket
;; Structure definition
(struct point (x y z))

;; Point is (point Number Number Number)
;; interp. a point in 3-D space
```
So we start by using the compound rule:
```racket
(def (fn-for-point p)
  (... (point-x p)
	   (point-y p)
	   (point-z p)))
```
After this we observe the type of each selector and note that these are primitive data. So we stop here. If these were non-primitive data, we could have used one of the rules described earlier to deal with it.


### Example 2: Modeling protons in a collider
A particle could be anything but for our exercise let's assume we are a physicist who has to deal with protons in a collider experiment. A proton is a complex piece of information to model but let's say we need to track its mass and position in 3-d space. So we start by writing a new structure for it:
```racket
(struct proton (mass pos))
;; Proton is (proton Number Point)
;; interp. a proton represented by mass and its position in 3-D space
```
Once again we start with compound rule:
```racket
(def (fn-for-proton p)
  (... (proton-mass p) ; Number
	   (proton-pos p)))     ; Point
```
The first piece of this data is a number, so we leave it as-it-is but note that second piece is non-primitive. Therefore, we use reference rule to wrap this selector inside the funtion for point:
```racket
(def (fn-for-proton p)
  (... (proton-mass p) ; Number
	   (fn-for-point (proton-pos p))))     ; Point
```
Furthermore, we ask how many protons are there in the collider? Well, we don't know - it could be zero, 10 or 10 million. This suggests that we are dealing with arbitrary number of protons, so we write its type comment as a well-formed self-reference:
```racket
;; ListOfProton is one of:
;;  - empty
;;  - (cons Proton ListOfProton)
;; interp. an arbitrary number of protons in a collider
```
So we start with *One Of* rule to write its template:
```racket
(def (fn-for-lop lop)
  (cond [Q A]
	    [Q A]))
```
In the first Q/A pair, we are dealing with atomic distinct data (empty), so we use the rule for that:
```racket
(def (fn-for-lop lop)
  (cond [(empty? lop) (...)]
	    [Q A]))
```
The second Q/A pair deals with `cons`, we will use compound rule and because it is the last Q/A, we can use `else` instead of a predicate:
```racket
(def (fn-for-lop lop)
  (cond [(empty? lop) (...)]
	    [else (... (first lop)        ; Proton
			       (rest lop))]))     ; ListOfProton
```
Now we observe that both pieces of the compound are non-primitive data, so we use the reference rules on both:
```racket
(def (fn-for-lop lop)
  (cond [(empty? lop) (...)]
	    [else (... (fn-for-proton (first lop))     ; Proton
			       (fn-for-lop (rest lop)))]))     ; ListOfProton
```
This becomes the final template for modeling arbitrary number of protons in a collider.