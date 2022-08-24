---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 24 August, Wednesday (Lecture 7)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# Evaluation
An AST can be considered as a graph where each node is a *term*. In the most general sense, they are functions (or constructors) whose arity is equal to the number of outgoing edges.

We can use this term graph to identify the "meaning" of the program according to a predefined semantics of the terms. We aim to transform the term graph to a single node, while preserving the semantics. The set of terms to which the expression could possibly evaluate is called the *semantic domain*.  
Such reduction systems should have certain desirable properties, like termination (do all expressions simplify to a single node?), confluence (does it matter what path is taken to simplify the expression?), and normalisation (does every expression simplify to a normal form?). The relations among these properties are
$$\begin{split}
\text{terminating} &\implies \text{normalising} \\
\text{confluent} &\implies \text{every expression has} \leq 1 \text{ normal form} \\
\text{normalising} + \text{confluent} &\implies \text{every expression has a unique normal form}
\end{split}$$
We would like our reduction to be *convergent* (terminating and confluent).

# An Example: `IF+`
We can define a language `IF+` with the following syntax:
```
exp := n              [NUM_LIT]
    := exp + exp      [PLUS_EXP]
    := exp / exp      [DIV_EXP]
    := if exp exp exp [IF_EXP]
    := b              [BOOL_LIT]
```

The semantic domain of this language consists of numbers and booleans:
```
val := n
    := b

n := NUM

b := TRUE
  := FALSE
```

We include errors in the normal forms of `IF+`. The following erros can be generated:

* The first argument of `if` takes a boolean.
* Both the arguments of `+` and `/` take numbers.
* The second argument of `/` is nonzero.