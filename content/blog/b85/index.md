---
title: "Organizational units of an OOP program"
date: 2021-07-06T06:02:39-05:00
draft: false
tags: ["Design", "Java"]
---
We will use Java as a medium to see how an object-oriented program is typically organized. 

### Tokens
These are the valid words and punctuation marks that make up a language. For example, in english, *hello* is a word but *lkjdlf* is not. Similarly in Java, `public` is a valid token but `\` by itself is not. In this sense, tokens are the most fundamental building blocks of a program.

### Expressions
Expressions are the core of any programming language and are combinations of various tokens like operators, variables and values. For example, in Java, `2 + 3` is a valid expression but `+ 2 3` is not. Expressions are evaluated by the language to get a *value* the type of which is also the type of expression. We can think of expressions as phrases.

### Statements
If expressions can be thought of as phrases then statements are complete sentences. A statement contains expressions and perform basic actions in a program. There are various kinds of statements:
- **Import statement**: Imports class(es) from other packages as discussed below.
- **Print statement**: Displays the results of an expression either on screen or stores in a file.
- **Declaration statement**: Declares a new variable.
- **Assignment statement**: Assigns a new value to an existing variable.

### Methods
Methods are named sequence of statements that can be run as often as needed by *calling* the method. 

### Classes
A class is a collection of related methods. Everything resides inside a class in Java. In the example below, we use methods from two different classes - `Scanner` and `System` and create a new class - `Convert3`

### Packages
A package is a collection of related classes. In the example below, we make use of a class from a package called `java.util`. As the name suggests, this package might contain *utility* classes.

## An example program
The following example program illustrates all the units discussed above.

```java
// import statement
// import class(es) from other packages
import java.util.Scanner;

// create a new class
public class Convert3 {
  // create a new method, main, inside class Convert3
  public static void main(String[] args) {
    /**
     * Converts temperature in degree Celcius to temperture in Kelvin.
     * */
    
    // declaration statement
    // declare new variables of type double
    double celcius, kelvin;
    
    // declare a new variable of type Scanner
    Scanner in = new Scanner(System.in);
    // print statement
    System.out.print("How many degreec C? ");
    // assignment statement
    // read one double value using a method of Scanner class
    // update the stored value of celcius to new value
    celcius = in.nextDouble();
    
    // another assignment statement
    // compute expression on the right hand side
    // update the stored value of kelvin to new value
    kelvin = celcius + 273.15;
    
    // print statement
    System.out.printf("%.2f C = %.2f K\n",
                      celcius, kelvin);
  }
}
```
