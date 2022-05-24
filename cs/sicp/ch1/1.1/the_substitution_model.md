---
title: "1.1.5 The Substitution Model"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

search: exclude

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1/
---

To evaluate a combination whose operator names a compound procedure, the interpreter follows much the  same process as for combinations whose operators name primitive procedures, which was described in section [1.1.3](/wiki/cs/sicp/ch1/1.1/evaluating_combinations/). That is, the interpreter evaluates the elements of the combination and applies the procedure (which is the value of the operator of the combination) to the arguments (which are the values of the  operands of the combination).

The mechanism for applying primitive procedures to arguments is built into the interpreter.

To apply a compound procedure to arguments, evaluate the body of the procedure with each formal parameter replaced by the corresponding argument.

Let's use the example compound procedure from section [1.1.4](/wiki/cs/sicp/ch1/1.1/compound_procedures/):

{% highlight scheme linenos %}
(define (square x) (* x x))
{% endhighlight %}

We know that invoking the procedure and providing an argument for the parameter `x` will result in that argument being multiplied by itself and the value of that expression will be returned, like so:

{% highlight scheme linenos %}
(square 5)
25
{% endhighlight %}

However, we can also assign this procedure to a different name:

{% highlight scheme linenos %}
(define f square)
{% endhighlight %}

Using only the name `f`, we can now invoke the same procedure:

{% highlight scheme linenos %}
(f 5)
25
{% endhighlight %}

We can also extend `square` using the name `f`:

{% highlight scheme linenos %}
(define f (square (+ a 1) (* a 2)))
{% endhighlight %}
