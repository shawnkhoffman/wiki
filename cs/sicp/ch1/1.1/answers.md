---
title: "Answers"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

## Exercise 1.1

{% highlight scheme linenos %}
10
12
8
3
6
19
false
4
16
6
16
{% endhighlight %}

## Exercise 1.2

{% highlight scheme linenos %}
(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) (* 3 (- 6 2) (- 2 7)))
{% endhighlight %}

## Exercise 1.3

{% highlight scheme linenos %}
; Procedure to evaluate the square of a number
(define (square x) (* x x))

; Procedure to evaluate "greater than or equal to"
(define (>= x y)
    (or (> x y) (= x y)))

; Procedure to evaluate the sum of two squares
(define (sum-of-squares x y)
  (+ (square x) (square y)))

; Solution using both of the above procedures
(define (solution a b c)
  (cond ((or (and (>= a b) (< c b)) (and (>= b a) (< c a)))
         (sum-of-squares a b))
        ((or (and (>= a c) (< b c)) (and (>= c a) (< b a)))
         (sum-of-squares a c))
        ((or (and (>= b c) (< a c)) (and (>= c b) (< a b)))
         (sum-of-squares b c))))

; Solution Tests
(solution 5 5 1)
50

(solution 5 1 5)
50

(solution 1 5 5)
50

(solution 10 10 1)
200

(solution 10 1 10)
200

(solution 1 10 10)
200
{% endhighlight %}

## Exercise 1.4

The compound procedure `a-plus-abs-b` is declared and takes two arguments: `a` and `b`.

{% highlight scheme linenos %}
(define (a-plus-abs-b a b)
...
{% endhighlight %}

In the body of this compound procedure, a procedure is declared and will be either an addition or subtraction compound expression contingent upon the result of the conditional special form `if`: if `b` is greater than `0`, the applied procedure will be `+` (addition); otherwise, the applied procedure will be `-` (subtraction). Then, that applied procedure will be applied to `a` and `b`; either `a + b` or `a - b`.

{% highlight scheme linenos %}
...
    ((if (> b 0) + -) a b))
{% endhighlight %}

## Exercise 1.5

In **applicative-order evaluation**, when the procedure "test" is invoked, Ben will observe that the interpreter will immediately substitute `x` for `0`; then, when the `if` conditional in the procedure body validates that `x` is equal to `0`, the procedure will returns the value `0`.

In **normal-order evaluation**, when the procedure "test" is invoked, Ben will observe that the interpreter will not immediately perform the substitution for the arguments; rather, the interpreter will wait to perform the substitution until the primitive procedure `(= x 0)` is called in the `if` conditional inside the body of the parent procedure. Once the substitution is performed, the primitive procedure is evaluated and the parent procedure returns the value `0`.
