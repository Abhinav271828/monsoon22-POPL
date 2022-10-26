---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 26 October, Wednesday (Lecture 21)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

# Imperative Transformation
We will see how lambda calculus expressions (programs) can be used as abstract state machines.

## Factorial
Consider the following definition of factorial, which uses an accumulator argument:
```scheme
(define (!/acc n)
  (letrec ([loop (λ (i a)
                   (cond
                     [(= i 0) a]
                     [else (loop (sub1 i) (* i a))]))])
    (loop n 1)))
```
This mimics an ASR where $A = \mathbb{N}^2$ and the reduction relation is defined as
$$(i,a) \to (i-1, ai)$$
for $i \neq 1$.

The call to `loop` is called a recursive *tail call*, since its return value is the return value of the function itself; no further computation is needed to return it. Thus the computation proceeds as
```
!/acc 3
-> loop 3 1
-> loop 2 3
-> loop 1 6
-> loop 0 6
-> 6
```

An imperative version of this function is
```scheme
(define (!/imp n)
  (let ([i n]
        [a 1])
    (let ([done (λ () a)]
          [goto (λ (l) (l))])
      (letrec ([loop (λ ()
                       (cond
                         [(= i 0) (goto done)]
                         [else
                           (begin
                             (set! a (* i a))
                             (set! i (sub1 i))
                             (goto loop))]))])))
        (loop)))
```

This maps to the C program
```c
int f(int n)
{
  int i = n;
  int a = 1;

done:
  return a;

loop:
  if (i == 0)
    goto done;
  else
  {
    a = a * i;
    i = i - 1;
    goto loop;
  }
}
```

This procedure of translation works for any definition that employs only tail calls.

We can modularise this further, defining registers `*n*`, `*i*`, `*a*` and `*answer*`, and a procedure `init`. This collection of definitions is called a virtual machine.

## Step
We can build a *stepper machine* in Scheme, which executes a single instruction of the machine and stops.

```scheme
(define (show)
  (println *n*)
  (println *i*)
  (println *a*)
  (println *answer*))
```

We also define the `*pc*` (program counter) register. We redefine `goto` to use this:
```scheme
(define (goto l)
  (set! *pc* l))
```
where `l` is a thunk, which we use to map labels in imperative code. Now we can define
```scheme
(define (step)
  (*pc*))
```