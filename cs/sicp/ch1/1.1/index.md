---
title: "1.1 The Elements of Programming"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8, https://youtube.com/playlist?list=PLu-l3j4eka05K_tXNEVaOGy1Vb-oO0Y2s

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

Every powerful language has three mechanisms for combining simple ideas to form more complex ideas:

- **Primitive expressions**, which represent the simplest entities the language is concerned with.
  - i.e., *a string (letters or words), an integer, a boolean (true or false),* etc.
- **Means of combination**, which allow you to create compound expressions from simpler expressions.
  - i.e., using numbers to perform mathematical calculations.
- **Means of abstraction**, by which compound elements can be named and manipulated as units.
  - i.e., the use of symbols such as the aforementioned operators, and phonetic names to refer to values as well as processes.

In Scheme programming, we deal with two kinds of elements: procedures and data. Informally, data is "stuff" that we want to manipulate, and procedures are the step-by-step descriptions of the rules for manipulating the data. Thus, any powerful programming language should be able to describe primitive data and primitive procedures and should have methods for combining and abstracting procedures and data.

In this chapter we deal only with simple numerical data so that we can focus on the rules for building procedures. In later chapters we see that these same rules allow us to build procedures to manipulate compound data as well. In this section we look specifically at procedures.
