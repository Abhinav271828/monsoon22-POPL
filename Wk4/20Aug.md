---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 20 August, Saturday (Lecture 5)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# ASTs in Racket
The syntax for defining ASTs in Racket is as follows (the example is of arithmetic expressions involving addition and multiplication):

```rkt
(define-datatype ast ast?
  (num-ast
    (num number?))
  (sum-ast
    (left ast?)
    (right ast?))
  (mul-ast
    (left ast?)
    (right ast?)))
```

Now, the constructors `left` and `right` work as extractors (or observers), and the function `ast?` acts as a predicate.