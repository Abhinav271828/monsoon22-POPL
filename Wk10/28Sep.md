---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 28 September, Wednesday (Lecture 14)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

# Abstract Reduction Systems (contd.)
## Properties of ARSs
Consider a system $S = (A, \to)$. We have seen the relations $(\xrightarrow{*}), (\xrightarrow{+}), (\longleftrightarrow), (\xleftrightarrow{*})$.

$S$ is a *Church-Rosser* system iff $x \xleftrightarrow{*} y$ implies $x \downarrow y$ for all $x, y \in A$. For example, if
$$(\to) = \{(x,1) \mid x \in A\},$$
then $S$ has the Church-Rosser property.

$S$ is *confluent* iff $x \xrightarrow{*} y$ and $x \xrightarrow{*} z$ implies $y \downarrow z$. It can be proved that confluence is equivalent to the Church-Rosser property; furthermore, it implies that every element in $A$ has *at most one* normal form.

$S$ is *semi-confluent* iff $x \to y$ and $x \xrightarrow{*} z$ implies $y \downarrow z$.

$S$ is *normalising* iff every $x \in A$ has a normal form $y$.

$S$ is *terminating* iff there are no infinite reduction sequences.

$S$ is *convergent* iff it is terminating and confluent.

## Well-Founded Induction
We know that a property $P$ must hold for all $n \in \mathbb{N}$ if
$$P(0)$$
and
$$P(n) \implies P(n+1)$$
both hold.

Induction, however, is generalisable to other sets – trees and lists are two examples. Both these are self-similar data structures with a base case (the empty tree and empty list respectively).

Induction over a set can be formulated in terms of an abstract reduction system. We can say that $x \to y$ iff $P(y)$ can be proved from $P(x)$ (reverse implication), and $0$ is a normal form ($P(0)$ cannot be proved from any number other than 0). Any element $x$ that has a normal form then satisfies $P$.