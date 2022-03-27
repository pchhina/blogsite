---
title: "Testing Guidelines For Functions"
date: 2022-03-27T13:11:25-05:00
draft: false
tags: ["Design"]
---
Writing tests before writing the function body is an important step in the design process. It helps us to think of concrete examples which are simpler to devise than a general solution. Using concrete examples wrapped in well-formed tests help break writing the general body into manageable steps. Next question to ponder is how many tests to write? While there is no fixed answer, the structure of data helps us here form some good guidelines:

- Test every branch for **booleans and other conditionals**

- Test closed boundaries and mid-points for **intervals**. For example, for Natural[0, 5), test at 0 and any natural number between 0 and 5 (for example 2).

- Test each case for **itemizations and enumerations**

- **Compounds** could have many test cases to illustrate special cases.

- If a **helper** function is used, for example in references to other non-primitive data types, helper function should be tested independently of the calling function. Calling function may assume that helper function is fully tested.

- Always test base case first for **self-references**.

- For non-base cases, test data structures (for example **lists**) that contains at least two elements.



