---
title: "Mastering Atomic Vectors in R"
date: 2018-03-11T19:14:27-05:00
draft: FALSE
tags: ["R"]
---


> Everything that exists in R is an `object` and anything that happens is a `function` call.

### Creating Our First Atomic Vector
We are going to create and manipulate vectors using existing functions and the resulting objects will be atomic vectors. To create the objects (atomic vectors in our case), we will use an assignment operator (<-). So we create our first object (*num_one*), and assign it a value of 3:


```r
num_one <- 3
```
Atomic vectors can only have one 'type' of data, and we start with `numeric` data type since this will be the most common one for our data analysis work. Our simple vector contains only one element (commonly called a scalar), but in all respects, it is a vector in R. `Expressions` on the right side are evaluated first and then assigned to the object name on the left. Let's look at an expression evaluation:


```r
num_one_log <- log(3)
```

log(3) is evaluated first and then assigned to an object named *num_one_log*. Let's create a slightly longer vector:


```r
num_five <- c(2, 15, -8, 0, -65)
```

Now we have a vector containing five elements instead of one. Here we make use of a function, `c`, that 'concatenates' the objects that are fed to it as its `arguments`. A function is 'called' by typing its name (in this case its just *c*) followed by one or more arguments typed inside parenthesis.

### Peeking into our Vector

Let's look into what kind of vector it is:


```r
str(num_five)
```

```
##  num [1:5] 2 15 -8 0 -65
```

`str` function gives a concise information of what kind of object we are dealing with. In this case, we have a numeric (num) object of length 5 ([1:5]). The above output is a good visual information but sometimes we want to use this output later in our work so it is good to know functions that extract type and length of the vector separately:


```r
typeof(num_five)
```

```
## [1] "double"
```

```r
length(num_five)
```

```
## [1] 5
```

Note that 'numeric' type is further divided into `double` and `integer`. Sometimes we want to do a `logical` test to see whether a vector is of particular type before carrying out any further computations:


```r
library(purrr)
is_numeric(num_five)
```

```
## Warning: Deprecated
```

```
## [1] TRUE
```

Above we used a function `is_numeric` from `purrr` package. There are equivalent functions in the base package too but slightly less consistent.

### Objects as Arguments to Functions

Objects can be arguments to a function:


```r
long_vec <- c(num_five, num_one, num_one_log)
long_vec
```

```
## [1]   2.000000  15.000000  -8.000000   0.000000 -65.000000   3.000000   1.098612
```

Here we created a longer vector using existing vector objects as arguments to *c* function. This provides us a framework for doing more interesting work.

### Other Ways of Creating a Vector

In addition to creating a vector from numbers or concatenating existing vectors, we can create sequences, repeat vectors, or pick numbers from common probability distributions:


```r
seq_simple <- -5:10
seq_interval <- seq(from = 3, to = 50, by = 0.5)
seq_length <- seq(from = 3, to = 50, length.out = 20)
rep_each <- rep(num_five, each = 2)
rep_times <- rep(num_five, times = 2)
norm_std <- rnorm(25)
norm_unique <- rnorm(25, mean = 10, sd = 5)
```
First we created a simple sequence of whole numbers. Then we used `seq` function to create a sequence with arbitrary start and end points and fixed interval between the elements. After that we created another one (seq_length) with desired number of elements between arbitrary start and end points. Then using `rep` function, we created a vector by repeating each element twice and then by repeating the vector twice. In the end we created a vector (norm_std) with 25 elements from a standard normal distribution using `rnorm` and lastly created a vector with non-standard normal distribution.

### Vector Operations

Now that we know how to create vector objects, let's play with these objects a bit and see what kind of operations we can perform on our vectors. We will use one of the vectors we created earlier:


```r
rep_times
```

```
##  [1]   2  15  -8   0 -65   2  15  -8   0 -65
```

**Arithmatic Operations**

We can do all kinds of math operations with our vectors. We are not going to store these in new objects but watch these *function calls* in action:


```r
rep_times ^ 2
```

```
##  [1]    4  225   64    0 4225    4  225   64    0 4225
```

