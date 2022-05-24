---
title: "1.1.3 Evaluating Combinations"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

One of our goals in this chapter is to isolate issues about thinking procedurally.

In evaluating combinations, the interpreter is itself following a procedure.

To evaluate a combination, do the following:

1. Evaluate the subexpressions of the combination.
2. Apply the procedure that is the value of the operator subexpressions to the arguments that are the values of the other subexpressions (the operands).

Note that the first step says that in order to accomplish the evaluation process for a combination we must first perform the evaluation process on each element of the combination. Thus, the evaluation rule is **recursive** (it needs to invoke itself).

**Recursion** is an evaluation process that includes, as one of its steps, the need to invoke the rule itself, and is a very powerful technique for dealing with hierarchical, tree-like objects.

Have a look at this expression:

{% highlight scheme linenos %}
(* (+ 2 (* 4 6))
   (+ 3 5 7))
{% endhighlight %}

Two things are being multiplied by `*`:

{% highlight scheme linenos %}
(+ 2 (* 4 6))
{% endhighlight %}

and

{% highlight scheme linenos %}
(+ 3 5 7)
{% endhighlight %}

We have to evaluate both of them. In order to evaluate the first, we need to evaluate `2` and `(* 4 6)`, which makes us have to recur again. You can think of the s-expressions (the expressions in parentheses) as a tree with partial evaluations percolating upwards. In programming, this describes **tree accumulation** which is the process of accumulating data placed in tree nodes according to their tree structure.

<p align="center">
  <img width="460" height="300" src="../images/sicp-recursion-tree.png">
</p>

Note also that we are asked to evaluate primitive expressions, and:

- the values of numerals are the numbers that they name. For example: `4` is equal to 4 and `6` is equal to 6.
- the values of built-in operators are the machine instruction sequences that carry out the corresponding operations. For example, the machine knows what to do with `+` and `*`.
- the values of other names are the objects associated with those names in the environment. For example, if we say `(define size 2)`, `size` will then be equal to the value of the number `2`.

## The role of the Environment & Special Forms

The role that the environment itself plays in determining the meaning of the symbols in expressions is crucial. In an interactive language such as Lisp, if you provided an expression like `(+ x 1)` without specifying any information about the environment that would provide a meaning for the symbol `x` (or even for the symbol `+`), it would return to you an error:

{% highlight scheme linenos %}
(+ x 1)

Unbound variable: x
{% endhighlight %}

Furthermore, if you don't specify an environment that has context about operators such as `+`, `-`, `*`, etc., and the context isn't set to the global environment, then the values of those operators do not exist and the operators are therefore meaningless. However, exceptions to this general evaluation rule are called **special forms**, such as `define` in Lisp which is used to define a variable or a procedure.

Therefore, the various kinds of expressions (each with its associated evaluation rule) constitute the syntax of the programming language.
