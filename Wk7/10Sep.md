---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 10 September, Saturday (Lecture 11)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# The Language `RECURSIVE`
We extend `FUNCTIONAL` with the `recfun` construct to allow us to define recursive functions. When a recursive function is defined, we save it with a closure that includes its own declaration; thus its body can be evaluated in the function call.