```r
rep_times * 1 / 5
```

```
##  [1]   0.4   3.0  -1.6   0.0 -13.0   0.4   3.0  -1.6   0.0 -13.0
```

Note that the way these operations are handled is slightly different than you might think. \*, ^ and other operators are *vectorized* - so first the number 2, which is a vector of length one, is *recycled* to make it the same length as the larger vector (ten in this case). Then each element of first vector is operated upon with the corresponding element of the second vector.

There is an interesting math operator called *modulo operator* (`%%`) - which is used to get different parts of answer of division operation. If you divide two numbers, top number is called *dividend* and bottom number is called *divisor*. The result will be a whole number (called *quotient*), and whatever fraction is left out is *remainder*.


```r
rep_times %% 2      # get remainder of rep_times divided by 2.
```

```
##  [1] 0 1 0 0 1 0 1 0 0 1
```

```r
rep_times %/% 2     # get quotient of rep_times divided by 2.
```

```
##  [1]   1   7  -4   0 -33   1   7  -4   0 -33
```

**Statistical Operations**

R will make you dizzy for the amount of statistical wizardry it packs up its sleeve. We are not going to take a deep dive, not even scratching the surface by any standards:


```r
mean(rep_times)
```

```
## [1] -11.2
```

```r
sum(rep_times)
```

```
## [1] -112
```

```r
sd(rep_times)
```

```
## [1] 29.40446
```

```r
max(rep_times)
```

```
## [1] 15
```

```r
summary(rep_times)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   -65.0    -8.0     0.0   -11.2     2.0    15.0
```

**Other Interesting Operations**


```r
sort(rep_times)
```

```
##  [1] -65 -65  -8  -8   0   0   2   2  15  15
```

```r
sort(rep_times, decreasing = TRUE)
```

```
##  [1]  15  15   2   2   0   0  -8  -8 -65 -65
```

```r
unique(rep_times)
```

```
## [1]   2  15  -8   0 -65
```

```r
rev(rep_times)
```

```
##  [1] -65   0  -8  15   2 -65   0  -8  15   2
```

### Chaining of Functions

The output of one function can be used as an input to another function. Say we want to find standard deviation of 50 standard normals:


```r
sd(rnorm(50))
```

```
## [1] 0.9552438
```

You probably want to avoid more convoluted chains as it will be confusing to read.

### Selecting Few Elements of a Vector (Subsetting)

So far we have created vectors using a variety of functions. Now, if a vector is given to us and we are tasked with selecting certain elements from it, how do we do that. There are two useful ways to do that:

**Subsetting using position**

In R, elements of a vector are positioned (*indexed*) from 1 (many languages start at 0) to the length of the vector. Our *rep_times* vector has 10 elements, and say we want to extract first and last element, we can feed these (*index values*) as integer vector inside square brackets:


```r
rep_times[c(1, length(rep_times))]
```

```
## [1]   2 -65
```

If we want to select everything but the first and last element, you can put a '-' sign before the vector:


```r
rep_times[-c(1, length(rep_times))]
```

```
## [1]  15  -8   0 -65   2  15  -8   0
```

A more powerful way of subsetting is through logical expressions.

**Subsetting using logical expressions**

Say if we had a light bulb that we could turn on if we want an element in that position and off if we don't, it would be great. It turns out that R (and generally any programming language) has a special data type for carrying out logical operations. Let's say we want to extract all odd numbers from our *rep_times* vector:


```r
odd_filter <- rep(c(TRUE, FALSE), 5)
str(odd_filter)
```

```
##  logi [1:10] TRUE FALSE TRUE FALSE TRUE FALSE ...
```

```r
is_logical(odd_filter)
```

```
## [1] TRUE
```

```r
rep_times[odd_filter]
```

```
## [1]   2  -8 -65  15   0
```
Cool, but most of the times we will not create this *switch* vector manually. Instead, `logical expressions` will be used to do this job, and this will serve as a handy tool in our R toolbox. Let's look at some logical expressions and their output:


```r
rep_times > 0
```

```
##  [1]  TRUE  TRUE FALSE FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE
```

