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

We know that a **[combination](/wiki/cs/sicp/ch1/1.1/expressions_and_combinations/)** is an expression that is formed by forming a list of expressions and includes both operators and operands.

Now we need to know that a **compound expression** is simply a type of combination that we define.

One of our goals in this chapter is to isolate issues by thinking procedurally. We want to understand how these procedures work step-by-step.

Therefore, in evaluating combinations, we understand that the interpreter is itself following a procedure, because it is a program itself, but we don't want to let the interpreter do all the thinking for us – we want to understand how to do these evaluations before we give them to the interpreter. So, to evaluate a combination, we do the following:

1. Evaluate the subexpressions of the combination.
2. Apply the procedure that is represented by the operator subexpressions to the arguments that represent the procedures of the other subexpressions (the operands).

Here's an example:

{% highlight scheme linenos %}
(* 3 7)
21
{% endhighlight %}

We know that the operator in this example (`*`) represents a procedure that multiplies, and that `3` represents that value equal to the number 3 and `7` represents a value equal to the number 7. Therefore, going back to the two steps of evaluating a combination, we know that the given operator in this example represents a procedure will apply multiplication the two values provided and that this will evaluate to the number `21`.

## Recursion

Note that the first step of evaluating a combination means that in order to accomplish the evaluation process for any given combination we must first perform the evaluation process on *each* expression in the combination. Thus, the evaluation rule is **recursive**, meaning it solves the entire problem by reducing it into smaller problems of the same kind. If something is defined recursively, then it is defined in terms of itself. **Recursion** as an evaluation process is a very powerful technique for dealing with hierarchical, tree-like objects.

Here's an example of a recursive compound procedure:

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

We have to evaluate both of them. In order to evaluate the first, we need to evaluate `2` and `(* 4 6)`, which makes us have to recur again. You can think of the **s-expressions** (the expressions in parentheses) as a tree with partial evaluations percolating upwards. In programming, this describes **tree accumulation** which is the process of accumulating data placed in tree nodes according to their tree structure.

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
