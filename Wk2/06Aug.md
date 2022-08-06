---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | Monsoon 2022, IIIT Hyderabad
          | 06 August, Saturday (Lecture 3)
author: Taught by Mrityunjay Kumar
header-includes: \usepackage{proof}
---

# Recursive Data Types
Data can be defined in an inductive manner, by specifying a base case (which has no sub-structures) and a structure with similar sub-structures.  

A recursive (or inductive) definition may be *top-down* or *bottom-up*. Consider the definition of the set $S = \{3n+2 \mid n \in \mathbb{N}\}$:
$$(m,n) \in S \iff (m,n) = (0,1) \text{ or } (m-1, n-2) \in S.$$
This is top-down; a bottom-up definition might be that $S$ is the smallest subset of $\mathbb{N}^2$ such that
$$(0,1) \in S.$$
$$(m,n) \in S \implies (m+1, n+2) \in S.$$

These may be casted in terms of *inference rules* or *grammars* – both of these are simply alternative ways to write the bottom-up definition.
 
# Syntax of Programming Languages
## Concrete and Abstract Syntax
A programming language, at its core, requires a mapping from *syntax* (its form) to *semantics* (its function). Syntax has two levels – the *concrete* and the *abstract*.

Concrete syntax is convenient for humans to process and understand, and is usually a string, while abstract syntax is designed for machines to manipulate, usually represented as trees (called *abstract syntax trees* or ASTs).

For example, consider the language of arithmetic expressions. An expression like $(4 + 3) \times 2 + (3 + 4)$ has the string representation `"(4 + 3) x 2 + (3 + 4)"`, and the tree representation `+ x + 4 3 2 + 3 4` (prefix notation).
```
+ --- x --- + --- 4
|     |     |
|     |     3
|     2
+ --- 3
|
4
```

These levels define our approach to languages. We start with the concrete syntax representation (a string), from which a *parser* extracts an abstract syntax representation (a tree). Rules of inference are used to validate this AST.  
An evaluator then processes this representation to produce an output.

## An Example
Consider the following example:
```
<exp> := '(' <exp> ' ' '+' ' ' <exp> ')'
      := <num>
```
where `<num>` is any natural number.

In this grammar, `<exp>`s are *nonterminals* – they are steps in the expansion of an expression that can be parsed further (as a number, which is a *terminal*, or an additive expression).

We can cast this grammar as a set of inference rules as well.

$$\infer{n : \text{AST}}{n : \mathbb{N}}$$
$$\infer{(e_1 + e_2) : \text{AST}}{e_1 : \text{AST} \\ e_2 : \text{AST}}$$