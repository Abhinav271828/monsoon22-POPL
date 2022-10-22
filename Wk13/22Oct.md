---
title: Principles of Programming Languages (CS1.402)
subtitle: |
          | 22 October, Saturday (Lecture 20)
author: Taught by Prof. Venkatesh Choppella
header-includes: \usepackage{proof}
---

# Lambda Calculus
## Recursion (contd.)
Recursion in programming languages relies on evaluating the body of the function in an environment in which its name is bound. However, lambda calculus has no identifier and environments; recursion is done in a different way.

First, we define a helper function for the recursive procedure. In the case of factorial, as we have seen, this function has the form
```scheme
(define g
  (lambda f
    (lambda n
      (if (zero? n) 1
          (* n (f (sub1 n)))))))
```
However, this will not help us, since we have nothing to pass to `g` that will call it again. We cannot pass `fac` to it as we do not have it defined.

We can solve this problem by giving it *one more parameter*:
```scheme
(define h
  (lambda f
    (lambda n
      (if (zero? n) 1
          (* n (: f f (sub1 n)))))))
```
so now, if we pass `h` itself to `h`, it will be able to call itself.

Consider, for example, the call `(: H H 2)`. This expands (after evaluating the `if`) to
```scheme
(* 2 (: H H (sub1 2)))
```
in which the second operand of `*` again evaluates to `(: H H 1)`, which reduces to
```scheme
(* 1 (: H H (sub1 0)))
```
which by the `if` condition returns `(* 1 1)`, or 1. Thus the top-level call returns `(* 2 1)`, or 2, which is 2!.  
Hence we can define factorial as `(define !/H (H H))`.

We can define $H$ in terms of $G$ as
$$H_G = \lambda f. (G \, (f \, f)).$$

Now, we would like a function, say $Y$, that could take a function like $G$ and return the factorial function. In essence, $Y$ should identify the fixpoint of its argument (factorial is a fixpoint of $G$ because $(G \, \text{fac}) = \, \text{fac}$.  
Since the return value of $Y$ is a fixpoint of $G$, we know that it must satisfy the property
$$(Y \, G) \equiv G \, (Y \, G).$$

We have seen, however, that since the fixpoint of $G$ is simply factorial, which we know to be $(H \, H)$. Now we can define $H$ in terms of $G$ and therefore write $Y$ as
```scheme
(define y
  (lambda g
    (let ([h (lambda (x) (g (x x)))])
      (h h))))
```

We can see that if we pass $G$ to $Y$, we get
$$\begin{split}
(Y \, G) &= (H \, H) \\
&= (\lambda x.(G \, (x \, x))) \, H \\
&= G \, (H \, H) \\
&= G \, (Y \, G)
\end{split}$$
which satisfies the fixpoint property.

Note that this behaviour of $Y$ is dependent on the reduction order (strategy) – it will not work if we try to evaluate the arguments to a function before passing them (as Scheme does).

## Reduction Strategies
Consider an arbitrary lambda calculus expression. It may have zero, one or many redexes – we need to decide an order in which to evaluate (reduce) these redexes. Many such strategies are possible.

*Normal order reduction* involves reducing the outermost, leftmost terms first. If we write the lambda expression as a tree, this is equivalent to a preorder traversal searching for a redex. For example, under this strategy
$$(\lambda x. t) \, \Omega \overset\beta\to t.$$

*Applicative order reduction* consists of reducing the innermost, leftmost terms first. In terms of the tree representation of lambda expressions, we can describe this as a postorder traversal searching for a redex. For example, under this strategy, $(\lambda x. t) \, \Omega$ never evaluates to anything, because $\Omega$ has no normal form.

*Weak reduction* is a type of reduction where we do not evaluate the body of a lambda abstraction *until it is applied*. In other words, lambdas form a kind of barrier to the evaluation of their body.

Scheme's reduction strategy is applicative order reduction combined with weak reduction; this is called *call-by-value* semantics. Haskell, on the other hand, uses *call-by-name*, which is a combination of normal order reduction and weak reduction.

We can use this property of Scheme to define a $Y$-combinator that works in Scheme. The issue with our previous definition was that the argument to `y`, `(h h)` was being evaluated first, creating a new term `(y (h h))` again. We therefore *delay* the evaluation of `h` by wrapping a new `lambda (n)` around it (creating a *closure*). This gives us the following definition:
```scheme
(define z
  (lambda g
    (let ([h (lambda (x)
               (lambda (n)
                 (g (x x n))))])
      (h h))))
```
This is called the *call-by-value $Y$-combinator*.