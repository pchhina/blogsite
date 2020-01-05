---
title: "Binary Search"
date: 2020-01-05T12:49:51-06:00
tags: ["Algorithms", "JavaScript"]
draft: false
---
Let's say we want to search for an entry(username, id etc.) in an array, a
simplest way one could think of is linear search - where you inspect every
single element of the array starting from the first until you find the value you
are looking for. The question is how long it will take us to find the value.
Well, in worst case, where the value is either at the end of the array or is not
in the array, it will take us n attempts for an array of n elements. In computer
lingo, they say the worst-case time complexity of linear search is $O(n)$. So to
find a value that is at the end of a million element array, it will take us
million time steps.

## Can we do better?
It turns out that there is a simple but brilliant solution to the search
problem - Binary Search. The assumption in binary search however is that the
data is presorted in the array from smalled to largest value. There are ways to
efficiently sort the array data the details of which I will learn soon but at
this time let's say our array is presorted.

## How it works?
The alogorithm checks the mid-point value and compares it with the item we are
searching for. If the value we are looking for is smaller than the mid-point
value, we discard the right half of the array and repeat the search process on
the left half. Each time we fail to find the value, we cut down the search space
into half. In other words, as the size of the problem(n) increases, the time it
takes to search for the value increases only by factor of log(n). So we say the
time complexity of binary search is $O(log_{2}n)$. 

## Pseudocode
```javascript
function BinarySearch(v, s)
    i = 1
    j = LENGTH(v)
    while (i <= j)
        m = floor((i + j) / 2)
        if s == v[m]
            return true
        else if s < v[m]
            j = m - 1
        else
            i = m + 1
    return false
end function
```

## JavaScript Implementation
```javascript
// binary.js -- implementation of binary search algorithm

function BinarySearch(array, x) {
	// return a Boolean: true if x is in array, and false otherwise
    var i = 0;
    var j = array.length - 1;
    while (j >= i)
    {
        var m = Math.floor((i + j) / 2); // find the mid-point of array
        if (x == array[m])
            return true;
        else if (x < array[m])
            j = m - 1; // select first half of the array for next iteration 
        else
            i = m + 1; // select second half of the array for next iteration
    }
    return false;
}

test_array = [1, 5, 8, 11, 13, 23, 25, 39, 40, 100];
console.log(BinarySearch(test_array, 39));
console.log(BinarySearch(test_array, 100));
console.log(BinarySearch(test_array, 1000));

```
## Output
```javascript
true
true
false
```
