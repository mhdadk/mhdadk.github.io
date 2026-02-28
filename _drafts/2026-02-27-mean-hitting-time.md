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
from the other states. This is often referred to as the _mean hitting time_ of states
0 and 3.

Unfortunately, most derivations of mean hitting times that I've found (such as the one on [this page](https://www.probabilitycourse.com/chapter11/11_2_5_using_the_law_of_total_probability_with_recursion.php)) tend to be "hand-wavy", where they skip some important steps. In this post, I aim
to be as clear and precise as possible when deriving the mean hitting times of states 0
and 3.

First, let the MC represented in Fig. 1 correspond to the random process
$\mathcal X = \lbrace X_0, X_1, X_2, \dots \rbrace$, where
$X_n \in \lbrace 0, 1, 2, 3\rbrace$ for each $n \in \lbrace 0, 1, \dots\rbrace$. Then,
let the hitting time of state $j$ be

$$
T_{j} = \min \lbrace n \geq 0 \mid X_n = j\rbrace
$$

Moreover, let $T_j \mid X_0 = i$ be the hitting time of state $j$ starting from state $i$.
Because each possible trajectory of $\mathcal X$ is random, then $T_{j}$ is a random
variable. For example, one possible trajectory of $\mathcal X$ is
$\lbrace 1, 2, 1, 0, 0, 0, \dots\rbrace$, such that $(T_0 \mid X_0 = 1) = 3$. Finally,
the mean hitting time of state $j$ starting from state $i$ is $E[T_{j} \mid X_0 = i]$.

We want to compute $E[T_{j} \mid X_0 = i]$ for $(i, j) \in \lbrace (1, 0), (2, 0), (1, 3), (2, 3)\rbrace$.
We skip the other $(i, j)$ pairs because states 0 and 3 are recurrent. We now derive an
expression for $E[T_{j} \mid X_0 = i]$. Let

$$
T_{j}(k) = \min \lbrace n \geq k \mid X_n = j\rbrace
$$

such that $T_j(k) \mid X_k = i$ is the hitting time of state $j$ starting from state $i$
at time $k$ and $T_j(0) = T_j$. Note that $T_j(0) = 1 + T_j(1)$. That is, the hitting
time of state $j$ starting from time 0 is $1$ time-step more than the hitting time
of state $j$ starting from time 1.

For example, if the hitting time of state $j$ starting at time 1 is $m$ for some
$m \in \lbrace 1, 2, \dots\rbrace$, then the hitting time of state $j$ starting at time
0 is $m + 1$. This means that

$$
\begin{align}
E[T_j \mid X_0 = i] &= E[T_j(0) \mid X_0 = i] \nonumber \\
E[1 + T_j(1) \mid X_0 = i] \nonumber \\
&= 1 + E[T_j(1) \mid X_0 = i] \label{eq1} \\
&= 1 + E[E[T_j(1) \mid X_0 = i, X_1]] \label{eq2}
\end{align}
$$

where we used the law of total expectation to go from $\eqref{eq1}$ to $\eqref{eq2}$. The
expression $E[E[T_j(1) \mid X_0 = i, X_1]]$ expands to

$$
\begin{align}
E[E[T_j(1) \mid X_0 = i, X_1]] &= \sum_{s=0}^3 \Pr(X_1 = s \mid X_0 = i) \cdot E[T_j(1) \mid X_0 = i, X_1 = s] \label{eq3} \\
&= \sum_{s=0}^3 \Pr(X_1 = s \mid X_0 = i) \cdot E[T_j(1) \mid X_1 = s] \label{eq4} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_j(1) \mid X_1 = s] \label{eq5}
\end{align}
$$

where we used the Markov property to go from $\eqref{eq3}$ to $\eqref{eq4}$ and we used
the fact that the MC in Fig. 1 is time-homogeneous to go from $\eqref{eq4}$ to $\eqref{eq5}$.
For the sake of brevity, let $E[T_j(k) \mid X_k = s] = \tau_{j \mid s}^k$. Then, to summarize,

$$
\tau_{j \mid i}^0 = 1 + \sum_{s=0}^3 p_{is} \cdot \tau_{j \mid s}^1
$$

We can now solve for all 4 mean hitting times simultaneously as follows.

$$
\begin{align*}
\tau_{0 \mid 1}^0 &= 1 + \sum_{s=0}^3 p_{1s} \cdot \tau_{0 \mid s}^1 \\
&= 1 + p_{10} \cdot \tau_{0 \mid 0}^1 + p_{12} \cdot \tau_{0 \mid 2}^1 \\
&= 1 + \frac{1}{3} \cdot \tau_{0 \mid 0}^1 + \frac{2}{3} \cdot \tau_{0 \mid 2}^1 \\

\tau_{0 \mid 2}^0 &= 1 + \sum_{s=0}^3 p_{2s} \cdot \tau_{0 \mid s}^1 \\
&= 1 + p_{21} \cdot \tau_{0 \mid 1}^1 + p_{23} \cdot \tau_{0 \mid 3}^1 \\
&= 1 + \frac{1}{2} \cdot \tau_{0 \mid 1}^1 + \frac{1}{2} \cdot \tau_{0 \mid 3}^1 \\

\tau_{3 \mid 1}^0 &= 1 + \sum_{s=0}^3 p_{1s} \cdot \tau_{3 \mid s}^1 \\
&= 1 + p_{10} \cdot \tau_{3 \mid 0}^1 + p_{12} \cdot \tau_{3 \mid 2}^1 \\
&= 1 + \frac{1}{3} \cdot \tau_{3 \mid 0}^1 + \frac{2}{3} \cdot \tau_{3 \mid 2}^1 \\

\tau_{3 \mid 2}^0 &= 1 + \sum_{s=0}^3 p_{2s} \cdot \tau_{3 \mid s}^1 \\
&= 1 + p_{21} \cdot \tau_{3 \mid 1}^1 + p_{23} \cdot \tau_{3 \mid 3}^1 \\
&= 1 + \frac{1}{2} \cdot \tau_{3 \mid 1}^1 + \frac{1}{2} \cdot \tau_{3 \mid 3}^1
\end{align*}
$$

Solving these 4 linear equations simultaneously yields

$$
\begin{align}
\end{align}
$$
