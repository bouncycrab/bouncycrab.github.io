---
layout: post
blog-category: blog
title: CVar Optimization for SVP
author: Yu Xuan
image: /img/ccvar.png
---

Hello, it seems that I am keeping up with blogging weekly, so here is another blog post.

After my previous meeting with my two supervisors, I was tasked to play around with CVar quantum optimization. For more information, you can refer to the paper [here](https://quantum-journal.org/papers/q-2020-04-20-256/pdf/).

I didn't know what Cvar was, and funnily enough it was a finance concept to deal with expected losses in a portfolio. 
Anyways, the whole point of CVar for quantum optimization was to allow optimization for "low-cost hamiltonians" instead of just optimizing over the expected value, which is just
$$
\min_\theta \langle \psi(\theta) | H | \psi(\theta) \rangle,
$$. 


Anyways, I used it to optimize my parameters for my QAOA circuits that I made to solve the Shortest Vector Problem (SVP) in lattices.

## Results

![Cvar Diagram](/img/Cvar.png)

Noticably, accross many instances of random lattices (I used randomly generated lattices with dim 3) it was observable that Cvar provided better results than using the normal expected value optimization.

## Cool, but whats next?
More papers to read I guess AHAHHHAAHA