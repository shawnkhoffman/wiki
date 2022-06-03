---
title: "SICP – 1. Building Abstractions with Procedures"
authors: Shawn Hoffman
editors: 
summary: "These study notes are from Structure and Interpretation of Computer Programs - 2nd Edition (MIT Electrical Engineering and Computer Science) by Abelson, H. and Sussman, G."
tags: [SICP, Computer Science, SICP Chapter 1]
keywords: SICP, Computer Science, SICP Chapter 1
references: https://web.mit.edu/6.001/6.037/sicp.pdf, https://en.wikipedia.org/wiki/An_Essay_Concerning_Human_Understanding, https://archive.org/details/ucberkeley_webcast_l28HAzKy0N8, https://youtube.com/playlist?list=PLu-l3j4eka05K_tXNEVaOGy1Vb-oO0Y2s

sidebar: compsci_sicp_ch1_sidebar
folder: wiki/cs/ch1/1.1
---

## Simple ideas vs. Complex ideas

The concept of *simple ideas vs. complex ideas* in the human mind is important for context. All ideas form through experiences involving sensation and reflection. **Simple ideas** are ideas that enter the mind through the senses – unrefined, pure, and uncomplicated. Furthermore, the mind has the power of using simple ideas to reflect about other ideas which enter its realm, and of linking and uniting several simple ideas to produce one single idea. Therefore, when the mind compares, unites, and extenuates through the process of reflection, those ideas become **complex ideas**.

## Computational processes, Procedures, and Programs

We often say that, *"a computer program is a set of written instructions that tell the computer what to do."*

**Computational processes** are complex ideas and processes that perform computations abstractly. As they evolve, processes manipulate other abstract things called data. Processes can also manipulate other processes. The evolution of a process is directed by a pattern of rules called a program. People create **programs** to direct processes.

**Procedures** are the exact steps of a computational process, akin to recipes, and they are designed to perform specific tasks. In other languages, we would call these *functions.* Procedures take simple ideas such as primitives, means of combination, and means of abstraction (more on these later) and they turn them into complex ideas (computational processes).

Well-designed computational systems are designed in a modular manner, so that the parts can be constructed, replaced, and debugged separately.
