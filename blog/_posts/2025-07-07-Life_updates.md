---
layout: post
blog-category: blog
title: BKZ algo using quantum oracles
author: Yu Xuan
image: /img/bkz.png
---

Hello to whoever is reading this, it seems like I have NOT been keeping up with blogging weekly, so here is another blog post almost one month later.

Let me just share another paper that I have been reading, which is [this one](https://arxiv.org/html/2505.08386v1)

It is a paper that describes how to use quantum oracles to speed up the BKZ algorithm for lattice problems. The paper is quite long, so I will just summarize the main points here.

## What is the BKZ algorithm?
The BKZ algorithm is a lattice reduction algorithm that is used to find short vectors in a lattice. It is a generalization of the LLL algorithm, which is a polynomial time algorithm for finding short vectors in a lattice. The BKZ algorithm is more powerful than the LLL algorithm, but it is also more expensive in terms of time and space complexity.

BKZ is defined by a "block size" parameter $$b$$. If one is familiar with the LLL algorithm, it is a generalization of the LLL algorithm where the block size is $b$ instead of 2. The BKZ algorithm works by reducing the lattice in blocks of size $b$, as compared to the LLL algorithm which reduces the lattice in blocks of size 2.

I am not going to explain BKZ here as there are plenty of resources online that explain it well, but it would help to learn LLL and its reduction conditions first before trying to learn BKZ.

## Diagram
![BKZ Diagram](/img/bkz.png)

This self made diagram probably isn't the best way to explain the BKZ algorithm, but it is a diagram that I made to help me understand the algorithm. Essentially, after an initial pre-processing using LLL, we apply a BKZ routine where there exist a subroutine that requires one to solve an SVP instance in a lattice of size $$b$$. This is where the quantum oracles come in, as one could use existing VQE/QAOA architectures to solve the SVP instance. One could then use the methods mentioned in my previous blog post to optimize the parameters of the quantum circuits used to solve the SVP instance.

![VQKZ Pseudo Code](/img/bkz2.png)

## Post processing of results
![Post processing Diagram](/img/postpros.png)
During the quantum oracle call, we would obtain a short vector in the lattice of size $$b$$. However, this vector is not guaranteed to be the shortest vector in the lattice. Thus, we would need to perform a post-processing step to ensure that the vector is indeed short enough. This is done by finding the most uncertain qubits after, say 1000 iterations of the quantum circuit, and then performing a classical search over these uncertain qubits to find the shortest vector in the lattice.

## Conclusion
My next direction is to learn about how ansatz circuits are made, and with my limited knowledge of quantum computing, I aim to learn as much as I can before my internship ends. I will also be trying to implement the quantum oracle for the BKZ algorithm using Qiskit, so stay tuned for that.