```r
rep_times < -10
```

```
##  [1] FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE
```

```r
rep_times == 0
```

```
##  [1] FALSE FALSE FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE
```

```r
rep_times %% 2 != 0     # selecting odd numbers
```

```
##  [1] FALSE  TRUE FALSE FALSE  TRUE FALSE  TRUE FALSE FALSE  TRUE
```
The output of these logical expressions is either TRUE or FALSE depending on whether the logical condition is satisfied or not. Now we can feed the output of these expressions inside square brackets for subsetting our vector:


```r
rep_times[rep_times < -10]
```

```
## [1] -65 -65
```
**Logical expressions can be combined using and (&) and or (|) operators**


```r
(rep_times > 0) | (rep_times < -10)
```

```
##  [1]  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE
```

```r
(rep_times > 0) & (rep_times %% 2 != 0)
```

```
##  [1] FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE FALSE FALSE
```

### More Uses of Logical Subsetting

**TRUE = 1 and FALSE = 0**

Behind the scenes, TRUE is encoded as 1 and FALSE is encoded as 0. This means we can do some neat math operations on logical vectors. Let's say we want to know how many values are positive in my vector:


```r
sum(rep_times > 0)
```

```
## [1] 4
```

Or say what proportion of values are non-zeros:


```r
mean(rep_times != 0)
```

```
## [1] 0.8
```

**Reassignment of some elements of a vector**

Sometimes we might need to reassign specific elements of a vector to new values. For example, let's say, we want to replace all 0's in our vector to -99:


```r
rep_times[rep_times == 0]  <- -99
rep_times
```

```
##  [1]   2  15  -8 -99 -65   2  15  -8 -99 -65
```
**Any or all values matching a condition**

Let's say we just want to know if any of the values in our vector are negative:


```r
any(rep_times < 0)
```

```
## [1] TRUE
```

Or, if all of the values are non-zeros:


```r
all(rep_times != 0)
```

```
## [1] TRUE
```

### Two Types of Vectors
Ok, now we know two types of atomic vectors: `numeric`, one that contains numeric numbers and `logical`, that contain logical values of either `TRUE` or `FALSE`.

### Pick Elements Randomly
So far we have looked at subsetting of vectors where we know which element we need either by their *index* or by *logical subsettin*. Sometimes we want to randomly select some elements, and `sample` function will come to our rescue here:


```r
sample(rep_times, 5)    # randomly select five elements from rep_times
```

```
## [1]   2 -99  -8  15  15
```

**Select certain fraction of elements**


```r
sample(rep_times, 0.4 * length(rep_times))    # 40% of elements
```

```
## [1]  -8 -65 -65  15
```

**A quick shortcut to shuffle integers**


```r
sample(10)
```

```
##  [1]  9  5  4 10  1  2  6  7  8  3
```

### Some Special Values

There are some special values that our two types of vectors can take, so we will discuss those here.

**NA - Not Available**

In real data, we do not always get nice clean numbers - some data points will be missing. A sensor could go bad, there could be a power failure during recording of data, a subject selected to interview is not at home etc. Before we decide what to do with these missing values, we need to understand some basic information on these values, for example how many or what proportion of the values are missing. R represents this missing data with `NA`. In reality the data exists, but we just don't know what those values are.

Let's first randomly assign some values as NA to our vector:


```r
rep_times[sample(length(rep_times), 4)] <- NA
rep_times
```

```
##  [1]   2  NA  -8  NA -65   2  NA  -8 -99  NA
```

Now to find how many or what proportion of NAs we have, we could use `is.na` function:


```r
sum(is.na(rep_times))
```

```
## [1] 4
```

```r
mean(is.na(rep_times))
```

```
## [1] 0.4
```

NAs are contagious:


```r
sum(rep_times)
```

```
## [1] NA
```

```r
mean(rep_times)
```

```
## [1] NA
```

But R provides arguments in these functions, and others, to ignore NAs while computing these statistics:


```r
sum(rep_times, na.rm = TRUE)
```

```
## [1] -176
```

```r
mean(rep_times, na.rm = TRUE)
```

```
## [1] -29.33333
```

