---
title: "1.1.7 Example: Square Roots by Newton's Method"
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

## Intro

The subject of mathematical functions correlates to programming. If you are not familiar with mathematical functions, it's a fairly straightforward concept, and I highly recommend you to study this concept until you understand it before proceeding forward into this section. You can use <a target="_blank" href="https://www.khanacademy.org/math/algebra/x2f8bb11595b61c86:functions/x2f8bb11595b61c86:evaluating-functions/v/what-is-a-function">this Khan Academy video</a> to do so.

{{site.data.alerts.tip}}<a target="_blank" href="https://www.khanacademy.org/">Khan's Academy</a> is my recommended solution for learning almost all things related to Math. They're a nonprofit organization and their platform is free, but they do rely on donations to keep their website going.{{site.data.alerts.end}}

## How procedures in programming relate to mathematical functions

Procedures are quite akin to ordinary mathematical functions; they specify a value that is determined by one or more parameters. But there is an important distinction between mathematical functions and computational procedures; procedures must be effective and serve some sort of purpose in your program. Also, math functions do not describe a procedure, and so they tell us almost nothing about how to actually find the solution of a given expression. Furthermore, it doesn't help to "pseudo rephrase" a math function in any programming language – that is, use a programming language to try to rephrase the math function.

Moreover, the difference between a mathematical function and a procedure is a reflection of the difference between describing properties of things and describing how to do things, or, as it is sometimes referred to, the distinction between *declarative knowledge* and *imperative knowledge.* In mathematics we are usually concerned with declarative (what is) descriptions, whereas in computer science we are usually concerned with imperative (how to) descriptions.

Therefore, what you must do to translate a math function into a programming language is to use the language to procedurally perform the evaluation (imperatively) step-by-step.

{{site.data.alerts.note}}The example the book mentions in this section is <b>Newton's Method of Successive Approximations</b>.<br>
Furthermore, if you are unfamiliar with <a target="_blank" href="https://youtu.be/WuaI5G04Rcw">Newton's Method</a>, this subject is taught in Calculus, but do not be alarmed; you do not have to know this, but you should try your best to follow along just to understand the main point that is being illustrated here, which is taking small and simple compound procedures and recursion (both of which are explained in previous sections) to construct a larger and more complex procedure.<br><br>
Moreover, even if you have zero experience with Calculus or you fear that your math skills are not prepared for this, you <i>can</i> learn this, and I have provided a link to a YouTube video on Newton's Method at the bottom of this page. However, I do encourage you to be familiar with mathematical functions and derivatives before you take a swing at becoming familiar with Newton's Method, and you can use <a target="_blank" href="https://www.khanacademy.org/math/differential-calculus">this Khan's Academy course</a> to do so.{{site.data.alerts.end}}

What the book does to illustrate Newton's Method in Scheme is it creates the following procedures;

The larger complex compound procedure made up of several simple compound procedures:

{% highlight scheme linenos %}
(define (sqrt-iter guess x) (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))
{% endhighlight %}

The simple compound procedures used by the larger complex compound procedure:

{% highlight scheme linenos %}
; Don't forget these two procedures previously mentioned; these will be used in this program.

(define (square x) (* x x))

(define (abs x)
    (cond ((> x 0) x)
          ((= x 0) 0)
          ((< x 0) (- x))))
{% endhighlight %}

{% highlight scheme linenos %}
; A procedure to improve the guess.
; This procedure also uses a simpler compound procedure that is defined below.

(define (improve guess x)
    (average guess (/ x guess)))
{% endhighlight %}

{% highlight scheme linenos %}
; A procedure to calculate the average of y from x/y.
; This evaluation is used to get a better guess.

(define (average x y)
    (/ (+ x y) 2))
{% endhighlight %}

{% highlight scheme linenos %}
; A procedure to determine whether the guess is good enough to be an acceptable
; match. This procedure gets used as a base case in the recursion of the larger
; complex procedure.

(define (good-enough? guess x)
    (< (abs (- (square guess) x)) 0.001))
{% endhighlight %}

{{site.data.alerts.tip}}It is a good idea to give predicates names that end with question marks to help us remember that they are predicates. This is just a stylistic convention; as far as the interpreter is concerned, the question mark is just an ordinary character.{{site.data.alerts.end}}<br>

{% highlight scheme linenos %}
; A procedure used as the entrypoint into the program. This entrypoint is the
; first procedure called to start the program.
; This procedure calls upon the aforementioned larger complex procedure.

(define (sqrt x)
    (sqrt-iter 1.0 x))
{% endhighlight %}

You could actually copy each of the procedures above in exact order and begin making use of this program:

{% highlight scheme linenos %}
(sqrt 100)
10.000000000139897

(sqrt 9)
3.00009155413138

(sqrt (+ 100 37))
11.704699917758145

(sqrt (+ (sqrt 2) (sqrt 3)))
1.7739279023207892

(square (sqrt 1000))
1000.000369924366
{% endhighlight %}
