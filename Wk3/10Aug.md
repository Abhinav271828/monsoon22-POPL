---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 10 August, Wednesday (Lecture 4)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

[**Recording**](https://www.youtube.com/playlist?list=PL8C7LmL6BGm3GRXdtIaw6qtqFhU3U10Dl)

# Basic Ideas of Functional Programming
We will illustrate some basic notions of functional PLs through Racket.

## Scope
*Scope* is a property of variables that link the place where they are used (the reference) to the place where they are declared (the binding). For example, in
```lisp
(let ([y 3])
  (+ x y))
```
the second `y` is a reference to the binding introduced by the first `y`. Here, `y` is said to be a *bound identifier*, and `x` (which has not been defined in this snippet) a *free identifier*.  
`let` is a binding construct in this program. It introduces *local* bindings.

```lisp
(let ([y 6]
      [z (sub1 y)])
  z)
```
Here, the second `y` is *not* a reference to the first `y`. It expects a value of `y` defined outside.

Linking references to bindings does not require us to actually run the program; it can be identified at compile time. Thus it is known as *lexical* or *static* scope.

## Recursion
We can illustrate recursion with a function `occurs-in?`, which finds if an element is present in a list or not. It should behave such that, for example,
```lisp
(occurs-in? 5 '(3 4 5))
```
evaluates to `#t`, and
```lisp
(occurs-in? 5 '())
```
evaluates to `#f`.

We can define it as follows.
```lisp
(require racket/list)
(define occurs-in?
  (lambda (x ls)
    (if (null? ls)
        #f
        (if (eq? x (first ls))
            #t
            (occurs-in? x (rest ls))))))
```
Alternatively, we can write
```lisp
(define occurs-in?
  (lambda (x ls)
    (cond
      [(null? ls) #f]
      [(eq? x (first ls)) #t]
      [else (occurs-in? x (rest ls))])))
```

These approaches, however, are basically equivalent in terms of bindings; the only difference is the `cond` in place of the `if`.

Another approach uses *local bindings* alone, as opposed to the above two.
```lisp
(define occurs-in?
  (lambda (x ls)
    (letrec ([loop (lambda (ls)
                     (cond
                       [(null? ls) #f]
                       [(eq? x (first ls)) #t]
                       [else (loop (rest ls))]))])
            (loop ls))))
```

## Higher-Order Functions
We can write functions that take functions as inputs. For example, the following function returns the composition of its inputs:
```lisp
(define compose
  (lambda (f g)
    (lambda (x)
      (f (g x)))))
```

Such functions are called *higher-order* functions.