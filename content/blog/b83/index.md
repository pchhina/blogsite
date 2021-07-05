---
title: "Design one function per task and compose"
date: 2021-07-04T16:44:09-05:00
draft: false
tags: ["Design", "BSL"]
---
One idea in program design is to limit the number of tasks performed by a single function to one. It is not always clear however what constitutes a single task. For example, one can argue that a spreadsheet program has a single task - process tabular data. This is one extreme of spectrum where a task definition is too broad. On the other hand, a far too narrow task will be to add two numbers where a function already exists and you can wrap this inside your own definition of adding two numbers. Generally it is a good idea though to keep the task to a limited number of operations which makes it easier to debug. When you have many functions where each one is performing a specific task, it also becomes easier to *compose* these functions in different ways which is a preferable method to design larger programs.

Now let's work on a concrete [problem](https://www.sfu.ca/math-coursenotes/Math%20157%20Course%20Notes/sec_Optimization.html) to see function composition at work.

## Problem Statement
*You want to sell a certain number n, of items in order to maximize your profit. Market research tells you that if you set the price at 1.50 dollars, you will be able to sell 5000 items, and for every 10 cents you lower the price below 1.50 dollars you will be able to sell another 1000 items. Suppose that your fixed costs ( “start-up costs” ) total 2000 dollars, and the per item cost of production ( “marginal cost” ) is 50 cents.*

*Find the price to set per item and the number of items sold in order to maximize profit, and also determine the maximum profit you can get.*

## Approach
We can write expressions for number of items sold, total cost, total revenue and profit as follows:

$$ n = 5000 + \frac{1.50 - unitPrice}{0.10} \times 1000$$

$$cost = 2000 + 0.5  n$$

$$revenue = n \times  unitPrice$$

$$profit = revenue - cost$$

Looking at these equations, one can do substitution (like in our algebra class) and come up with a single expression to compute profit as a function of unit price. In programming however, we want to keep these functions separate and compose them to get the final answer. It's good idea to define our constants first:

```lisp
; constant definitions

(define BASE_COST 2000)
(define MRGNL_COST 0.5)

(define BASE_PRICE 1.50)
(define BASE_ITEMS 5000)
(define MRGNL_ITEMS (/ 1000 0.1))
```

## Helper functions
Our objective is to find profit for a given unit price of an item. Profit depends on revenue and cost. Both revenue and cost depend on the number of items while the number of items depend on the unit cost. This makes the first three equations above as helper functions which are defined as follows:

```lisp
; function definitions

; Number -> Number
; computes number of items sold based on unit cost
(check-expect (how-many-items? 1.50) 5000)
(check-expect (how-many-items? 1.40) 6000)
(define (how-many-items? cost)
  (+ 5000 (* MRGNL_ITEMS (- BASE_PRICE cost))))

; Number Number -> Number
; computes revenue based on qty items sold
; and unit cost
(check-expect (revenue 1000 0.5) 500)
(check-expect (revenue 5000 1.5) 7500)
(define (revenue qty cost)
  (* qty cost))

; Number -> Number
; computes the total cost based on qty sold
(check-expect (cost 5) 2002.5)
(check-expect (cost 5000) 4500)
(define (cost qty)
  (+ BASE_COST (* MRGNL_COST qty)))
```

## Main function
Final function to compute profit becomes the main function the three helper functions above to do its job:

```lisp
; Number Number -> Number
; computes profit based on unit cost
(check-expect (profit 1.5) 3000)
(check-expect (profit 1.4) 3400)
(define (profit unit-cost)
  (- (revenue (how-many-items? unit-cost)
              unit-cost)
     (cost (how-many-items? unit-cost))))
```

By running our profit function for various prices, we find that 1.25 dollars gives the maximum profit of 3625 dollars.

```
> (profit 1.5)
3000
> (profit 1.4)
3400
> (profit 1.3)
3600
> (profit 1.2)
3600
> (profit 1.1)
3400
> (profit 1.25)
3625
> (profit 1.27)
3621
> (profit 1.23)
3621
> (profit 1.6)
2400
```
We make this profit by selling 7500 items:

```
> (how-many-items? 1.25)
7500
```
