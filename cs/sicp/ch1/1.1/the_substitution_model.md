---
title: "1.1.5 The Substitution Model"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8, https://youtu.be/z3BGkwjCeZw, https://youtu.be/FV02zkWmpUw

search: exclude

folder: wiki/cs/ch1/1.1/
sidebar: compsci_sicp_ch1_sidebar
---

As a part of this section, I have included another video lecture by Dr. Andrew Runka, which goes over the Substitution Model and Special Forms, and I highly encourage you to watch it.

The mechanism for applying primitive procedures to arguments is built into the interpreter. Any procedure beyond that is not primitive because you define it.

However, to evaluate a combination whose operator names a compound procedure, the interpreter follows mostly the same process as for combinations whose operators name primitive procedures, which was described in section [1.1.3](/wiki/cs/sicp/ch1/1.1/evaluating_combinations/). That is, the interpreter recursively evaluates every part of the expression: it evaluates the procedure (which is the value of the given operator in the combination), it then evaluates each operand, and it then evaluates the application of the procedure to the arguments (which are the values of the given operands in the combination).

The **substitution model** is the method Scheme uses to apply compound procedures to their arguments. When a compound procedure is invoked, the Scheme interpreter goes through the body of that procedure and replaces every copy of a formal parameter with the corresponding actual argument value(s) provided when the procedure is invoked. Then Scheme evaluates the resulting expression.

Let's use the last example compound procedure from section [1.1.4](/wiki/cs/sicp/ch1/1.1/compound_procedures/):

{% highlight scheme linenos %}
(f 5)
{% endhighlight %}

We begin by examining the body of `f`:

{% highlight scheme linenos %}
(sum-of-squares (+ a 1) (* a 2))
{% endhighlight %}

Then we replace the formal parameter a by the argument 5:

{% highlight scheme linenos %}
(sum-of-squares (+ 5 1) (* 5 2))
{% endhighlight %}

which reduces by multiplication to:

{% highlight scheme linenos %}
(+ 36 100)
{% endhighlight %}

and finally to:

{% highlight scheme linenos %}
136
{% endhighlight %}

However, there are two points that should be stressed:

1. The purpose of the substitution model is to help us think about how procedures are applied, not to provide a description of how the interpreter really works.

2. There are many more increasingly elaborate models of how interpreters work. The substitution model is only the first of these models – a way to get started thinking formally about the evaluation process. In general, when modeling phenomena in science and engineering, we begin with simplified, incomplete models. As we examine things in greater detail, these simple models become inadequate and must be replaced by more refined models, and the substitution model is no exception.

## Applicative order vs. Normal order

According to the description of evaluation so far, the interpreter first evaluates the operator and operands and then applies the resulting procedure to the resulting arguments, just as was described in Section [1.1.3](/wiki/cs/sicp/ch1/1.1/evaluating_combinations/). This method of evaluation is called **applicative-order evaluation**.

However, this is not the only way to perform evaluation. There is an alternative evaluation model that does not evaluate the operands (parameters) until their values are needed. Instead, it first substitutes operand expressions for parameters until it obtains an expression involving only primitive operators, and then it performs the evaluation. This is called **normal-order evaluation**. Here's an example of this method:

The evaluation of `(f 5)` proceeds according to the following sequence of expansions:

{% highlight scheme linenos %}
(sum-of-squares (+ 5 1) (* 5 2))

(+    (square (+ 5 1))      (square (* 5 2))   )

(+    (* (+ 5 1) (+ 5 1))   (* (* 5 2) (* 5 2)))
{% endhighlight %}

followed by the following reductions:

{% highlight scheme linenos %}
(+    (* 6 6)       (* 10 10))
            (+ 36 100)
                136
{% endhighlight %}

This gives the same answer as our previous evaluation model, but the process is different. In particular, the evaluations of `(+ 5 1)` and `(* 5 2)` are each performed twice here, corresponding to the reduction of the expression `(* x x)` with `x` replaced respectively by `(+ 5 1)` and `(* 5 2)`.
