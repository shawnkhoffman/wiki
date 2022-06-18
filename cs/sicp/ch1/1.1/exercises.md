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

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
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
