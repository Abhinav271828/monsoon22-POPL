---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 24 September, Saturday (Lecture 13)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

[TRaAT Chapter 1]
# Abstract Reduction Systems
We have seen that a program is nothing but a tree, and running it is simply a traversal of this tree. We formalise this through the notion of ARSs (abstract reduction systems): an ARS is a pair $S = (A, \to)$, where
$A$ is a set and $(\to)$ is a binary relation (called a *reduction relation*) on A. This can be visualised as a graph over the elements of $A$.  
A single pair $(a,b) \in (\to)$ is called a *reduction*.

Examples of this include:

* $A = \mathbb{N}; (\to) = \{(x, 2x) \mid x \in \mathbb{N}\}$
* $A = \mathbb{N}_2; (\to) = \{(a,b) \mid \exists k \in \mathbb{N}_2 : a = kb\}$
* $A = \{a,b\}^*; (\to) = \{(\alpha ba \beta, \alpha ab \beta) \mid \alpha, \beta \in \Sigma^*\}$

ARSs model computation – every step of a computation is represented by a reduction.

We can consider a *relational algebra*, where the identity is $R^0 = \{(a,a) \mid a \in A\}$.  
**Homework.** Prove that $R \circ R^0 = R^0 \circ R = R$ and $R \circ R^{-1} = R^0$.

We can define the reflexive and transitive closures of relations as well:
$$\begin{split}
R^{n+1} &= R \circ R^n \\
R^* &= \bigcup_{n \geq 0} R^n \text{ [reflexive-transitive closure]} \\
R^+ &= \bigcup_{n > 0} R^n \text{ [transitive closure]}
\end{split}$$

An element of $R^*$ is a *simplification* (nonstandard terminology).  
Under an ARS $(A, \to)$, we say that $y \in A$ is a *normal form of $x \in A$* if $x \xrightarrow{*}y$ and $y$ is irreducible. A *normal form of $A$* itself is any irreducible element of $A$.  
For instance, in example 2 above, all prime numbers are normal forms. In this case, we have
$$\begin{split}
(\xrightarrow{+}) &= (\to) \\
(\xrightarrow{*}) &= (\to) \cup (\xrightarrow{0}) \\
&\supseteq (\to)
\end{split}$$

Further, we call $x, y \in A$ *joinable* if there exists $z \in A$ such that $x \xrightarrow{*} z$ and $y \xrightarrow{*} z$. This is denoted $x \downarrow y$.