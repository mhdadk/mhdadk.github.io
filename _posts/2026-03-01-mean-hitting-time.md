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
the mean hitting time for any one of the recurrent states in the MC when the MC starts
from the non-recurrent states (i.e. states 1 and 2). That is, on average, how long does
it take the MC to enter state 0 or 3 if it starts from state 1 and if it starts from
state 2?

Unfortunately, most derivations of mean hitting times that I've found (such as the one
on [this page](https://www.probabilitycourse.com/chapter11/11_2_5_using_the_law_of_total_probability_with_recursion.php))
tend to be "hand-wavy", where they skip some important steps. In this post, I aim
to be as clear and precise as possible when deriving the mean hitting time.

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

Notice that both $m_{0 \mid 3}$ and $m_{3 \mid 0}$ are infinite, so we instead want to
compute the mean hitting for either state 0 or state 3, and not exclusively one or the
other. This would then mean that $m_{0 \textrm{or} 3 \mid 3}$ and
$m_{0 \textrm{or} 3 \mid 0}$ are both $0$. Moreover, we want to avoid infinite expected
values.

Hence, we generalize the notation introduced above as follows. First, let

$$
T_{j_1,j_2,\dots ,j_m} = \min \lbrace n \ge 0 \mid X_n \in \lbrace j_1, \dots, j_m\rbrace \rbrace
$$

be the hitting time for any one of states $j_1,\dots,j_m$. Then, for
$i \in \lbrace 0,1,2,3 \rbrace$, let $m_{j_1,\dots,j_m \mid i} = E[T_{j_1,\dots,j_m} \mid X_0 = i]$
be the mean hitting time for any one of the states $j_1,\dots,j_m$. We are interested
in computing $m_{0, 3 \mid 1}$ and $m_{0, 3 \mid 2}$.

If $X_0 \neq j$, then $T_j(0) = 1 + T_j(1)$. That is, the hitting time of state $j$
starting from time 0 is $1$ time-step more than the hitting time of state $j$ starting
from time 1.

For example, if the hitting time of state $j$ starting at time 1 is $m$ for some
$m \in \lbrace 1, 2, \dots\rbrace$, then the hitting time of state $j$ starting at time
0 is $m + 1$.

This identity holds only on the event $X_0 \ne j$. Since we are interested in $m_{j \mid i}$
for $i \ne j$, this condition is satisfied when $X_0=i$.

We now derive expressions for $m_{0, 3 \mid 1}$ and $m_{0, 3 \mid 2}$ as follows.

$$
\begin{align}
m_{0, 3 \mid i} &= E[T_{0, 3} \mid X_0 = i] \nonumber \\
&= E[T_{0, 3}(0) \mid X_0 = i] \nonumber \\
E[1 + T_{0, 3}(1) \mid X_0 = i] \nonumber \\
&= 1 + E[T_{0, 3}(1) \mid X_0 = i] \label{eq1} \\
&= 1 + E[E[T_{0, 3}(1) \mid X_0 = i, X_1]] \label{eq2}
\end{align}
$$

where we used the law of total expectation to go from $\eqref{eq1}$ to $\eqref{eq2}$. The
expression $E[E[T_{0, 3}(1) \mid X_0 = i, X_1]]$ expands to

$$
\begin{align}
E[E[T_{0, 3}(1) \mid X_0 = i, X_1]] &= \sum_{s=0}^3 \Pr(X_1 = s \mid X_0 = i) \cdot E[T_{0, 3}(1) \mid X_0 = i, X_1 = s] \label{eq3} \\
&= \sum_{s=0}^3 \Pr(X_1 = s \mid X_0 = i) \cdot E[T_{0, 3}(1) \mid X_1 = s] \label{eq4} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_{0, 3}(1) \mid X_1 = s] \label{eq5} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_{0, 3}(0) \mid X_0 = s] \label{eq6} \\
&= \sum_{s=0}^3 p_{is} \cdot E[T_{0, 3} \mid X_0 = s] \nonumber \\
&= \sum_{s=0}^3 p_{is} \cdot m_{0, 3 \mid s} \nonumber
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
m_{0, 3 \mid i} = 1 + \sum_{s=0}^3 p_{is} \cdot m_{0, 3 \mid s}
$$

We can now solve for the mean hitting times $m_{0, 3 \mid 1}$ and $m_{0, 3 \mid 2}$
simultaneously as follows. First,

$$
\begin{align}
m_{0, 3 \mid 1} &= 1 + \sum_{s=0}^3 p_{1s} \cdot m_{0, 3 \mid s} \nonumber \\
&= 1 + p_{10} \cdot m_{0, 3 \mid 0} + p_{11} \cdot m_{0, 3 \mid 1} + p_{12} \cdot m_{0, 3 \mid 2} + p_{13} \cdot m_{0, 3 \mid 3} \label{eq7} \\
&= 1 + p_{12} \cdot m_{0, 3 \mid 2} \label{eq8} \\
&= 1 + \frac{2}{3} \cdot m_{0, 3 \mid 2} \nonumber
\end{align}
$$

where we used the fact that $m_{0, 3 \mid 0} = m_{0, 3 \mid 3} = 0$ and $p_{11} = 0$
to go from $\eqref{eq7}$ to $\eqref{eq8}$. Next,

$$
\begin{align}
m_{0, 3 \mid 2} &= 1 + \sum_{s=0}^3 p_{2s} \cdot m_{0, 3 \mid s} \\
&= 1 + p_{20} \cdot m_{0, 3 \mid 0} + p_{21} \cdot m_{0, 3 \mid 1} + p_{22} \cdot m_{0, 3 \mid 2} + p_{23} \cdot m_{0, 3 \mid 3} \label{eq9} \\
&= 1 + p_{21} \cdot m_{0, 3 \mid 1} \label{eq10} \\
&= 1 + \frac{1}{2} \cdot m_{0, 3 \mid 1} \nonumber
\end{align}
$$

where we used the fact that $m_{0, 3 \mid 0} = m_{0, 3 \mid 3} = 0$ and $p_{22} = 0$
to go from $\eqref{eq9}$ to $\eqref{eq10}$. We now have two equations and two unknowns,
which when solved together yield

$$
\begin{align}
m_{0, 3 \mid 1} &= \frac{5}{2} \\
m_{0, 3 \mid 2} &= \frac{9}{4}
\end{align}
$$

as desired.

It is interesting to note that the fact that mean hitting times can be infinite when
considering hitting individual recurrent states (rather than any one of them) motivates
the need for classifying states into recurrent and transient classes, as explained [here](https://www.probabilitycourse.com/chapter11/11_2_4_classification_of_states.php).
