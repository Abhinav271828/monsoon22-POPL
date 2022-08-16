---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 13 August, Friday (Lecture 5)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

[**Recording**](https://www.youtube.com/watch?v=cMxGYUwT6Zg)

# Basic Elements of Racket Syntax
A LISP (or Racket) program is simply structured data. The grammar it follows is:
```
<datum>        ::= <list-exp>  | <non-list-exp>
<list-exp>     ::= (<datum>*)  |
                   (<datum>+ . <datum>)
<non-list-exp> ::= <atom>      | <vector>
<atom>         ::= <boolean>   | <number> |
                   <character> | <symbol> |
                   <string>    | <procedure>
<vector>       ::= #(<datum>*)
```

A *pair* is a `<list-exp>` of the form `(<datum> . <datum>)`. The inbuilt functions `car` and `cdr` extract the first part and the second part respectively.  
Nested pairs are interpreted as lists. For example, `(3 . (2 . 1))` is `[3,2,1]`. Singleton lists can take their `cdr` as the empty list; *e.g.*, `[3]` is represented as `(3 . ())`.

There are some rules we can use to determine if two `<list-exp>`s are equal. This puts the above representations on a formal footing.  
For example, nest-elimination allows us to simplify
```
(<d1> <d2> ... . (<d3> . <d4>))
```
(the *redex*) to
```
(<d1> <d2> ... <d3> . <d4>)
```
Note that nest-introduction is the name of the reverse substitution.

Also, by `()`-elimination, we can substitute
```
(<d1> <d2> ... . ())
```
with
```
(<d1> <d2> ...)
```
and vice versa by `()`-introduction.

The syntax of lists can be expressed formally as
```
<list> ::= (<datum>*) |
           (<datum>+ . <non-list-exp>)
```
There are two kinds of lists: *proper lists*, which are simply lists of datums, and *improper lists*, which end with non-list expressions.

A list expression is in *list normal form* if there are no elimination redexes in it. Conversely, a list expression is in *pair normal form* if there no introduction redexes in it. Internally, all lists are in pair normal form.