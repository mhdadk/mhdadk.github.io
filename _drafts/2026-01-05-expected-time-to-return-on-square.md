---
title: "The expected return time around a square"
layout: post
excerpt: "We explore the theory of Markov chains and geometric distributions using a random walk around a square."
tags:
  - puzzle
  - probability
---

Imagine you are standing on vertex $A$ of a square, with vertices labeled $A, B, C, D$ in clockwise order. Time starts at $t = 0$.

At each vertex, you play the following game: flip a fair coin repeatedly until it lands Heads. Each flip takes one unit of time, so after each flip, the clock increments by $1$. As
soon as you get Heads, you move clockwise to the next vertex and repeat the same process.

You continue around the square, flipping coins at each vertex, until you finally return
to vertex $A$.

Two questions naturally arise from this journey:

1. On average, how many coin flips does it take to return to vertex $A$?
2. On average, what is the time $t$ at which you return to $A$?

There two questions can be answered using the theory of geometric distributions or using
the theory of Markov chains. We will describe both approaches.

## Geometric distributions

### Coin flips

TODO: modify the explanation to use the geometric random variables $X_{AB}, X_{BC}, X_{CD}, X_{DA}$.

We can decompose the problem into four sequential and independent random experiments:

1. At vertex $A$, flip a fair coin repeatedly until you observe Heads, then move to vertex $B$.
2. At vertex $B$, flip a fair coin repeatedly until you observe Heads, then move to vertex $C$.
3. At vertex $C$, flip a fair coin repeatedly until you observe Heads, then move to vertex $D$.
4. At vertex $D$, flip a fair coin repeatedly until you observe Heads, then move to vertex $A$.

So, the average number of coin flips needed to move from vertex $A$ back to vertex $A$ is
equivalent to the average number of coin flips needed to move from vertex $A$ to vertex $B$,
from vertex $B$ to vertex $C$, from vertex $C$ to vertex $D$, and from vertex $D$ back
to vertex $A$.

Let $X$ be the number of flips of a fair coin required to observe the first Heads. Then,
$X$ follows a [Geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution)
with parameter $p = 1/2$. The mean of $X$ is $1/p = 2$ coin flips.

Hence, the average number of flips of a fair coin required to move between any two
vertices is $2$. Because there are 4 transitions needed to return to vertex $A$, then
the average number of flips needed to return to vertex $A$ is $2 \times 4 = 8$.

### Time

Notice that the number of coin flips so far at time $t = T$ will always be $T + 1$. To
see this, consider the following sample

## Markov chains

### Time

### Coin flips


At time time $t = 0$ on vertex A of a square with vertices A, B, C, and D. For t = 1, 2, ..., you move to either vertex B or vertex D with probability 1/4 each and stay at vertex A with probability 1/2. Same goes for all other vertices, but you can only move to neighboring vertices (i.e. A can move to B or D, from B we can move to A or C, from C we can move to B or D, and from D we can move to A or C)

On average, how many time-steps does it take to return to vertex A?

Solve this problem in a step-by-step manner by defining the random process X_t as a discrete-time Markov chain with four states {A, B, C, D} and then define T_A = \min{t \geq 1 \mid X_t = A} as the stopping time and then we want to compute E[T_A]
