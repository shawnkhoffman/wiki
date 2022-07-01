---
title: "1.1.1 Expressions and Combinations"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8, https://youtu.be/NMf9yjuC944

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

As previously mentioned, this book uses the Scheme programming language throughout for pedagogical reasons, and it is highly recommended that you use this language to follow along. I have added a YouTube link to an introductory lesson on Scheme by Dr. Andrew Runka from the School of Computer Science at Carleton University in Canada, which you can find at the bottom of this page.

## The REPL

An **interpreter** is a special kind of program that is designed to evaluate the commands in a program. You will typically only find these interpeters in *interpreted languages,* such as Scheme. Even with complex expressions, the Scheme interpreter always operates in the same basic cycle: It reads an expression from the terminal, evaluates the expression, and then prints the result. This mode of operation is often expressed by saying that the interpreter runs in a **read-eval-print loop (REPL)**. Furthermore, you can interact with the interpreter's REPL in most interpreted programming languages.

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

If you try to lookup an unrecognized expression:

{% highlight scheme linenos %}
undefined
{% endhighlight %}

You get an error:

{% highlight output linenos %}
Unbound variable: undefined
{% endhighlight %}

## Compound elements

A **combination** is an expression that is formed by forming a list of expressions and includes both operators and operands. Here is the structure of a basic combination:

{% highlight scheme linenos %}
(operator operand₁ operand₂ ...)
{% endhighlight %}

The symbolic abstraction representing a mathematical or logical manipulation is called the **operator**. The other elements, which the operator is *operating* on, are called **operands**. Moreover, the value of a combination is obtained by applying the operator to the operands.

It is important to note that the operator itself represents a procedure. In fact, you can simply pass in any built-in operator and it will tell you that it's a procedure:

{% highlight scheme linenos %}
+
<procedure:+>

-
<procedure:->

*
<procedure:->

/
<procedure:/>
{% endhighlight %}

Therefore, the value of any combination is the result of applying the given procedure to the arguments. So, if we say `(+ 2 2)`, then the operator `+` would be the procedure, both `2`'s would be the arguments, and so the value would be `4`.

We can see more examples of basic procedures:

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

The convention of placing the operator to the left of the operands is known as **prefix notation**, and it may be somewhat confusing at first because it departs significantly from the customary mathematical convention. Prefix notation has several advantages, however. One of them is that it can accommodate functions that may take an arbitrary number of arguments, as in the following examples:

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
(+ (* 3 (+
        (* 2 4)
        (+ 3 5)))
   (+ (- 10 7)
            6))

57
{% endhighlight %}

## Type Checking

This isn't in the book, but it is important to understand for context. **Type checking** is the language's process of ensuring that the correct types of values are being provided as operands for specific operations.

For example, the following procedure would return an error:

{% highlight scheme linenos %}
(+ 6 "six")

+: contract violation
  expected: number?
  given: "twelve"
{% endhighlight %}

Type checking keeps us from trying to perform strange operations such as dividing a string by a number or a boolean.

There are two forms of type checking. **Static type checking** is done at compile time where all variables are known as *static types* because they do not change (when something is static, it is immutable). Because type checking is done at compile time, the type for each variable must be declared before compile time. For example, in Golang *– a statically typed language –* the formal method of declaring a variable would look something like this:

{% highlight go linenos %}
var i int = 1
{% endhighlight %}

Static type checking is beneficial in that it keeps your program from crashing from simple type errors, because the compiler will catch the error before it allows the code to be compiled.

In **dynamic type checking** the type of the variable is not constrained and the type checking is performed at runtime. Most interpreted languages are dynamically typed language because it has the benefit of having the interpreter available to validate types at runtime, but the tradeoff is interpreted languages are significantly less performant than compiled languages. Furthermore, in a *dynamically typed language* such as Python the type of a variable can change at any time, and therefore there is less regulation on the structure of variable declaration. For example:

{% highlight python linenos %}
# Initial variable declaration
i = 1

# Variable re-declaration on a different line
i = "one"
{% endhighlight %}