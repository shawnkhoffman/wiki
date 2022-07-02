---
title: "Exercises"
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

Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.

{% highlight scheme linenos %}
10
(+ 5 3 4)
(- 9 1)
(/ 6 2)
(+ (* 2 4) (- 4 6))
(define a 3)
(define b (+ a 1))
(+ a b (* a b))
(= a b)
(if (and (> b a) (< b (* a b)))
    b
    a)
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
(+ 2 (if (> b a) b a))
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
{% endhighlight %}

<br>

**Subjects**:

- [Expressions and Combinations](/wiki/cs/sicp/ch1/1.1/expressions_and_combinations/)
- [Evaluating Combinations](/wiki/cs/sicp/ch1/1.1/evaluating_combinations/)
- [Compound Procedures](/wiki/cs/sicp/ch1/1.1/compound_procedures/)
- [Conditional Expressions and Predicates](/wiki/cs/sicp/ch1/1.1/conditional_expressions_and_predicates/)

---

## Exercise 1.2

Translate the following expression into prefix form:

<math xmlns="https://www.w3.org/TR/MathML3/" display="block">
  <mrow class="MJX-TeXAtom-ORD">
    <mfrac>
      <mrow>
        <mn>5</mn>
        <mo>+</mo>
        <mn>4</mn>
        <mo>+</mo>
        <mo stretchy="false">(</mo>
        <mn>2</mn>
        <mo>&#x2212;<!-- − --></mo>
        <mo stretchy="false">(</mo>
        <mn>3</mn>
        <mo>&#x2212;<!-- − --></mo>
        <mo stretchy="false">(</mo>
        <mn>6</mn>
        <mo>+</mo>
        <mfrac>
          <mn>4</mn>
          <mn>5</mn>
        </mfrac>
        <mo stretchy="false">)</mo>
        <mo stretchy="false">)</mo>
        <mo stretchy="false">)</mo>
      </mrow>
      <mrow>
        <mn>3</mn>
        <mo stretchy="false">(</mo>
        <mn>6</mn>
        <mo>&#x2212;<!-- − --></mo>
        <mn>2</mn>
        <mo stretchy="false">)</mo>
        <mo stretchy="false">(</mo>
        <mn>2</mn>
        <mo>&#x2212;<!-- − --></mo>
        <mn>7</mn>
        <mo stretchy="false">)</mo>
      </mrow>
    </mfrac>
    <mo>.</mo>
  </mrow>
</math>

<br>

**Subjects**:

- [Expressions and Combinations](/wiki/cs/sicp/ch1/1.1/expressions_and_combinations/)

---

## Exercise 1.3

Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.

<br>

**Subjects**:

- [Expressions and Combinations](/wiki/cs/sicp/ch1/1.1/expressions_and_combinations/)
- [Evaluating Combinations](/wiki/cs/sicp/ch1/1.1/evaluating_combinations/)
- [Compound Procedures](/wiki/cs/sicp/ch1/1.1/compound_procedures/)
- [Conditional Expressions and Predicates](/wiki/cs/sicp/ch1/1.1/conditional_expressions_and_predicates/)

---

## Exercise 1.4

Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:

{% highlight scheme linenos %}
(define (a-plus-abs-b a b)
    ((if (> b 0) + -) a b))
{% endhighlight %}

<br>

**Subjects**:

- [Compound Procedures](/wiki/cs/sicp/ch1/1.1/compound_procedures/)
- [Conditional Expressions and Predicates](/wiki/cs/sicp/ch1/1.1/conditional_expressions_and_predicates/)

---

## Exercise 1.5

Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is  using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:

{% highlight scheme linenos %}
(define (p) (p))

(define (test x y)
(if (= x 0)
    0
    y))
{% endhighlight %}

Then he evaluates the expression:

{% highlight scheme linenos %}
(test 0 (p))
{% endhighlight %}

What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer. (Assume that the evaluation rule for the special form `if` is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate  the consequent or the alternative expression.)

<br>

**Subjects**:

- [Applicative order vs. Normal order evaluation](/wiki/cs/sicp/ch1/1.1/the_substitution_model/#applicative-order-vs-normal-order)
- [Compound Procedures](/wiki/cs/sicp/ch1/1.1/compound_procedures/)
- [Conditional Expressions and Predicates](/wiki/cs/sicp/ch1/1.1/conditional_expressions_and_predicates/)

## Exercise 1.6

Alyssa P. Hacker doesn’t see why `if` needs to be provided as a special form. *"Why can’t I just define it as an ordinary procedure in terms of cond?"* she asks. Alyssa’s friend Eva Lu Ator claims this can indeed be done, and she defines a new version of `if`:

{% highlight scheme linenos %}
(define (new-if predicate then-clause else-clause)
    (cond (predicate then-clause)
        (else else-clause)))
{% endhighlight %}

Eva demonstrates the program for Alyssa:

{% highlight scheme linenos %}
(new-if (= 2 3) 0 5)
5

(new-if (= 1 1) 0 5)
0
{% endhighlight %}

Delighted, Alyssa uses new-if to rewrite the square-root program:

{% highlight scheme linenos %}
(define (sqrt-iter guess x)
    (new-if (good-enough? guess x)
            guess
            (sqrt-iter (improve guess x) x)))
{% endhighlight %}

What happens when Alyssa attempts to use this to compute square roots? Explain.

<br>

**Subjects**:

- [The Substitution Model](/wiki/cs/sicp/ch1/1.1/the_substitution_model/)
- [Predicates](/wiki/cs/sicp/ch1/1.1/conditional_expressions_and_predicates/#predicates)
- [Square Roots by Newton's Method](/wiki/cs/sicp/ch1/1.1/example_newtons_method/)

## Exercise 1.7

The `good-enough?` test used in computing square roots will not be very effective for finding the square roots of very small numbers. Also, in real computers, arithmetic operations are almost always performed with limited precision. This makes our test inadequate for very large numbers. Explain these statements, with examples showing how the test fails for small and large numbers. An alternative strategy for implementing `good-enough?` is to watch how guess changes from one iteration to the next and to stop when the change is a very small fraction of the guess. Design a square-root procedure that uses this kind of end test. Does this work better for small and large numbers?

## Exercise 1.8

*Newton’s method* for cube roots is based on the fact that if *y* is an approximation to the cube root of *x*, then a better approximation is given by the value:

<math xmlns="https://www.w3.org/TR/MathML3/" display="block">
    <mfrac>
        <mrow>
            <mi>x</mi>
            <mo>/</mo>
            <msup>
                <mi>y</mi>
                <mn>2</mn>
            </msup>
            <mo>&#xA0;</mo>
            <mo>+</mo>
            <mo>&#x2009;</mo>
            <mn>2</mn>
            <mi>y</mi>
        </mrow>
        <mn>3</mn>
    </mfrac>
    <mo>.</mo>
</math>

Use this formula to implement a cube-root procedure analogous to the square-root procedure.
