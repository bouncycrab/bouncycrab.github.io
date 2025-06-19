---
layout: post
blog-category: blog
title: Novel Probabilistic Qubit Encodings?
author: Yu Xuan
image: /img/qubit.png
---

Hello, this is Yu Xuan here. (Obviously, because this is my website.)

Recently during my A*STAR internship I was working on a project where I had to map SVP problems into QUBO / Ising problems.

Naturally, the problem of finding the shortest vector in a lattice corresponded to finding the ground state of a quantum system. But the nature of this problem also forces the shortest vector to be non zero.

This results in having to find a way to either 
1. Find the second least excited state, or
2. Find a way to encode the zero vector in a way that it is not the ground state.

Naively, suppose we want to encode $x_i$ (this is an integer) into a binary encoding $b_{i,1}, b_{i,2}, \ldots, b_{i,k}$ where $k$ is the number of bits used to encode $x_i$.
We can do this by using the following encoding:
$$
x_i = -a_i + c_{ii} + \omega_i(a + 1) + \sum_{j=0}^{\lfloor\log(a_i-1)\rfloor-1} 2^j x_{ij} + (a_i - 2^{\lfloor\log(a_i-1)\rfloor}) \cdot x_{i,\lfloor\log(a_i-1)\rfloor}
$$
Note that it is provable (using some inelegant inequality bashing that I will not write here) that this encoding results in $c_{ii} = 1$ if $x_i = 0$ and $c_{ii} = 0$ otherwise.
So it suffices to "penalize" the case where $c_{ii} = 1$ in the Hamiltonian.

Now this idea, as compared to the naive binary encoding idea, results in a use of extra linear (specifically 3n) qubits, but it allows us to encode the zero vector in a way that it is not the ground state.

However, I felt that this encoding is too strict as the space complexity of the encoding goes from a O(lg(n)) to O(n) in the number of bits used to encode the integer.

So, instead of penalizing each $x_i$ individually, one could penalize a constant number of $x_i$'s at a time.

# What does this mean?

E.g. if we penalize $x_1 = 0$, then the zero vector would not be the ground state of the system anymore. However, suppose if the SVP solution contains $x_1 = 0$, we would be penalizing the solution as well.

# Why i think it doesn't matter?
The conditional probability of the SVP solution having $x_1 = 0$ (Or i guess $x_i$ WLOG) is very low, so any cryptographially relavant lattice would not have this problem anyways.

Using handwave and napkin mathematics, SVP solution follows a Gaussian heuristic, and (heuristically) one could show that any coefficient in the SVP follows that distribution as well. As the rank of the lattice increases, the probability of having a coefficient of 0 in the SVP solution decreases linearly.

# Alternative Ansatz idea
Instead of introducing the penality inside the Hamiltonian, one could also alter the Mixer Hamiltonian by leaving all the amplitides of certain states unchanged. I.e. if there are $2^n$ states, one could leave the amplitudes of certain states at $1/\sqrt{2^n}$ instead of altering it during the mixing process.

![Mixer Hamiltonian diagram](/img/mixer.png)
The left diagram shows the original mixer Hamiltonian where suppose there exist some unique encoding of the zero vector (For example, take it to be |0000>). By construction, this mixer results in all the 'CNOT' (C-Rotate gates?) to NOT be applied to the zero state, so the zero state has its amplitude left unchanged since now the Mixer and Cost hamiltonians commute.

The right diagram shows the modified mixer Hamiltonian where instead of checking |0000>, we penalize |0xxx> where x can be either 0 or 1. This results $2^{n-1}$ states to have their amplitudes left unchanged, so the zero state is not the ground state anymore, alongside the states |0xxx>.

# Why is this useful?
By leaving the amplitudes of $2^{n-1}$ states (instead of one state) unchanged, by the law of total probability, naively I had thought the probability of measuring the zero state would be higher than the original mixer Hamiltonian. Experimentally, I noticed no significant difference in the probability of measuring the zero state, so I guess this is not useful. D:. Lol.


# Cool, what's next?
Well, I thought that this would be a pretty cool idea to reduce the reliance on Qubits needed to solve any SVP instance albeit it makes the algorithm non deterministic / probablistic, but anyways I wrote it down here as a blog post.
However, a month ago a [new paper](https://arxiv.org/pdf/2505.08386) was released that claims to solve SVP using projected sublattices instead. So the space complexity relies on some parameter $\beta$ instead of the rank of the lattice, which drastically reduces the space complexity of the problem since you can choose $\beta$.

This uses the deep insertion LLL technique which is also known as the $\beta$-BKZ algorithm. I guess I will read more and write a blog post about it later.
