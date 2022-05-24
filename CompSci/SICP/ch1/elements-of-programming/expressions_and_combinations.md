---
title: "1.1.1 Expressions and Combinations"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_sidebar
folder: wiki/CompSci/ch1/elements-of-programming
---

## The REPL

Even with complex expressions, the Scheme interpreter always operates in the same basic cycle: It reads an expression from the terminal, evaluates the expression, and prints the result. This mode of operation is often expressed by saying that the interpreter runs in a **read-eval-print loop (REPL)**.

An **expression** is a written programmatic construct that returns a value. Some of these are primitive expressions.

## Primitive Expressions

The examples of primitive expressions entered into the REPL below are self-evaluating (they evaluate to themselves).

### Integers

If you enter an integer into the Scheme interpreter:

{% highlight scheme linenos %}
495
{% endhighlight %}

It will return the value of that integer to you:

{% highlight output linenos %}
495
{% endhighlight %}

### Strings

If you enter a string into the Scheme interpreter:

{% highlight scheme linenos %}
"Hello, World!"
{% endhighlight %}

It will return the value of that string to you:

{% highlight output linenos %}
"Hello, World!"
{% endhighlight %}

### Booleans

{% highlight scheme linenos %}
true
#t

false
#f
{% endhighlight %}

### Unbound variables

If you try to lookup an unrecognized expression:.

{% highlight scheme linenos %}
undefined
{% endhighlight %}

You get an error:

{% highlight output linenos %}
Unbound variable: undefined
{% endhighlight %}

## Compound elements

A **combination** is an expression that is formed by forming a list of expressions and includes both operators and operands. The element that represents a symbol representing a mathematical or logical manipulation is called the **operator**, and the other elements, which the operator is *operating* on, are called **operands**. The value of a combination is obtained by applying the procedure specified for the operator to the operands.

The convention of placing the operator to the left of the operands is known as **prefix notation**, and it may be somewhat confusing at first because it departs significantly from the customary mathematical convention. 

{% highlight scheme linenos %}
(+ 137 349)
486

(- 1000 344)
666

(* 5 99)
495

(/ 10 5)
2

(+ 2.7 10)
12.7
{% endhighlight %}

Prefix notation has several advantages, however. One of them is that it can accommodate functions that may take an arbitrary number of arguments, as in the following examples:

{% highlight scheme linenos %}
(* 2 3 4)
24
{% endhighlight %}

### Nesting combinations

Prefix notation also extends in a straightforward way to allow combinations to be nested – that is, to have combinations whose elements are themselves combinations:

{% highlight scheme linenos %}
(+ (* 3 5) (- 10 6))
19

(+ (* 3 (+ (* 2 4) (+ 3 5))) (+ (- 10 7) 6))
57
{% endhighlight %}

**pretty-printing** is where you write each long combination so that the operands are aligned vertically. It makes expressions more human-readable, but are not needed by the machine. The resulting indentations clearly display the structure of the expression. From the last Scheme example:

{% highlight scheme linenos %}
(+ (* 3
      (+ (* 2 4)
         (+ 3 5)))
   (+ (- 10 7)
      6))

57
{% endhighlight %}
