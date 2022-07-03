---
title: "1.1.6 Conditional Expressions and Predicates"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8, https://youtu.be/BomXJWPF2cM

search: exclude

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1/
---

I have provided another YouTube link at the bottom of this page to a very helpful video lecture provided by Dr. Andrew Runka where he covers Conditional Expressions and Predicates.

## Case Analysis

The procedures we have defined thus far have no built-in way to test values and then perform different operations depending on the result of a test. For instance, we cannot define a procedure that computes the absolute value of a number by testing whether the number is positive, negative, or zero and then take different actions in those different cases according to the rule:

{% highlight pseudocode %}
|r| =

r  if r > 0
0  if r = 0
-r if r < 0
{% endhighlight %}

{{site.data.alerts.note}}The term <b>psuedocode</b> is used when we refer to a detailed yet readable description of what a computer program or algorithm must do, expressed in a formally-styled natural language rather than in a programming language.<br><br>In the pseudocode written above, we are saying, "r equals... r if r is greater than 0; 0 if r equals 0; or -r if r is less than 0."{{site.data.alerts.end}}

<br>

The construct of testing a value, in order to perform an operation depending on that value, is called **case analysis**.

---

## Predicates

A **predicate** is a kind of operation that evaluates to a **boolean**, which is a data type that is interpreted as either True or False. This is a primitive construct in most programming languages.

### Relational Operators

One of the primitive boolean expressions available are **relational operators** and include `<`, `=`, and `>`.

For example:

{% highlight scheme linenos %}
(= 5 5)
#t

(< 5 1)
#f

(> 25 5)
#t
{% endhighlight %}

### Logical Operators

In addition to the relational operators that can be used for comparison operations, we can use **logical operators** in a boolean expression:

- `(and <e1> <e2> ... <en>)` <br>The interpreter evaluates the expressions `<e>` one at a time, in left-to-right order. If any `<e>` evaluates to false, the value of the `and` expression is false, and the rest of the `<e>`'s are not evaluated. If all `<e>`'s evaluate to true values, the value of the `and` expression is the value of the last one.

- `(or <e1> <e2> ... <en>)` <br>The interpreter evaluates the expressions `<e>` one at a time, in left-to-right order. If any `<e>` evaluates to a true value, that value is returned as the value of the `or` expression, and the rest of  the `<e>`'s are not evaluated. If all `<e>`'s evaluate to false, the value of the `or` expression is false.

- `(not <e>)` <br>The value of a `not` expression is true when the expression `<e>` evaluates to false, and false otherwise.

Notice that `and` and `or` are special forms, not procedures, because not all of the subexpressions are evaluated, but `not` is an ordinary procedure.

As an example of how these are used, the condition that a number `x` be in the range `5 < x < 10` may be  expressed as:

{% highlight scheme linenos %}
(and (> x 5) (< x 10))
{% endhighlight %}

When we define what `x` is and then run this procedure, we can see a result:

{% highlight scheme linenos %}
(define x 1)
(and (> x 5) (< x 10))
#f

(define x 7)
(and (> x 5) (< x 10))
#t
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

{{site.data.alerts.note}}Defining a procedure for the purpose of using it as a predicate is an example of a compound <b>predicate procedure</b>. More details about this kind of predicate below.{{site.data.alerts.end}}

### Predicate Procedures

Another boolean expression available is the **predicate procedure**, which can either be a built-in procedure or a compound procedure.

Some of the built-in predicate procedures include: `number?`, `integer?`, `even?`, `odd?`, `positive?`, `negative?`, etc... Here's an example:

{% highlight scheme linenos %}
(even? 1)
#f

(even? 2)
#t

(odd? 1)
#t

(odd? 2)
#f
{% endhighlight %}

Here's an example of a compound predicate:

{% highlight scheme linenos %}
(define (even? x)
  (- 0 (modulo x 2)))
{% endhighlight %}

## Cond Expressions

Scheme contains a special form to perform case analysis called `cond` (which stands for **conditional**). The general form of a conditional expression is:

{% highlight scheme linenos %}
(cond (<p1> <e1>)
      (<p2> <e2>)
      ...
      (<pn> <en>))
{% endhighlight %}

The aforementioned procedure consists of the symbolic abstraction `cond` followed by parenthesized pairs of expressions (`<p>` and `<e>`) called **clauses**. The first expression in each pair (`<p>`) is the *predicate.* The corresponding expression (`<e>`) is the resulting expression if the returned boolean from the predicate is True. This allows us to perform case analysis.

