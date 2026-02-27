---
title: "Mean hitting time"
layout: post
excerpt: "We derive the mean hitting time for an arbitrary discrete-time, finite state, and time-homogeneous Markov chain."
tags:
  - probability
---

Consider the discrete-time and time-homogeneous Markov chain (MC) shown in Fig. 1.

{% include figure.html
   filename="markov-chain.png"
   caption="Discrete-time Markov chain with 4 states."
   fignum=1
   scale=70
%}

There are 2 recurrent states in this MC: states 0 and 3. Our goal is to determine how
many transitions it takes, on average, for the MC to enter states 0 and 3 if it starts
from the other states. This is often referred to as the "mean hitting time".

Unfortunately, most derivations of mean hitting times that I've found tend to be
"hand-wavy", such that they skip some important steps. In this post, I will attempt to
be as clear as possible when deriving the mean hitting times.

First, we compute the mean hitting time for state 0. Let $T_0(k) = \inf \lbrace n \geq 0 \mid X_{n+k} = 0\rbrace$ be the hitting time for state 0 from time $k$ onwards, where the random process
$X_0, X_1, \dots$ represents the MC in Fig. 1 and $E[T_0(0)]$ is the mean hitting time
we are interested in computing.

Note that $T_0(0) = 1 + T_0(1)$. That is, the hitting time for state 0 from time $0$
onwards is one more than the hitting time for state 0 from time $1$ onwards. This makes
sense since if we perform a transition after time $0$, we've completed a time step. Then,

$$
\begin{align}
E[T_0(0) \mid X_0 = 1] &= E[1 + T_0(1) \mid X_0 = 1] \nonumber \\
&= 1 + E[T_0(1) \mid X_0 = 1] \label{eq1} \\
&= 1 + E[E[T_0(1) \mid X_0 = 1, X_1]] \label{eq2} \\
\end{align}
$$

where we used the law of total expectation to go from $\eqref{eq1}$ to $\eqref{eq2}$.
Note that

$$
\begin{align}
E[E[T_0(1) \mid X_0 = 1, X_1]] &= \sum_{k=0}^3 \Pr(X_1 = k \mid X_0 = 1) \cdot E[T_0(1) \mid X_0 = 1, X_1 = k] \\
&= \sum_{k=0}^3 \Pr(X_1 = k \mid X_0 = 1) \cdot E[T_0(1) \mid X_1 = k]
\end{align}
$$





Imagine you are standing on vertex $A$ of a square with vertices labeled $A, B, C, D$ in
clockwise order. Time starts at $t = 0$.

At each vertex, you play the following game: flip a fair coin repeatedly until it lands
Heads. As soon as you get Heads, you move clockwise to the next vertex and repeat the
same process.

You continue around the square, flipping coins at each vertex, until you finally return
to vertex $A$.

Two questions naturally arise from this journey:

1. On average, how many coin flips does it take to return to vertex $A$?
2. On average, how many time steps does it take for you to return to vertex $A$?

Note that coin flips are counted separately from time steps. Time steps measure the
number of vertex-to-vertex transitions, not the number of coin flips.

These two questions can be answered using the theory of geometric distributions or using
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

We now describe the Markov chain approach to solving this problem. This approach is
useful for generalizations to the problem where we can transition to more than one other
vertex on the square. Before describing the solution, we first review some Markov chain
concepts.

### Preliminaries

Consider the discrete-time random process $X_0, X_1, X_2, \dots$. We say that this
random process is a discrete-time Markov chain if

$$
\Pr(X_{t+1} = x_{t+1} \mid X_t = x_t, \dots, X_0 = x_0) = \Pr(X_{t+1} = x_{t+1} \mid X_t = x_t)
$$

for every $t, x_{t+1},x_t,\dots,x_0$. We assume that the range of each $X_t$ is a finite
set of "states", such that $X_t \in \{0, 1, 2, \dots, N-1\}$ for every $t$ and
"$X_t = k$" is read as "$X_t$ is in state $k$".

We refer to $\Pr(X_{t+1} = j \mid X_t = i)$ as the probability of transition from state
$i$ to state $j$, and we assume that these transition probabilities are time-invariant,
such that $\Pr(X_{T_0+1} = j \mid X_{T_0} = i) = \Pr(X_{T_1+1} = j \mid X_{T_1} = i)$
for every pair $(T_0, T_1)$. Hence, we can write $\Pr(X_{t+1} = j \mid X_t = i)$ as $p_{ij}$.

### Return time

Going back to our problem, consider the random process
$\\{X_t \mid t \in \\{0\\} \cup \mathbb N\\}$ that represents which vertex we are at
during each time step. Moreover, we assume that the range of each $X_t$ is $\\{A, B, C, D\\}$.
Because we always start at vertex $A$, then $X_0 = A$. Finally, we will assume that this
random process is a discrete-time Markov chain with the following transition probability
matrix:

$$
\begin{bmatrix}
p_{AA} & p_{AB} & 0 & 0 \\
0 & p_{BB} & p_{BC} & 0 & 0 \\
0 & 0 & p_{CC} & p_{CD} \\
p_{DA} & 0 & 0 & p_{DD}
\end{bmatrix}
$$

Next, let $T$ be a random variable that represents the number of time-steps required to
return to vertex $A$, such that

$$
T = \min\\{t \mid t \in \mathbb N, X_t = A\\}
$$

For example, consider the sample trajectory below.

| $t$  | $X_t$ |
| ---- | ----  |
| 0 | $A$ |
| 1 | $B$ |
| 2 | $B$ |
| 3 | $B$ |
| 4 | $C$ |
| 5 | $C$ |
| 6 | $D$ |
| 7 | $D$ |
| 8 | $D$ |
| 9 | $A$ |
| 10 | $A$ |
| 11 | $A$ |
| 12 | $B$ |

We see that

$$
\begin{align*}
T &= \min\\{t \mid t \in \mathbb N, X_t = A\\} \\
&= \min\\{9, 10, 11\\} \\
&= 9
\end{align*}
$$

for this sample trajectory. Our goal is to compute $E[T]$.

### Computing $E[T]$

First, note that when $X_1 \neq A$, then $T \mid X_1 \neq A = 1 + T$


### Time steps



### Coin flips


At time time $t = 0$ on vertex A of a square with vertices A, B, C, and D. For t = 1, 2, ..., you move to either vertex B or vertex D with probability 1/4 each and stay at vertex A with probability 1/2. Same goes for all other vertices, but you can only move to neighboring vertices (i.e. A can move to B or D, from B we can move to A or C, from C we can move to B or D, and from D we can move to A or C)

On average, how many time-steps does it take to return to vertex A?

Solve this problem in a step-by-step manner by defining the random process X_t as a discrete-time Markov chain with four states {A, B, C, D} and then define T_A = \min{t \geq 1 \mid X_t = A} as the stopping time and then we want to compute E[T_A]