*What to do with these NAs?*

There are two basic treatments we can do with vectors containing NAs. 

1. We can remove those observations:


```r
rep_times_na_removed <- rep_times[!is.na(rep_times)]
rep_times_na_removed
```

```
## [1]   2  -8 -65   2  -8 -99
```

2. We can replace NAs with the mean of rest of observations:


```r
rep_times_clean <- rep_times
rep_times_clean[is.na(rep_times_clean)] <- mean(rep_times_clean, na.rm = TRUE)
rep_times_clean
```

```
##  [1]   2.00000 -29.33333  -8.00000 -29.33333 -65.00000   2.00000 -29.33333
##  [8]  -8.00000 -99.00000 -29.33333
```
**Inf and NaN**

Although not created intentionally, some chain of math operations in our code may lead to situations where we are dividing by 0. We want to avoid these situations since this will lead to errors and exceptions. So it is a good idea to check for these conditions on inputs before using the inputs in our functions. `is.infinite` and `is.nan` functions can be used to check these conditions:


```r
undesirable <- c(0, 1) / 0
undesirable
```

```
## [1] NaN Inf
```

```r
is.infinite(undesirable)
```

```
## [1] FALSE  TRUE
```

```r
is.nan(undesirable)
```

```
## [1]  TRUE FALSE
```
### Sequences for Loops

Sometimes we need a vector of indices of a vector so we can do iterations, loops etc. `seq_along` will be a useful function for that. This will generate an index vector 1, 2,..., length(vector).


