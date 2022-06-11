---
title: "1.1.6 Conditional Expressions and Predicates"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8

search: exclude

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1/
---

The procedures we have defined thus far have no built-in way to test values and then perform different operations depending on the result of a test. For instance, we cannot define a procedure that computes the absolute value of a number by testing whether the number is positive, negative, or zero and then take different actions in those different cases according to the rule:

{% highlight pseudocode %}
|r| =

r  if r > 0
0  if r = 0
-r if r < 0
{% endhighlight %}

The construct of testing a value, in order to perform an operation depending on that value, is called **case analysis**, and Scheme contains a special form to perform this kind of evaluation called `cond` (which stands for **conditional**). The general form of a conditional expression is:

{% highlight scheme linenos %}
(cond (<p1> <e1>)
      (<p2> <e2>)
      ...
      (<pn> <en>))
{% endhighlight %}

The above procedure consists of the symbol `cond` followed by parenthesized pairs of expressions (`<p>` and `<e>`) called **clauses**. The first expression in each pair (`<p>`) is a **predicate** – that is, an expression whose value is interpreted as either true or false. The corresponding expression (`<e>`) is the resulting expression if the result of the predicate that precedes it is true. This allows us to perform case analysis.

Furthermore, predicate `<p1>` is evaluated first; if its value is false, then `<p2>` is evaluated; if `<p2>`'s value is also false, then `<p3>` is evaluated. This process continues until a predicate is found whose value is true, in which case the interpreter returns the value of the corresponding expression (`<e>`) of the clause as the value of the entire `cond` expression. If none of the `<p>`'s are found to be true, the value of the `cond` expression is undefined.

Using the absolute value example, we can write a procedure using `cond`:

{% highlight scheme linenos %}
(define (abs x)
    (cond ((> x 0) x)
          ((= x 0) 0)
          ((< x 0) (- x))))
{% endhighlight %}

Correlating the aforementioned structure of using `cond`, you can see that `<p1>` (predicate #1) is equal to `(> x 0)`, and `<e1>` (expression #1) is equal to `x`, which is the same value of `x` that is originally passed into the formal parameter defined in the `abs` procedure.

Another way to write the absolute value procedure is:

{% highlight scheme linenos %}
(define (abs x)
    (cond ((< x 0) (- x))
          (else x)))
{% endhighlight %}

This could be expressed in English as *"If `x` is less than zero return `-x`; otherwise return `x`."* The special symbol `else` can be used in place of the `<p>` in the final clause of a `cond`; this causes the `cond` expression to return the value of the corresponding `<e>` whenever all previous clauses have been bypassed. In fact, any expression that always evaluates to true could be used as the `<p>` here.

Here is yet another way to write the absolute value procedure:

{% highlight scheme linenos %}
(define (abs x)
    (if (< x 0)
        (- x)
        x))
{% endhighlight %}

This uses the special form `if`, which is a restricted type of conditional that can be used when there are precisely two cases in the case analysis. The general form of an `if` expression is:

{% highlight scheme linenos %}
(if <predicate> <consequent> <alternative>)
{% endhighlight %}

To evaluate an `if` expression, the interpreter starts by evaluating the `<predicate>` part of the expression. If the `<predicate>` evaluates to true, the interpreter then evaluates the `<consequent>` and returns its value. Otherwise, it evaluates the `<alternative>` and returns its value.

In addition to primitive predicates that use comparison operators, such as `<`, `=`, and `>`, there are logical composition operations that enable us to construct compound predicates; the three most frequently used are `and`, `or`, and `not`:

- `(and <e1> <e2> ... <en>)` <br>The interpreter evaluates the expressions `<e>` one at a time, in left-to-right order. If any `<e>` evaluates to false, the value of the `and` expression is false, and the rest of the `<e>`'s are not evaluated. If all `<e>`'s evaluate to true values, the value of the `and` expression is the value of the last one.

- `(or <e1> <e2>... <en>)` <br>The interpreter evaluates the expressions `<e>` one at a time, in left-to-right order. If any `<e>` evaluates to a true value, that value is returned as the value of the `or` expression, and the rest of  the `<e>`'s are not evaluated. If all `<e>`'s evaluate to false, the value of the `or` expression is false.

- `(not <e>)` <br>The value of a `not` expression is true when the expression `<e>` evaluates to false, and false otherwise.

Notice that `and` and `or` are special forms, not procedures, because not all of the subexpressions are evaluated, but `not` is an ordinary procedure.

As an example of how these are used, the condition that a number `x` be in the range `5 < x < 10` may be  expressed as:

{% highlight scheme linenos %}
(and (> x 5) (< x 10))
{% endhighlight %}

As another example, we can define a predicate to test whether one number is greater than or equal to another as:

{% highlight scheme linenos %}
(define (>= x y)
    (or (> x y) (= x y)))
{% endhighlight %}

or alternatively as:

{% highlight scheme linenos %}
(define (>= x y)
    (not (< x y)))
{% endhighlight %}
