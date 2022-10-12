---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 12 October, Wednesday (Lecture 17)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

# Lambda Calculus (contd.)
We have seen that a lambda calculus expression may be a variable (identifier), an abstraction or an application. This syntax defines a set
$$\begin{split}
E = \{&x , \\
&x \, y , \\
&\lambda x.x , \\
&\lambda x. \lambda y. (x \, y) , \\
&\dots\} \end{split}$$
of expressions, on which we can then define a reduction relation to create an ARS.

Note that application always associates to the left; thus $(x \, y \, z)$ is equivalent to $((x \, y) \, z)$.  
We also abbreviate repeated abstractions, *e.g*, $\lambda x. \lambda y. (x \, y)$, as $\lambda x \, y. (x \, y)$.

## Properties and Operations
### Replacement
We indicate using the notation $\{x/z\}$ after an expression to indicate that expression with all occurrences of $x$ replaced with $z$. For example,
$$\lambda x.(x \, z)\{x/y\}$$
becomes
$$\lambda y.(y \, z).$$

### Variables
We denote by $\text{vars}(\cdot)$ the set of variables (free or bound) occurring in an expression.  
We denote by $\text{FV}(\cdot)$ the set of free variables of an expression.
$$\begin{split}
\text{FV}(\lambda x. x) &= \{\} \\
\text{FV}(x \, (\lambda x.(x \, y))) &= \{x, y\} \end{split}$$
If $\text{FV}(e) = \{\}$, then $e$ is called a *combinator*.

### $\alpha$-Equivalence
Consider the two terms $\lambda x.x$ and $\lambda y.y$ – syntactically, they are different, but we know they both represent the same function, *i.e.*, the identity function.

For this reason, when drawing the syntax tree of a lambda term like $\lambda x \, y. (x \, y)$, we simply write `var` at the leaf nodes and link them back to the lambda they are bound to. We do not use the arbitrary names $x$ and $y$ when describing the syntax.

This form of equivalence (modulo the names of bound identifier) is called *$\alpha$-equivalence*. We formally define this notion using the following rules:
$$\begin{split}
y \notin \text{vars}(e) \implies& \\
&\lambda x. e \overset{\alpha}= \lambda y. (e\{x/y\}) \\
e \overset{\alpha}= e' \implies& \\
&\lambda x. e \overset{\alpha}= \lambda x. e' \\
e_1 \overset{\alpha}= e_1' \, \wedge \, e_2 \overset{\alpha}= e_2' \implies& \\
&e_1 \, e_2 \overset{\alpha}= e_1' \, e_2'
\end{split}$$
Furthermore, $\alpha$-equivalence is reflexive, symmetric and transitive.

Henceforth, when we refer to a term $e$, it indicates any term belonging to $e$'s equivalence class under $\alpha$-equivalence.

### Substitution
At first glance, we may assume that
$$(\lambda x. e) \, y$$
is equivalent to $e\{x/y\}$. However, consider the following example:
$$(\lambda y. \lambda x. (+ \, x \, y)) \, x \, 2$$

If we proceed using ordinary replacement, we get
$$(\lambda x. (+ \, x \, x)) \, 2$$
but we know that this is not the same. The free variable $x$ got "captured" by the lambda that binds it in the body.

This motivates the notion of subtitution, which lays some restrictions on replacement. We define substitution as replacement of all *free* occurrences of a variable with another expression. We denote this operation as $e[x/e']$.

Formally, we can apply the following rules.
$$\begin{split}
y[x/e] &= \begin{cases}
e & x = y \\
y & x \neq y
\end{cases} \\
(e_1 \, e_2)[x/e] &= (e_1[x/e] \, e_2[x/e]) \\
(\lambda x. e)[x/e'] &= (\lambda x. e) \\

(\lambda y. e')[x/e] &= \begin{cases}
\lambda y. (e'[x/e]) & y \neq x \wedge y \notin \text{FV}(e) \\
(\lambda z. e'\{y/z\})[z/e] & y \neq x \wedge y \in \text{FV}(e) \wedge z \notin \text{vars}(e) \cup \text{vars}(e') \cup \{x,y\}
\end{cases}
\end{split}$$