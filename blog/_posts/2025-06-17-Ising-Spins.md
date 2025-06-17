---
layout: post
blog-category: blog
title: Ising Spins and Quantum Computing
author: Yu Xuan
image: /img/qc.jpg
---

What exactly are Ising spin models and why must some problems be converted into them in order for quantum computers to solve them?

I encountered this question while I was reading about Quantum Algorithms used to solve NP-hard problems. In this post, I will try to motivate the use of Ising spin models and how they are related to some NP-hard problems.

## What are Ising Spins?

Let's take a look at a more formal definition of Ising spins first --

Ising spins are a mathematical model used in statistical mechanics to represent magnetic dipole moments of atomic spins. They are named after the physicist Ernst Ising, who introduced the model in his doctoral thesis in 1924.

In the Ising model, each spin can take one of two values, typically represented as +1 (up) or -1 (down). The spins are arranged on a lattice, and the interactions between neighboring spins determine the overall energy of the system. The Hamiltonian of the Ising model is given by:

$$
H = -J \sum_{<i,j>} S_i S_j - h \sum_i S_i
$$

where:
- $$\( S_i \)$$ is the spin at site \( i \) (either +1 or -1),
- \( J \) is the coupling constant that determines the strength of interaction between neighboring spins,
- \( h \) is the external magnetic field,
- the first sum runs over all pairs of neighboring spins, and
- the second sum runs over all spins in the system.
The Ising model is used to study phase transitions, magnetism, and critical phenomena in statistical physics. It has applications in various fields, including condensed matter physics, materials science, and even in optimization problems.

Okay... thats alot of words. But what does it mean?

## What does it mean?
I am going to ignore all the physics jargon and try to explain it in a more intuitive way. If you prefer a more formal definition, you can refer to the [this YouTube playlist](https://www.youtube.com/playlist?list=PLotxEOxVaaoKRXdDN-7lI3Y88PaHqyOZL).

Imagine you have a bunch of magnets, each of which can point either up or down. The Ising model is a way to describe how these magnets interact with each other and how they behave as a whole.

When you have a lot of magnets, they can either align with each other (all pointing up or all pointing down) or be in a mixed state (some pointing up, some pointing down). The Ising model helps us understand how these magnets behave based on their interactions and external influences.

Another analogy is to think about a classroom of students. Each student can either be happy (up) or sad (down). The Ising model helps us understand how the mood of the classroom changes based on how students interact with each other and external factors like a how much I like my friend or how much the teacher is scolding us.

## Okay... so how can this solve problems?
An Ising model is usually described by a Hamiltonian(Think of a matrix). So lets say in order to model some NP-hard problem, we have to consider \[2^n\] possible states, where n is the number of variables in the problem. This means that in order to solve this classically, we would have to consider a dimension 2^n matrix, which is infeasible for large n.

However, if we can describe such a matrix as an Ising model, we can use quantum computers to solve it. By formulating the \[2^n\] matrix as a physical system of Ising spins, and if we let the solution of the problem correspond to the lowest energy state of the system, we can use quantum annealing to find this lowest energy state. 

For exact Ising formulations of NP-hard problems you can refer to [this paper](https://arxiv.org/pdf/1302.5843).

## Conclusion
Main takeaway from this blogpost:
Suppose you have a problem that is too much to solve classically (think of a 2^n matrix), you can try to convert it into an Ising spin model using qubits. If you can do that, you can use quantum annealing to find the solution.

Any QUBO (Quadratic Unconstrained Binary Optimization) problem is essentially can be converted into an Ising spin model, and thus can be solved using quantum annealing. QUBO and Ising models simply differ by a change of basis so it is a bijective (might be using this term loosely) model.

If you are interested in learning more about quantum computing, I recommend checking out the [Qiskit documentation](https://qiskit.org/documentation/) and the [Qiskit textbook](https://qiskit.org/textbook/preface.html). They have a lot of resources to help you get started with quantum computing and quantum algorithms.