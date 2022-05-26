---
title: "1.1.4 Compound Procedures"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

A **compound procedure** is a procedure that you define. This is the opposite of a primitive procedure, which we have already seen in this chapter.

## Defining procedures

You define a procedure using a **procedure definition**, which is a powerful abstraction technique where a compound operation can be given a name and then referred to as a unit. We call this a **procedural abstraction**. For example:

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
441
{% endhighlight %}

## Extending Compound Procedures

Back to Scheme, we can also use `square` as a building block in defining other procedures. For example, `x² + y²` can be expressed as:

{% highlight scheme linenos %}
(+ (square x) (square y))
{% endhighlight %}

Therefore, we can easily define a procedure that, where any two numbers are provided as arguments, produces the sum of their squares: 

{% highlight scheme linenos %}
(define (sum-of-squares x y)
  (+ (square x) (square y)))

(sum-of-squares 3 4)
25
{% endhighlight %}

Now we can take this a step further and use `sum-of-squares` as a building block in constructing more compound procedures:

{% highlight scheme linenos %}
(define (f a)
  (sum-of-squares (+ a 1) (* a 2)))

(f 5)
136
{% endhighlight %}

This is useful when we start getting into incremental development.
