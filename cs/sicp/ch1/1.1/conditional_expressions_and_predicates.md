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

The procedures we have defined thus far have no built-in way to test values and perform different operations depending on the result of said test.

For instance, we cannot define a procedure that computes the absolute value of a number by testing whether the number is positive, negative, or zero and taking different actions in the different cases according to the rule:

{% highlight pseudocode %}
|r| =

r if r > 0
0 if r = 0
-r if r < 0
{% endhighlight %}

The construct above is called *case analysis*, and Scheme contains a special form to perform this kind of evaluation called `cond` (which stands for *conditional*), and it is used as follows:

{% highlight scheme linenos %}
(define (abs x)
    (cond ((> x 0) x)
          ((= x 0) 0)
          ((< x 0) (- x))))
{% endhighlight %}

The general form of a conditional expression is:

{% highlight scheme linenos %}
(cond (<p1> <e1>)
      (<p2> <e2>)
      .
      .
      .
      (<pn> <en>))
{% endhighlight %}

The above structure consisting of the symbol `cond` followed by parenthesized pairs of expressions (`<p>` and `<e>`) called *clauses.* The first expression in each pair is a *predicate* – that is, an expression whose value is interpreted as either true or false.