Furthermore, predicate `<p1>` is evaluated first; if its value is false, then `<p2>` is evaluated; if `<p2>`'s value is also false, then `<p3>` is evaluated. This process continues until a predicate is found whose value is true, in which case the interpreter returns the value of the corresponding expression (`<e>`) of the clause as the value of the entire `cond` expression. If none of the `<p>`'s are found to be true, the value of the `cond` expression is undefined.

Using the absolute value example, we can write a procedure using `cond`:

{% highlight scheme linenos %}
(define (abs x)
    (cond ((> x 0) x)
          ((= x 0) 0)
          ((< x 0) (- x))))
{% endhighlight %}

Correlating the aforementioned structure of using `cond`, you can see that `<p1>` (predicate #1) is equal to `(> x 0)`, and `<e1>` (expression #1) is equal to `x`, which is the same value of `x` that is originally passed into the formal parameter defined in the `abs` procedure.

### else

Another way to write the absolute value procedure is:

{% highlight scheme linenos %}
(define (abs x)
    (cond ((< x 0) (- x))
          (else x)))
{% endhighlight %}

This could be expressed in English as *"If `x` is less than zero return `-x`; otherwise return `x`."* The special symbolic abstraction `else` can be used in place of the `<p>` in the final clause of a `cond`; this causes the `cond` expression to return the value of the corresponding `<e>` whenever all previous clauses have been bypassed. In fact, any expression that always evaluates to true could be used as the `<p>` here.

## If Expressions

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

### if vs. cond

In most programming languages, you will likely only have access to some form of an `if` expression, so you may be asking yourself what the functional difference is between `cond` and `if` in Scheme, and the answer to this is in how they both handle **sequencing**, which is evaluating expression after expression within a single context or branch (e.g., a combination of procedures within the body of a compound procedure – more on this below). Furthermore, clauses in `cond` will allow sequencing whereas `if` will not.

The sole purpose of sequencing in Scheme is to impose side effects (e.g., printing information to the screen) because the value returned from the sequence is the value of the final expression in the sequence.

We can see this in the example below where we define the `signum` piecewise function as a procedure that makes use of sequencing:

{% highlight scheme linenos %}
(define (signum x)
  (cond ((< x 0) (display "negative: ") x)
        ((= x 0) (display "zero: ") x)
        (else (display "positive: ") x)))

(signum 0)
zero: 0

(signum 10)
positive: 10

(signum -100)
negative: -100
{% endhighlight %}

Furthermore, we can see that if we add a step (`+ 1 1`) to the sequence, and that step does not output data to the screen and it isn't the final expression in the sequence, then the value of that step will be discarded:

{% highlight scheme linenos %}
(define (signum x)
  (cond ((< x 0) (display "negative: ") x)
        ((= x 0) (display "zero: ") x)
        (else (display "positive: ") (+ 1 1) x)))

(signum 0)
zero: 0

(signum 10)
positive: 10

(signum -100)
negative: -100
{% endhighlight %}

Now let's take a look at how `if` expressions handles sequencing:

{% highlight scheme linenos %}
(if (> 2 0) ((display "positive: ") 1) ((display "negative: ") -1))

application: not a procedure;
 expected a procedure that can be applied to arguments
  given: #<void>
{% endhighlight %}

The reason this error is thrown is that `if` expressions do not handle sequences the same way `cond` expressions do; rather, when an `if` expression evaluates either the consequent or the alternative, it does so in the *applicative-order* way where it attempts to evaluate the value of the subexpressions and then apply the value of the procedure to its arguments.

{{site.data.alerts.note}}Keep in mind that the <i>applicative-order evaluation</i> that is performed in an if expression only applies to either the consequent or the alternative (whichever the predicate evaluates to), but never both.{{site.data.alerts.end}}

Therefore, once the `display` procedure is evaluated in the sequence, it cannot be applied as a procedure against the integer that follows. However, there is a way to enforce sequencing here (which also works in other places where sequencing is not supported by Scheme), and that is by using the special sequence `begin`, which explicitly tells the interpreter to perform a sequence of operations and then return the value of the last expression in that sequence. In most other cases of sequencing, `begin` is implicitly applied.

Using our previous example, we can see this in practice:

{% highlight scheme linenos %}
(if (> 2 0) (begin (display "positive: ") 1) (begin (display "negative: ") -1))

positive: 1
{% endhighlight %}

## Exercises

Complete Exercises [1.1](/wiki/cs/sicp/ch1/1.1/exercises/#exercise-11) through [1.5](/wiki/cs/sicp/ch1/1.1/exercises/#exercise-15) and revisit the related subjects you struggle with.
