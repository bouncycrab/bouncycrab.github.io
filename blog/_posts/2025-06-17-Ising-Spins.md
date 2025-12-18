---
layout: post
blog-category: blog
title: Roleplaying as Crypto(graphy) Designers
author: Yu Xuan
image: /img/lattice.png
---

Something that I don't enjoy about the study of cryptography is that most of the time, we are just consumers of cryptographic schemes. We read papers, understand the schemes, and maybe implement them using well-known libraries (e.g., OpenSSL, libsodium, etc.).

But we rarely get to design our own schemes, or even think about how to design them.

## Roleplaying as Crypto(graphy) Designers
Anyone that has taken a cryptography course would know how a basic RSA scheme works. You pick two large primes $p$ and $q$, compute $n = p \times q$, pick an encryption exponent $e$, compute the decryption exponent $d$, and publish $(n, e)$ as the public key.

But have you ever wondered why we pick $e = 65537$ so often? (Ans: low Hamming weight + sufficiently large). What are the tradeoffs of picking a larger or smaller $e$?

Well, today I'm not here to talk about RSA, but rather to talk about **Lattice-based cryptography**, and the inspirations and motivations behind the design of some of the schemes.

## Security Needs: What properties make certain objects good for crypto primitives?

* **Hash functions:**
    * Compression function (maps larger input to smaller output).
    * Collision resistance (hard to find $x_1, x_2$ such that $H(x_1) = H(x_2)$).
* **Encrypt/Decrypt:**
    * Asymmetry in hardness of computation.
    * Existence and uniqueness (of private key).
    * Ease of scalability.

## SIS (Shortest Integer Solution) 

Introduced by Ajtai in 1996, the SIS problem is a foundational problem in lattice-based cryptography.

[Image of a lattice in cryptography with vectors illustrating the Shortest Integer Solution problem]

$$\text{SIS}(n, m, q, B): \text{ Given } A \in_R \mathbb{Z}_q^{n \times m}, \text{ find } z \in \mathbb{Z}^m \text{ s.t. } Az = 0 \pmod{q}, \text{ where } z \neq 0 \text{ and } z \in [-B, B]^m \text{ (and } B \ll q/2).$$

* $\mathbb{Z}_q = \{0, 1, \dots, q-1\}$
* $x \in_R S$ means $x$ is uniformly chosen from $S$.
* All vectors are column vectors.

> **Example:**
> * Let $n = 3$, $m = 5$, $q = 13$, and $B = 3$.
> * **SIS instance:** >   $$A = \begin{pmatrix} 1 & 0 & 7 & 12 & 4 \\ 2 & 11 & 3 & 6 & 12 \\ 9 & 8 & 10 & 5 & 1 \end{pmatrix}$$
> * We need to find nonzero $z = (z_1, z_2, z_3, z_4, z_5) \in [-3,3]^5$ with $Az \equiv 0 \pmod{13}$.
> * Some solutions within our bound $[-3,3]^5$ are:
>   $z_1 = \pm (3,1,-1,0,1)$
>   $z_2 = \pm (-1,0,2,1,-2)$

---

## Basic Linear Algebra

As a designer, one must not make "stupid" problems. 
For example, if $n \ge m$, then one expects that $Az = 0 \pmod{q}$ has no non-trivial solutions (since it is likely full rank). Hence, we assume $n < m$.

We also cannot design problems with no solutions. To ensure a solution exists, we must have sufficiently many choices of $z$.

**Remark:** If $(B+1)^m > q^n$, then by the Pigeonhole Principle there must exist $z_1, z_2 \in [-B/2, B/2]^m$ such that $z_1 \neq z_2$ and $Az_1 = Az_2 \pmod{q}$. Then, $z = z_1 - z_2$ is a SIS solution. 

As designers, we can construct a SIS problem as long as $m > \frac{n \log q}{\log(B+1)}$, as a solution is guaranteed to exist.

## Creating a Hash Function from SIS

We can use SIS to construct collision-resistant hash functions.
1. Select $A \in_R \mathbb{Z}_q^{n \times m}$, where $m > n \log q$.
2. Define $H_A : \{0,1\}^m \rightarrow \mathbb{Z}_q^n$ by $H_A(z) = Az \pmod q$.

$H_A$ works as a compression function since $m > n \log q \implies 2^m > q^n$.

**Collision Resistance:** Suppose one can efficiently find $z_1, z_2 \in \{0,1\}^m$ with $z_1 \neq z_2$ and $H_A(z_1) = H_A(z_2)$. Then $Az_1 = Az_2 \pmod{q}$, which means $Az = 0 \pmod{q}$ where $z = z_1 - z_2$. Since $z \neq 0$ and $z \in [-1,1]^m$, $z$ is a SIS solution (with $B = 1$) that has been efficiently found.

## Conclusion
As a cryptographic scheme designer, one must always consider the properties of the underlying mathematical problems. In the case of SIS, existence is guaranteed by the pigeonhole principle, and collision resistance is tied to the hardness of finding solutions.

Thanks for reading! (I will improve at writing blog posts one day i realised how incoherent everything reads)