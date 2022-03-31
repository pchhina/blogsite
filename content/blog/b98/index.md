---
title: "Symbols for Enumerations"
date: 2022-03-31T06:06:28-05:00
draft: false
tags: ["Racket"]
---
[Symbols](https://docs.racket-lang.org/reference/symbols.html) are interesting datatype in racket. These are just like strings but created using a single quote in front (`'hello` instead of `"hello"` for example). While they can contain whitespace characters too (when converting from strings using `string->symbol` procedure), it looks like it is not very common. We can think of these as identifiers where the identifier itself is the value. This makes them convenient to use in enumeration. In addition, comparing symbols is apparently faster than comparing strings. Let's see this in an example.

Let's say we want to classify buildings in the city of Chicago based on how old they are into of the three categories - new, old and heritage.

## Data Design
```racket
;; BuildingStatus is one of:
;;  - 'new
;;  - 'old
;;  - 'heritage
;; interp. status of buildings in downtown Chicago

#;
(define (fn-for-building-status bs)
  (cond [(symbol=? 'new bs) (...)]
        [(symbol=? 'old bs) (...)]
        [(symbol=? 'heritage bs) (...)]))
```

## Function Design
Suppose we want to design a function now that determines whether a building should be demolished - only older buildings can be demolished.

```racket
;; BuildingStatus -> Boolean
;; produce true if the building is old and thus can be demolished

(define (demolish? bs)
  (cond [(symbol=? 'new bs) false]
        [(symbol=? 'old bs) true]
        [(symbol=? 'heritage bs) false]))

(check-false (demolish? 'new))
(check-true (demolish? 'old))
(check-false (demolish? 'heritage))
```