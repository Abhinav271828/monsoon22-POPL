---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 07 September, Wednesday (Lecture 10)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# The Language `FUNCTIONAL+`
This language is extended from `IF+`. Its abstract syntax is as follows:
```
e ::= v | x
      | if e e e
      | + e e
      | let (x = e) ... e
      | \ x -> e | @ e e
```

The `\ x e` syntax indicates a function abstraction, and the `@ e e` syntax indicates an application. For example, the expression
```
@ (\ x -> (+ x 1)) 5
```
would evaluate to 6.

To evaluate an expression containing a function, we resolve the function to its closure, which includes an environment, and use that environment (extended by the bindings of the formal parameters) to evaluate the body. The environment must be saved with the function when it is declared.
