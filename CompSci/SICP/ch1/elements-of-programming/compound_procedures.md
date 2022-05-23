---
title: "1.1.4 Compound Procedures"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/CompSci/ch1/elements-of-programming
---

Procedures should not be confused with functions. A procedure performs a task, whereas a function produces information. Functions return values and procedures do not.

### Defining procedures

A **procedure definition** is a powerful abstraction technique where a compound operation can be given a name and then referred to as a unit. For example:

{% highlight scheme linenos %}
(define (square x) (* x x))
{% endhighlight %}

This is an example of a **compound procedure**. The procedure in this example represents the operation of multiplying something by itself. The thing to be multiplied is given a local name, `x`. Evaluating the definition creates this compound procedure and associates it with the name `square`.

The general form of a procedure definition in Scheme is:

{% highlight scheme linenos %}
(define (<name> <formal parameters>) <body>)
{% endhighlight %}

The `<name>` is a symbol to be associated with the procedure definition in the environment, allowing you to invoke the procedure by name. The `<formal  parameters>` are the names used within the body of the procedure to refer to the corresponding *arguments* of the procedure, which allows you to take any arbitrary numeric value and pass it into the procedure for evaluation. The `<body>` is an expression that will yield the value from the procedure application. The `<name>` and the `<formal parameters>` are grouped within a set of parentheses, just as they would be in an actual call to the procedure being defined.

Once we define a procedure, we can of course use it in expressions:

{% highlight scheme linenos %}
(square 21)
441

(square (+ 2 5))
49

(square (square 3))
81
{% endhighlight %}

If we wrote this as a function in Python, it would look something like this:

{% highlight python linenos %}
def square(x):
    v = x * x
    return v
{% endhighlight %}

The general form of a function definition in Python is:

{% highlight python linenos %}
def <name>(<formal parameters>):
    <body>
{% endhighlight %}

Now we can invoke the function by name:

{% highlight python linenos %}
square(21)
{% endhighlight %}
