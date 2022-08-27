---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 27 August, Saturday (Lecture 8)
author: Taught by Mritunjay Kumar
header-includes: \usepackage{proof}
---

# An Example: `IF+` (contd.)
To carry out evaluation for any language, we need to define a semantic domain, rewrite rules, reduction rules, simplification rules and valuation rules.

In the case of `IF+`, the semantic domain consists of numbers and booleans (`true` and `false`). Some examples of rewrite rules are
$$\begin{split}
\overline{n_1} + \overline{n_2} &\to \overline{n_1 + n_2} \\
\text{if } \overline{\text{false}} \text{ } e \text{ } e' &\to e'
\end{split}$$