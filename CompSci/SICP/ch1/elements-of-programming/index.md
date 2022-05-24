---
title: "1.1 The Elements of Programming"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf

sidebar: compsci_sicp_sidebar
folder: wiki/CompSci/ch1/elements-of-programming
---

## 1.1 The Elements of Programming

Every powerful language has three mechanisms for combining simple ideas to form more complex ideas:

- **Primitive expressions**, which represent the simplest entities the language is concerned with.
  - i.e., *a string, an integer, a boolean,* etc.
- **Means of combination**, by which compound elements are built from simpler ones.
  - i.e., *operators* such as *+, -, /,* etc.
- **Means of abstraction**, by which compound elements can be named and manipulated as units.
  - i.e., *variables, procedures, functions* and *classes.*

In programming, we deal with two kinds of elements: procedures and data. Informally, data is "stuff" that we want to manipulate, and procedures are the step-by-step descriptions of the rules for manipulating the data. Thus, any powerful programming language should be able to describe primitive data and primitive procedures and should have methods for combining and abstracting procedures and data.

In this chapter we deal only with simple numerical data so that we can focus on the rules for building procedures. In later chapters we see that these same rules allow us to build procedures to manipulate compound data as well. In this section we look specifically at procedures.
