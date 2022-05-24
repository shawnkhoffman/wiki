---
title: "1. Building Abstractions with Procedures"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://en.wikipedia.org/wiki/An_Essay_Concerning_Human_Understanding

sidebar: compsci_sicp_sidebar
folder: wiki/CompSci/ch1/elements-of-programming
---

## Intro – Concepts for Context

This is a difficult book to follow, but is arguably one of the best in the study of Computer Science. It is still used by MIT today.

This book is written such that it's assumed you have at least *some* previous exposure to programming. If you do not, it is highly recommended that you take an introductory course to a programming language such as Python.

Furthermore, this book also uses the Scheme dialect of the Lisp programming language, which is available for most common platforms. Scheme is a pedagogical language and contains only the essentials so that you can learn programming in more advanced languages later. It is highly recommended that you follow the original SICP book that uses Scheme rather than other iterations.

## Simple ideas vs. Complex ideas

The concept of *simple ideas vs. complex ideas* in the human mind is important for context. All ideas form through experiences involving sensation and reflection. **Simple ideas** are ideas that enter the mind through the senses – unrefined, pure, and uncomplicated. Furthermore, the mind has the power of using simple ideas to reflect about other ideas which enter its realm, and of linking and uniting several simple ideas to produce one single idea. Therefore, when the mind compares, unites, and extenuates through the process of reflection, those ideas become **complex ideas**.

## Computational processes, Procedures, and Programs

We often say that, *"a computer program is a set of written instructions that tell the computer what to do."*

**Computational processes** are complex ideas and processes that perform computations abstractly. When you write a program, you define computational processes by a number of written procedures (more on this below).

**Procedures** are the exact steps of a computational process, akin to recipes, and they are designed to perform specific tasks. This should not be confused with algorithms, rather a procedure has all of the characteristics of an algorithm except that it possibly lacks finiteness (it *may* run forever) whereas an algorithm only runs until all steps are completed and a value is returned. Procedures take simple ideas such as primitives, means of combination, and means of abstraction (more on these below) and turn them into complex ideas (computational processes).

A **program** is made up of an evolution of computational processes. The evolution of computational processes may manipulate other processes, data, or both.

Well-designed computational systems are designed in a modular manner, so that the parts can be constructed, replaced, and debugged separately.

Every powerful language has three mechanisms for combining simple ideas to form more complex ideas:

- **Primitive expressions**, which represent the simplest entities the language is concerned with.
  - i.e., *a string, an integer, a boolean,* etc.
- **Means of combination**, by which compound elements are built from simpler ones.
  - i.e., *operators* such as *+, -, /,* etc.
- **Means of abstraction**, by which compound elements can be named and manipulated as units.
  - i.e., *variables, functions* and *classes.*

Any powerful programming language should be able to describe primitive data and primitive procedures and should have methods for combining and abstracting procedures and data.
