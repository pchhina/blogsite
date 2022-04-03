---
title: "Templates for medium to large sized enumerations"
date: 2022-04-03T11:54:09-05:00
draft: false
tags: ["Design"]
---
[Templates](../b96) really help in clarifying the structure of the function. But once we get a hang of them, we may wait to write them until designing the function. For example, template for atomic non-distinct data is simple:

```racket
(define (fn-for-x x)
  (... x))
```
There is no need to write this in the data definition. Another important case however is that of medium (I'm thinking 10-20) or large (> 20) sized enumerations. It is because enumerations fall under *one-of* rule which requires as many `cond` cases as there are number of enumerations. It is a waste of time to write all these cases in a template knowing that the final function would likely be more specific and would need only a subset of cases that require explicit handling while most other would fall under *catch-all* `else` clause.

Recently I was working with a card game program where I used enumeration to represent a suit. This data would result in writing 13 cases in cond clause:

```racket
;; Card is one of:
;;  - 'ace
;;  - 'two
;;  - 'three
;;  - 'four
;;  - 'five
;;  - 'six
;;  - 'seven
;;  - 'eight
;;  - 'nine
;;  - 'ten
;;  - 'jack
;;  - 'queen
;;  - 'king

;; interp. cards in a plying deck
```

Instead of writing the template then, we want to wait and see what function needs to be written for this data. For example, if we want to write `value-of` function that translates a `Card` to a numerical value in range `[1, 10]` where `'ten` and all picture cards have value of 10, we can write the template (and in this case the function as well) as:

```racket
;; Card -> Integer[1, 10]
;; produce numerical value for the given card
;(define (score c) 2)
(define (value-of c)
  (cond [(symbol=? 'ace c) 1]
        [(symbol=? 'two c) 2]
        [(symbol=? 'three c) 3]
        [(symbol=? 'four c) 4]
        [(symbol=? 'five c) 5]
        [(symbol=? 'six c) 6]
        [(symbol=? 'seven c) 7]
        [(symbol=? 'eight c) 8]
        [(symbol=? 'nine c) 9]
        [else 10]))
```

Although we had to enumerate cases 1 through 9 explicitly, the other four could be handles together in `else` clause.