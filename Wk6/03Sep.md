---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 03 September, Saturday (Lecture 9)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# Scope
The notion of variables immediately necessitates the idea of scope – it defines where an accessed variable has been defined.

Scope is usually modelled as a list of variable-value pairs, or an *environment*. The *current environment* of an evaluation is a composition of all the environments that apply to it.