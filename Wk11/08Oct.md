---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 08 October, Saturday (Lecture 15)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

# Properties of PARLANG
## Termination
For proving the termination of PARLANG programs, we need to define a *measure function* over its terms. We can use the following system, where $| \cdot | : S \to \mathbb{N}^4$.
$$|s| = [\#(|), \#(;), \#(:=), \#(\text{done})]$$
We then use lexicographic order on $\mathbb{N}^4$, denoted $\sqsubset$.

Now, we claim that
$$\langle G, s \rangle \to \langle G', s' \rangle \implies |s'| \sqsubset |s|.$$
We can prove this by induction on the $(\to)$ relation. The following cases arise:

* $\langle G, x := n \rangle \to \langle [x \to n]G, \text{done} \rangle$: Here the size goes from $[0,0,1,0]$ to $[0,0,0,1]$.
* $\langle G, \text{done}; s \rangle \to \langle G, s \rangle$: Here the size goes from $[i,j+1,k,l+1]$ to $[i,j,k,l]$ for some $i,j,k,l \geq 0$.
* $\langle G, s_1 \mid s_2 \rangle \to \langle G, s_1 ; s_2 \rangle$ or $\langle G, s_2 ; s_1 \rangle$: Here the size goes from $[i+1, j, k, l]$ to $[i, j+1, k, l]$.
* If $\langle G, s_1 \rangle \to \langle G', s_1' \rangle$, then $\langle G, s_1 ; s_2 \rangle \to \langle G', s_1' ; s_2 \rangle$: Here the size goes from $|s_1'| + |s_2| + [0,1,0,0]$ to $|s_1| + |s_2| + [0,1,0,0]$. Since, by induction, we know that $|s_1'| \sqsubset |s_1|$, the measure has reduced.

## Non-Confluence
The `PAR` rule makes PARLANG non-confluent. For example, $x := 3; x := 5$ may end with $G = [x \to 3]$ or $G = [x \to 5]$.

## Normalisation
We can show that $s = \text{done}$ is the only normal form, and that all programs reduce to it.

# Lambda Calculus
## Syntax
Lambda calculus has the syntax
$$\begin{split}
e &::= x \\
&::= e \, e \\
&::= \lambda x.e
\end{split}$$

An example of a lambda calculus expression is $(\lambda x. \lambda y. (y \, z))$.