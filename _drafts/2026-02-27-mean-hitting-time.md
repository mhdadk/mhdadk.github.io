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

Consider the discrete-time and time-homogeneous Markov chain (MC) shown in Fig. 1. There
are two absorbing (recurrent) states in this MC: states 0 and 3. Our goal is to determine
the mean hitting time of these states when the chain starts from states 1 or 2.

Unfortunately, most derivations of mean hitting times that I've found (such as the one on [this page](https://www.probabilitycourse.com/chapter11/11_2_5_using_the_law_of_total_probability_with_recursion.php)) tend to be "hand-wavy", where they skip some important steps. In this post, I aim
to be as clear and precise as possible when deriving the mean hitting times of states 0
and 3.

First, let the MC represented in Fig. 1 correspond to the random process
$\mathcal X = \lbrace X_0, X_1, X_2, \dots \rbrace$, where
$X_n \in \lbrace 0, 1, 2, 3\rbrace$ for each $n \in \lbrace 0, 1, \dots\rbrace$. Then,
let the hitting time of state $j$ be

$$
T_j = \min \lbrace n \ge 0 \mid X_n = j \rbrace
$$

$T_j$ is a (possibly infinite) random variable because the possible trajectories of
$\mathcal X$ are random. For example, one possible trajectory of $\mathcal X$ is
$\lbrace 1, 2, 1, 0, 0, 0, \dots\rbrace$, such that $(T_0 \mid X_0 = 1) = 3$.

For $i \in \lbrace 0,1,2,3 \rbrace$, define $m_{j \mid i} = E[T_j \mid X_0 = i]$.
Additionally, for $k \geq 0$, define $T_j(k) = \min \lbrace n \ge k \mid X_n = j \rbrace$
such that $T_j(k) \mid X_k = i$ is the hitting time of state $j$ starting from state $i$
at time $k$ and $T_j(0) = T_j$.

If $X_0 \neq j$, then $T_j(0) = 1 + T_j(1)$. That is, the hitting time of state $j$
starting from time 0 is $1$ time-step more than the hitting time of state $j$ starting
from time 1.

For example, if the hitting time of state $j$ starting at time 1 is $m$ for some
$m \in \lbrace 1, 2, \dots\rbrace$, then the hitting time of state $j$ starting at time
0 is $m + 1$.

This identity holds only on the event $X_0 \ne j$. Since we are interested in $m_{j \mid i}$
for $i \ne j$, this condition is satisfied when $X_0=i$.

We want to compute $m_{j \mid i}$ for $(i, j) \in \lbrace (1, 0), (2, 0), (1, 3), (2, 3)\rbrace$.
We skip the other $(i, j)$ pairs because states 0 and 3 are recurrent. We derive an
expression for $m_{j \mid i}$ as follows.

$$
\begin{align}
m_{j \mid i} &= E[T_j \mid X_0 = i] \nonumber \\
&= E[T_j(0) \mid X_0 = i] \nonumber \\
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
&= \sum_{s=0}^3 p_{is} \cdot E[T_j(1) \mid X_1 = s] \label{eq5} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_j(0) \mid X_0 = s] \label{eq6} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_j \mid X_0 = s] \nonumber \\
&= \sum_{s=0}^3 p_{is} \cdot m_{j \mid s} \nonumber
\end{align}
$$

where

* we used the Markov property to go from $\eqref{eq3}$ to $\eqref{eq4}$ and
* we used the fact that the MC in Fig. 1 is time-homogeneous to go from $\eqref{eq4}$ to $\eqref{eq5}$ and
from $\eqref{eq5}$ to $\eqref{eq6}$.

Intuitively, the MC being time-homogeneous implies that the underlying random process
starting from time 1 with $X_1=s$ behaves like a fresh copy of the chain starting
from time 0 with $X_0 = s$. For a more precise version of this reasoning, see
[this answer](https://math.stackexchange.com/a/5126476/652310). To summarize,

$$
m_{j \mid i} = 1 + \sum_{s=0}^3 p_{is} \cdot m_{j \mid s}
$$

**TODO**: Note that probabilitycourse.com assumes that the hitting state is either 0 or
3 and not only one or the other. This avoids the case when expected values can be
infinite. This is the issue encountered below. So, re-define the hitting time as the
time it takes to reach either state 0 or state 3.

We can now solve for all 4 mean hitting times simultaneously as follows. First,

$$
\begin{align}
m_{0 \mid 1} &= 1 + \sum_{s=0}^3 p_{1s} \cdot m_{0 \mid s} \nonumber \\
&= 1 + p_{10} \cdot m_{0 \mid 0} + p_{12} \cdot m_{0 \mid 2} \label{eq7} \\
&= 1 + p_{12} \cdot m_{0 \mid 2} \label{eq8}
\end{align}
$$

where we used the fact that $m_{0 \mid 0} = 0$ to go from $\eqref{eq7}$ to $\eqref{eq8}$.
Next,

$$
\begin{align}
m_{0 \mid 2} &= 1 + \sum_{s=0}^3 p_{2s} \cdot m_{0 \mid s} \\
&= 1 + p_{21} \cdot m_{0 \mid 1} + p_{23} \cdot m_{0 \mid 3} \\
\end{align}
$$




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
