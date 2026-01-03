---
title: "The expected return time around a square"
layout: post
excerpt: "We explore the theory of Markov chains and geometric distributions using a random walk around a square."
tags:
  - puzzle
  - probability
---

Imagine you are standing on vertex $A$ of a square, with vertices labeled $A, B, C, D$ in clockwise order. Time starts at $t = 0$.

At each vertex, you play the following game: flip a fair coin repeatedly until it lands
Heads. As soon as you get Heads, you move clockwise to the next vertex and repeat the
same process. Coin flips are counted separately from time steps. Time steps measure the
number of vertex-to-vertex transitions, not the number of flips.

You continue around the square, flipping coins at each vertex, until you finally return
to vertex $A$.

Two questions naturally arise from this journey:

1. On average, how many coin flips does it take to return to vertex $A$?
2. On average, how many time steps does it take for you to return to vertex $A$?

There two questions can be answered using the theory of geometric distributions or using
the theory of Markov chains. We will describe both approaches.

## Geometric distributions

### Coin flips

Let $X$ be the number of flips of the fair coin made before returning to vertex $A$ and
let $X_{AB}, X_{BC}, X_{CD},$ and $X_{DA}$ be the number of coin flips made to transition
from vertex $A$ to vertex $B$, from vertex $B$ to vertex $C$, from vertex $C$ to vertex
$D$, and from vertex $D$ to vertex $A$, respectively.

Then, $X = X_{AB} + X_{BC} + X_{CD} + X_{DA}$. We are trying to determine $E[X]$, the
average number of flips of the coin made before returning to vertex $A$, which is
equivalent to determining $E[X_{AB}] + E[X_{BC}] + E[X_{CD}] + E[X_{DA}]$.

Each of $X_{AB}, X_{BC}, X_{CD},$ and $X_{DA}$ follow a
[Geometric distribution](https://en.wikipedia.org/wiki/Geometric_distribution) with
parameter $p = 1/2$. Hence, $E[X_{AB}] = E[X_{BC}] = E[X_{CD}] = E[X_{DA}] = 1/p = 2$
coin flips. So, the average number of coin flips needed to return to vertex $A$ is

$$
\begin{align*}
E[X] &= E[X_{AB}] + E[X_{BC}] + E[X_{CD}] + E[X_{DA}] \\
&= 2 + 2 + 2 + 2 \\
&= 8
\end{align*}
$$

### Time steps

Notice that from time $t = k$ until $t = T + \Delta k$ inclusive, for some number of
time steps $\Delta k$, there will be exactly $\Delta k + 1$ coin flips. In other words,
the number of time steps it takes to make $x$ coin flips is $x - 1$.

This means that $X_{AB}, X_{BC}, X_{CD},$ and $X_{DA}$ coin flips will take
$X_{AB} - 1, X_{BC} - 1, X_{CD} - 1,$ and $X_{DA} - 1$ time steps, respectively. If we
let $T$ be the number of time steps it takes to return to vertex $A$, then

$$
\begin{align*}
T &= X_{AB} - 1 + X_{BC} - 1 + X_{CD} - 1 + X_{DA} - 1 \\
&= X_{AB} + X_{BC} + X_{CD} + X_{DA} - 4 \\
&= X - 4
\end{align*}
$$

and so the average number of time steps required to return to vertex $A$ is
$E[T] = E[X] - 4 = 8 - 4 = 4$.

## Markov chains

### Time steps



### Coin flips


At time time $t = 0$ on vertex A of a square with vertices A, B, C, and D. For t = 1, 2, ..., you move to either vertex B or vertex D with probability 1/4 each and stay at vertex A with probability 1/2. Same goes for all other vertices, but you can only move to neighboring vertices (i.e. A can move to B or D, from B we can move to A or C, from C we can move to B or D, and from D we can move to A or C)

On average, how many time-steps does it take to return to vertex A?

Solve this problem in a step-by-step manner by defining the random process X_t as a discrete-time Markov chain with four states {A, B, C, D} and then define T_A = \min{t \geq 1 \mid X_t = A} as the stopping time and then we want to compute E[T_A]
