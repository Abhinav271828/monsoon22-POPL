---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | Monsoon 2022, IIIT Hyderabad
          | 03 August, Wednesday (Lecture 2)
author: Taught by Mrityunjay Kumar
---

# Introduction (contd.)
## Imperative and Declarative Programming
The various ways of looking at programming correspond to different programming paradigms – imperative or declarative.

**Imperative programming** is based on sequences of instructions that specify *how* to complete a task by means of detailed instructions (which increases the chance of errors). Most common languages (C, C++, Python) fall in this paradigm. In this paradigm, optimisation is easier *for the programmer*.  
```
function fact(N) =
    A = 1
    for I in [1..N]
        A = A * I
    return A
```

**Declarative programming** specifies *what* is to be done. This is less optimisable by the programmer and more optimisable *by the compiler* (or interpreter).
```
function fact(N) = product [1..N]
```
To an extent, the declarative programmer has less control over the lower levels of the program's execution than in the case of imperative programming.

Consider the example of a function that finds the first multiple of five in a list.
```py
def mul_of_5(n):
    return (n % 5 == 0)

def check1(lst):
    for a in lst:
        if mul_of_5(a):
            return a
    return 'No multiple of 5'

def check2(lst):
    return list(filter(mul_of_5, lst))[0]
```

Here, `check1()` is more imperative, as it specifies down to the level of individually processing each element what exactly is to be done. `check2()`, on the other hand, only specifies what is needed: the first element in the list of multiples of five occurring in the input list.


#### Currying
An important concept of declarative (or functional) programming is *currying* – the process of transforming a multi-argument function into a single-argument one. This is possible by allowing the function to return *another function*, which takes the second argument as its only argument, and so on. In abstract terms, it relies on the idea that $A \to B \to C$ (more clearly, $A \to (B \to C)$) is isomorphic to $A \times B \to C$.

Thus, imperative programming provides more control at the cost of increased work and chance of error. The philosophy of declarative programming, on the other hand, is to provide functions to abstract away details, which allows programs to resemble and map to the problem statement itself.

## Functional Programming
Functional programming is a paradigm that enables declarative programming – allowing us to write more concise, more readable code, with fewer bugs. It has two important attributes:

* Pure functions: Pure functions have *referential transparency* – the output depends *only* on the input, and not on any other external factors. Examples of impure functions are `rand()` or `input()`. This allows us to reason about the operation of the program, and facilitates memory optimisations in some ways.  
  Further, they have no side-effects. No state of the program (more generally, of the machine) is changed as a result of a function call.
* Higher-order functions: Functions can be taken as input and returned as output, enabling us to code at a higher level of abstraction. In other words, functions are *first-class citizens*.