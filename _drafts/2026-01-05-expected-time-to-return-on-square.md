---
title: "Expected time to return on a square"
layout: post
excerpt: "We explore the theory of Markov Chains using a random walk puzzle on a square."
tags:
  - puzzle
  - probability
---

At time $t = 0$, you are placed on vertex $A$ of a square with vertices $A, B, C,$ and $D$. This square is shown in Fig. 1.

{% include figure.html
   filename="trapping-rain-water/trapping_rain_water_orig.jpg"
   caption='TEST'
   fignum=1
   scale=90
%}

At time $t = 1, 2, ...$, you move to either vert

At time time $t = 0$ on vertex A of a square with vertices A, B, C, and D. For t = 1, 2, ..., you move to either vertex B or vertex D with probability 1/4 each and stay at vertex A with probability 1/2. Same goes for all other vertices, but you can only move to neighboring vertices (i.e. A can move to B or D, from B we can move to A or C, from C we can move to B or D, and from D we can move to A or C)

On average, how many time-steps does it take to return to vertex A?

Solve this problem in a step-by-step manner by defining the random process X_t as a discrete-time Markov chain with four states {A, B, C, D} and then define T_A = \min{t \geq 1 \mid X_t = A} as the stopping time and then we want to compute E[T_A]
