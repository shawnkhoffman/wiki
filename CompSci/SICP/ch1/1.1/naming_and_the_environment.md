---
title: "1.1.2 Naming and the Environment"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/CompSci/ch1/1.1
---

A critical aspect of a programming language is the means it provides for using names to refer to computational objects. We say that the name identifies a **variable** whose **value** is the object. For example:

{% highlight scheme linenos %}
(define size 2)
{% endhighlight %}

The above code causes the interpreter to associate the value `2` with the name `size`. Now we can refer to the value `2` in expressions by the name:

{% highlight scheme linenos %}
size
2

(* 5 size)
10
{% endhighlight %}

Variables are a language's simplest means of abstraction as they allow us to use simple names to refer to the results of compound operations.

In general, computational objects may have very complex structures, and so it would be extremely inconvenient to have to remember and repeat their details each time we want to use them. Furthermore, complex programs are constructed by building, step-by-step, computational objects of increasing complexity, and the REPL makes this step-by-step program construction particularly convenient because name-object associations can be created incrementally in successive interactions. This feature encourages the incremental development and testing of programs and is largely responsible for the fact that a Lisp program usually consists of a large number of relatively simple functions.

In an interpreted language, the possibility of associating values with symbols (i.e., variables) and later retrieving them means that the interpreter must maintain some sort of memory that keeps track of the name-object pairs. This memory is called the **environment** (more precisely the **global environment**). A computation may involve a number of different environments.