```r
seq_along(rep_times)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

### Creating an empty vector

We may want to create an empty vector sometimes. This will be useful when we want to populate the vector using loops.


```r
new_num <- vector("numeric", 10)
new_log <- vector("logical", 10)
new_num
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0
```

```r
new_log
```

```
##  [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
```

### Some Useful Vector Transformations

**Recenter around mean 0**


```r
data <- sample(1:20, 10) * sample(10)
data - mean(data)
```

```
##  [1]  -7.1  -5.1  66.9 -61.1 -10.1 116.9 -63.1  -9.1  -3.1 -25.1
```

**Recenter around mean 0 and standard deviation 1**


```r
(data - mean(data)) / sd(data)
```

```
##  [1] -0.13021521 -0.09353487  1.22695737 -1.12058439 -0.18523572  2.14396587
##  [7] -1.15726473 -0.16689555 -0.05685453 -0.46033827
```

**Rescale so all values are between 0 and 1**


```r
(data- min(data)) / (max(data) - min(data))
```

```
##  [1] 0.31111111 0.32222222 0.72222222 0.01111111 0.29444444 1.00000000
##  [7] 0.00000000 0.30000000 0.33333333 0.21111111
```

### Comparing Two Vectors

Sometimes we just want to check if two vectors are equal. `all.equal` can be a useful function for that:


```r
x1 <- c(1, 2, 3)
y1 <- c(1, 2, 3)
y2 <- c(2, 3, 4)
all.equal(x1, y1)
```

```
## [1] TRUE
```

```r
all.equal(x1, y2)
```

```
## [1] "Mean relative difference: 0.5"
```

### Third Type of Atomic Vector

The third and last type of atomic vector is `character` which is very powerful in data analysis as well as general programming. Although we have not created this type of vector so far explicitly, note that the output of some functions that we have used was a string of characters:


```r
x <- typeof(rep_times)
x
```

```
## [1] "double"
```

```r
typeof(x)
```

```
## [1] "character"
```

```r
is_character(x)
```

```
## [1] TRUE
```
Mastering `Regular Expressions` is key to working with character data so we will learn the basics here. We are going to use `stringr` package to play with our character vectors.


```r
library(stringr)
```
You can create a character vector by entering anything inside quotes. We are going to use double quotes for consistency:


```r
greet <- "hello!"
greet
```

```
## [1] "hello!"
```

```r
typeof(greet)
```

```
## [1] "character"
```

```r
is_character(greet)
```

```
## [1] TRUE
```
Alright, let's create another vector and combine the two with `c` just like we did earler for numeric vectors:


```r
subject <- "world"
greetings <- c(greet, subject)
greetings
```

```
## [1] "hello!" "world"
```

```r
length(greetings)
```

```
## [1] 2
```

So our greetings vector contains two *string* elements. It is interesting to note that both strings can have arbitrary number of characters. Let's say we want to know how many characters each string element has, we will use `str_length` function:


```r
str_length(greetings)
```

```
## [1] 6 5
```
**Combining elements of a vector**

If we want to combine the individual elements of a single character vector, we will use `str_c` function with **collapse** argument:


```r
str_c(greetings, collapse = "")
```

```
## [1] "hello!world"
```
**Combining elements of two vectors**

If we want to combine two character vectors (element wise), we will use the same `str_c` function but with **sep** argument:


```r
str_c(greetings, "!", sep = "")
```

```
## [1] "hello!!" "world!"
```

### Sorting Character Vectors and Changing Case

We will use data set, `words` available in *stringr* for this exercise:


```r
word_sample <- words[sample(length(words), 5)]
word_sample
```

```
## [1] "do"      "similar" "tax"     "general" "king"
```

```r
str_sort(word_sample, locale = "en")
```

```
## [1] "do"      "general" "king"    "similar" "tax"
```

```r
str_to_lower(word_sample)
```

```
## [1] "do"      "similar" "tax"     "general" "king"
```

```r
str_to_upper(word_sample)
```

```
## [1] "DO"      "SIMILAR" "TAX"     "GENERAL" "KING"
```

```r
str_to_title(word_sample)
```

```
## [1] "Do"      "Similar" "Tax"     "General" "King"
```

### Pattern Matching in Character Vectors using Regular Expressions

Now we are going to scratch the surface of regular expressions. Regular expressions is a subject in itself in computing world but we will cover minimum stuff just to get us started with cleaning data where needed and not worry (yet) about text analysis.

> Everything in ASCII (or Unicode) is a *character* (letters, digits, spaces, punctuations, etc.) and sequence of characters is called a *string*.

*stringr* has different functions to do different things using regular expressions but they have the same format. We will use one, `str_extract`, which extracts the matched pattern. At the end we will apply our pattern matching skills to other tools. Let's begin by picking five random strings from `sentences` data available in *stringr*. Note that we are using `set.seed` function, this lets us repeatedly get the same set of sentences:


```r
set.seed(1234)
sent_five <- sentences[sample(length(sentences), 5)]
sent_five
```

```
## [1] "ii cloud of dust stung his tender eyes."        
## [2] "Oak is strong and also gives shade."            
## [3] "A whiff of it will cure the most stubborn cold."
## [4] "The pods of peas ferment in bare fields."       
## [5] "The cone costs five cents on Mondays."
```
**Literal match - just type what you want to match**


```r
str_extract(sent_five, "northern")
```

```
## [1] NA NA NA NA NA
```

**Use dot (.) to match any character**

Here we introduce *regex*'s first metacharacter (something that is not matched literally but indicates some pattern). "." will match *any single character*. Let's say we are interested in grabbing ten characters following *the*:


```r
str_extract(sent_five, "the..........")
```

```
## [1] NA              NA              "the most stub" NA             
## [5] NA
```

It picked *the* and the following ten characters including spaces. Note that it did not pick *The* from third sentence because it starts with a capital T. If we want to disregard the case, there are multiple ways but let's create a *character class* in this case to solve the problem.

**Use character class [ ] to match one of the characters defined in the class**:


```r
str_extract(sent_five, "[Tt]he..........")
```

```
## [1] NA              NA              "the most stub" "The pods of p"
## [5] "The cone cost"
```
Now it matched either t or T followed by he followed by any ten characters. Great, now let's say we want to pick sentences that begin with The or the. In this case, it will only be the third sentence.

- appending ^ to pattern inside [] will match all characters *excluding* those defined in the class.

**^ to match the beginning of the string**

If you want to match a pattern only at the beginning of the string, you would prepend your pattern with ^:


```r
str_extract(sent_five, "^[Tt]he..........")
```

```
## [1] NA              NA              NA              "The pods of p"
## [5] "The cone cost"
```

**$ to match the end of the string**

Append your pattern with \$ to match it only at the end of the string:


```r
str_extract(sent_five, "mouse$")
```

```
## [1] NA NA NA NA NA
```
Hmm..it did not match our first string that we were hoping for. Aahhh, we missed the period (.). All strings are ending with a period so we have to include that too in our pattern, but remember that period already is a metacharacter. So how do we let regex engine know that we want the literal period. Well, we append our metacharacter with an *escape character*, \\. One problem here is that \\ is also used in our strings and regular expressions are string arguments to our functions. We we have to escape the escape character!

> To match regex's metacharacter literally, append it with two escape charaters: \\\\ 


```r
str_extract(sent_five, "mouse\\.$")
```

```
## [1] NA NA NA NA NA
```
**Use `writeLines` to check if your regex pattern is what you expected**

Since these escape characters can cause confusion on what the regex engine is finally seeing, it is a good idea to feed our argument to `writeLines` first:


```r
writeLines("mouse\\.$")
```

```
## mouse\.$
```
**Repeating Patterns**

So far we have looked at single character matches. So if we want to match two capital letter in the following vector, we will define a character class and repeat it twice:


```r
states <- c("AL", "MI", "Hawaii", "ME", "Illinois" )
str_extract(states, "[A-Z][A-Z]")
```

```
## [1] "AL" "MI" NA   "ME" NA
```
This can get messy quickly, say if we want to match five capital letters, or ten digits. It turns out *regex engine* has a very powerful feature of matching repetetive patterns.

- {n} match preceding character or group exactly *n* number of times:


```r
str_extract(states, "[A-Z]{2}")
```

```
## [1] "AL" "MI" NA   "ME" NA
```

- {m,} match the preceding character or a group m or more number of times:


```r
str_extract(sent_five, "l{2,}")
```

```
## [1] NA   NA   "ll" NA   NA
```

Similar concept applies for {,n} to match up to *n* times and {m,n} to match at least *m* times and at the most *n* times. There are shortcuts for some common repetitions:

- \+ to match 1 or more times:

```r
money <- c("$23", "$ab", "$4000")
str_extract(money, "\\$[0-9]+")
```

```
## [1] "$23"   NA      "$4000"
```
- \* to match 0 to more times:

```r
str_extract(money, "\\$[0-9]*")
```

```
## [1] "$23"   "$"     "$4000"
```

- ? to match 0 or 1 times:

```r
days <- c("sunday", "mon", "tue", "wednesday")
str_extract(days, "((sun)|(mon)|(tue)|(wednes))(day)?")
```

```
## [1] "sunday"    "mon"       "tue"       "wednesday"
```
Here we also quickly demonstrated the conecept of or, |, and groups using ()

**More character classes**

Earlier we learned a couple of examples of how to define a character class. Another example is [A-Za-z0-9] will match any letter or digit. There are shortcuts available however for commonly used classes. These when used with repetetion matches results in can result in flexible and powerful patterns:

- \\d will match any single digit:


```r
str_extract(money, "\\$\\d+")
```

```
## [1] "$23"   NA      "$4000"
```

- \\D will match any single non-digit

```r
address <- c("abc street", "NY", "US", "54875")
str_extract(address, "\\D+")
```

```
## [1] "abc street" "NY"         "US"         NA
```
- \\w will match any alphanumeric character including _. This is equivalent to [A-Za-z0-9_]


```r
some_txt <- c("$$Name..", "   $Class--", "$$$Address$$   ")
str_extract(some_txt, "\\w+")
```

```
## [1] "Name"    "Class"   "Address"
```
- \\W will match any non-alphanumeric character

```r
str_extract(some_txt , "\\W+")
```

```
## [1] "$$"   "   $" "$$$"
```
- \\s will match any whitespace (space, tab, newline, carriage return)

```r
str_extract(some_txt , "\\s+")
```

```
## [1] NA    "   " "   "
```
- \\S will match any non-whitespace

```r
str_extract(some_txt , "\\S+")
```

```
## [1] "$$Name.."     "$Class--"     "$$$Address$$"
```
- \\b will match a word boundary (end of word)

```r
str_extract(sent_five, "\\w+\\b")
```

```
## [1] "ii"  "Oak" "A"   "The" "The"
```

### Stringr Tools

Ok, now that we are comfortable using regular expressions, we need to put these into use with the available tools in stringr. We have already used one - `str_extract` which extracts the matched pattern. There are few others that can be very useful.

**Logical output if a match is found**

```r
str_detect(address, "\\D+")
```

```
## [1]  TRUE  TRUE  TRUE FALSE
```
Note that this approach can also be used to simplify regular expressions.

**How many matches are there in the string**


```r
str_count(sent_five, "the")
```

```
## [1] 0 0 1 0 0
```
**Replace a match with new string**

```r
str_replace(sent_five, "the", "***")
```

```
## [1] "ii cloud of dust stung his tender eyes."        
## [2] "Oak is strong and also gives shade."            
## [3] "A whiff of it will cure *** most stubborn cold."
## [4] "The pods of peas ferment in bare fields."       
## [5] "The cone costs five cents on Mondays."
```
**Split a string into pieces**

```r
str_split(sent_five[1], " ")
```

```
## [[1]]
## [1] "ii"     "cloud"  "of"     "dust"   "stung"  "his"    "tender" "eyes."
```
Note that this gave us a `list` object, which we have not studied yet, but you can see the function of splitting a string here.

**Not one but all matches**

All the tools that we have looked at so far will match and give you the *first* occurence in a string. All others are ignored. Above functions have equivalent `_all` commands that will apply to all matches. For example, `str_extract_all` will extract all matches. These will result in a data structure called `list`, which we have not studied yet but will look into in the future.

### Factors - an Augmented Vector Type

Alright, we are going to look at one more vector type, which is not an atomic vector by definition but it occurs so frequently in our data analysis that our vector discussion will be incomplete without it. It is an augmented vector because it is built on top of one of the atomic vectors - `integer`. Often times in our data, we will see variables that are `categorical` in nature, meaning they have discrete categories. For example, sex, month name, color, class etc. These type of data are modeled with `factor`. Let's create a simple factor vector:


```r
sex <- c("M", "M", "F", "M", "F")
sex_fac <- factor(sex)
sex_fac
```

```
## [1] M M F M F
## Levels: F M
```

```r
is.factor(sex_fac)
```

```
## [1] TRUE
```
R automatically assigns `levels` for the factor vector. This assignment will be alphabetical. If we don't want that, we have to define the levels when creating the vector:


```r
sort(sex_fac)
```

```
## [1] F F M M M
## Levels: F M
```

```r
sex_fac2 <- factor(sex, levels = c("M", "F"))
sort(sex_fac2)
```

```
## [1] M M M F F
## Levels: M F
```

**Recoding factor levels**

In the above case, the levels are apparently clear due to clear object name. Sometimes, we may want to recode to make the levels more clear. We will use `fct_recode` function from the package `forcats`:


```r
library(forcats)
levels(sex_fac2)
```

```
## [1] "M" "F"
```

```r
sex_fac2 <- fct_recode(sex_fac2, 
                       "Male" = "M",
                       "Female" = "F")
levels(sex_fac2)
```

```
## [1] "Male"   "Female"
```

**Defining order where it matters**

Some variables will have an implied order. For example, size of a clothing line. We can define the order while creating the vector with `ordered = TRUE` argument:


```r
size <- factor(c("small", "large", "medium", "small", "medium"),
               levels = c("small", "medium", "large"),
               ordered = TRUE)
size[2] > size[1]
```

```
## [1] TRUE
```
**Sumamrize factors**

We can use `summary` function used earlier with numeric vectors to get concise infor about the factor vector as well:


```r
summary(size)
```

```
##  small medium  large 
##      2      2      1
```

### Summary
Now we have with us the fundamental building blocks using which we can understand the existing higher level data structures as well as create some of our own! So this is really exciting. Next we will take a dive into learning some programming concepts using which we can do some more powerful things beyond the one liners that we have been using here.


