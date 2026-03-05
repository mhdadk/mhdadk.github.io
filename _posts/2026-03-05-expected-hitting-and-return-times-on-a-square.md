---
title: "The expected hitting and return times on a square"
layout: post
excerpt: "We explore the theory of Markov chains using a random walk around a square."
tags:
  - probability
  - puzzle
---

Imagine you are standing on vertex $A$ of a square with vertices labeled $A, B, C, D$ in
clockwise order. Time starts at $t = 0$.

At each vertex, you play the following game: flip a fair coin and move to the neighboring
vertex in clockwise order if it lands Heads and in counter-clockwise order if it lands
Tails. For example, suppose you are standing at vertex $B$ and flip the coin. If it
lands Heads, move to vertex $C$ and if it lands Tails, move to vertex $A$.

You repeat this game at each vertex, consuming 1 second of time each time you move from
one vertex to another. You also play this game for an infinite amount of time.

This random walk on the square can be naturally represented using the discrete-time and
time-homogeneous Markov chain (MC) shown in Fig. 1.

{% include figure.html
   filename="expected-hitting-and-return-time-on-square/mc-transient.svg"
   caption="Discrete-time MC with 4 states representing the square traversal problem."
   fignum=1
   scale=50
%}

Two questions naturally arise from this random walk:

1. Starting at vertex $A$, how long does it take, on average, to reach vertex $D$?
2. Starting at vertex $A$, how long does it take, on average, to return to vertex $A$?

We answer these questios using the concepts of _mean hitting time_ and _mean return time_.
Most explanations of mean hitting and return times found online, such as the one
on [this page](https://www.probabilitycourse.com/chapter11/11_2_5_using_the_law_of_total_probability_with_recursion.php), tend to be "hand-wavy",
where they skip some important steps. In this post, I aim to be as clear and precise as
possible when deriving expressions for the mean hitting and return times.

This post would not have been possible without the help of Misha Lavrov, who answered
my related questions [_Why are these absorption probabilities equal?_](https://math.stackexchange.com/q/5126471/652310) and [_How can I derive a general expression for the mean return time in a Markov chain?_](https://math.stackexchange.com/q/5127245/652310).

# Mean hitting time

We now answer the first question.

Let the MC shown in Fig. 1 be represented by the random process
$\mathcal X = \lbrace X_0, X_1, X_2, \dots \rbrace$, where
$X_n \in \lbrace A, B, C, D\rbrace$ for each $n \in \lbrace 0, 1, \dots\rbrace$. Then,
for each state $i$, let

$$
T_i(k) = \min \lbrace n \ge 0 \mid X_{n+k} = i \rbrace
$$

be the hitting time of state $i$ as measured from time $k$. For example, for the sample
trajectory $B, C, B, C, D, C, A, \dots$, $T_A(0) = 6, T_A(1) = 5, T_D(0) = 4,$ and
$T_D(2) = 2$.

It can be verified that for each state $i$, if $X_0 \neq i$, $T_i(0) = 1 + T_i(1)$. Moreover,
$T_i(0) \mid X_0 = j$ is the hitting time of state $i$ starting from state $j$ at time
$0$. This identity will be useful for the derivations that follow.

In the special case that $i = D$ and $j = A$, $T_D(0) \mid X_0 = A$ is
the hitting time of state $D$ starting from state $A$ at time $0$. So, we want to
compute $\tau_{AD} = E[T_D(0) \mid X_0 = A]$, the average time required to reach
vertex $D$ from vertex $A$ starting at time $0$. We derive an expression for $\tau_{AD}$
as follows.

$$
\begin{align}
\tau_{AD} &= E[T_D(0) \mid X_0 = A] \label{tauAD-1} \\
&= E[1 + T_D(1) \mid X_0 = A] \label{tauAD-2} \\
&= 1 + E[T_D(1) \mid X_0 = A] \label{tauAD-3} \\
&= 1 + E[E[T_D(1) \mid X_0 = A, X_1]] \label{tauAD-4}
\end{align}
$$

where

* $\eqref{tauAD-1} \to \eqref{tauAD-2}$ follows from $T_D(0) = 1 + T_D(1)$
(assuming $X_0 \neq D$).
* $\eqref{tauAD-2} \to \eqref{tauAD-3}$ by linearity of the conditional expectation.
* $\eqref{tauAD-3} \to \eqref{tauAD-4}$ follows from the law of total expectation.

Next, the expression $E[E[T_D(1) \mid X_0 = A, X_1]]$ in $\eqref{tauAD-4}$ expands to

$$
\begin{align}
E[E[T_D(1) \mid X_0 = A, X_1]] &= \sum_{s \in \lbrace A, B, C, D\rbrace} \Pr(X_1 = s \mid X_0 = A) \cdot E[T_D(1) \mid X_0 = A, X_1 = s] \label{exptauAD-1} \\
&= \sum_{s \in \lbrace A, B, C, D\rbrace} \Pr(X_1 = s \mid X_0 = A) \cdot E[T_D(1) \mid X_1 = s] \label{exptauAD-2} \\
&= \sum_{s \in \lbrace A, B, C, D\rbrace} p_{As} \cdot E[T_D(1) \mid X_1 = s] \label{exptauAD-3} \\
&= \sum_{s \in \lbrace A, B, C, D\rbrace} p_{As} \cdot E[T_D(0) \mid X_0 = s] \label{exptauAD-4} \\
&= \sum_{s \in \lbrace A, B, C, D\rbrace} p_{As} \cdot \tau_{sD} \label{exptauAD-5}
\end{align}
$$

where

* $\eqref{exptauAD-1} \to \eqref{exptauAD-2}$ follows from the Markov property.
* $\eqref{exptauAD-2} \to \eqref{exptauAD-3}$ by defining $p_{As} = \Pr(X_1 = s \mid X_0 = A)$ because the MC in Fig. 1 is time-homogeneous.
* $\eqref{exptauAD-3} \to \eqref{exptauAD-4}$ follows again from the MC being
time-homogeneous. Intuitively, the MC being time-homogeneous implies that the underlying
random process starting from time $1$ with $X_1=s$ behaves like a fresh copy of the chain
starting from time $0$ with $X_0 = s$. For a more rigorous version of this reasoning, see
[this answer](https://math.stackexchange.com/a/5126476/652310).

To summarize,

$$
\begin{align}
\tau_{AD} &= 1 + \sum_{s \in \lbrace A, B, C, D\rbrace} \tau_{sD} \cdot p_{As} \label{tauAD-5} \\
&= 1 + \tau_{AD} \cdot p_{AA} + \tau_{BD} \cdot p_{AB} + \tau_{CD} \cdot p_{AC} + \tau_{DD} \cdot p_{AD} \nonumber \\
&= 1 + \tau_{BD} \cdot p_{AB} \label{tauAD-6} \\
&= 1 + \tau_{BD} \cdot \frac{1}{2} \nonumber
\end{align}
$$

where $\eqref{tauAD-5} \to \eqref{tauAD-6}$ follows because
$\tau_{DD} = E[T_D(0) \mid X_0 = D] = 0$ and $p_{AA} = p_{AC} = 0$. Then, generalizing
$\eqref{tauAD-5}$,

$$
\begin{align}
\tau_{BD} &= 1 + \sum_{s \in \lbrace A, B, C, D\rbrace} \tau_{sD} \cdot p_{Bs} \nonumber \\
&= 1 + \tau_{AD} \cdot p_{BA} + \tau_{BD} \cdot p_{BB} + \tau_{CD} \cdot p_{BC} + \tau_{DD} \cdot p_{BD} \label{tauBD-1} \\
&= 1 + \tau_{AD} \cdot p_{BA} + \tau_{CD} \cdot p_{BC} \label{tauBD-2} \\
&= 1 + \tau_{AD} \cdot \frac{1}{2} + \tau_{CD} \cdot \frac{1}{2} \nonumber
\end{align}
$$

where $\eqref{tauBD-1} \to \eqref{tauBD-2}$ follows because $p_{BB} = \tau_{DD} = 0$. Next,

$$
\begin{align}
\tau_{CD} &= 1 + \sum_{s \in \lbrace A, B, C, D\rbrace} \tau_{sD} \cdot p_{Cs} \nonumber \\
&= 1 + \tau_{AD} \cdot p_{CA} + \tau_{BD} \cdot p_{CB} + \tau_{CD} \cdot p_{CC} + \tau_{DD} \cdot p_{CD} \label{tauCD-1} \\
&= 1 + \tau_{BD} \cdot p_{CB} \label{tauCD-2} \\
&= 1 + \tau_{BD} \cdot \frac{1}{2} \nonumber
\end{align}
$$

where $\eqref{tauCD-1} \to \eqref{tauCD-2}$ follows because
$p_{CA} = p_{CC} = \tau_{DD} = 0$. Notice that $\tau_{AD} = \tau_{CD}$. This makes sense
since both vertices $A$ and $C$ have the exact same possible transitions to vertex $D$ (they
are symmetric). Hence, this leaves us with two equations and two unknowns:

$$
\begin{align*}
\tau_{BD} &= 1 + \tau_{AD} \\
\tau_{AD} &= 1 + \frac{1}{2} \tau_{BD}
\end{align*}
$$

Solving these two equations together yields $\tau_{AD} = \tau_{CD} = 3$ and $\tau_{BD} = 4$.
That is, the average time required to go from vertices $A$ or $C$ to vertex $D$ is $3$ seconds,
while the average time required to go from vertex $B$ to $D$ is $4$ seconds.

# Mean return time

We now answer the second question, which was "Starting at vertex $A$, how long does it
take, on average, to return to vertex $A$?", using the concept of _mean return time_.

First, for each state $i$, let

$$
R_i(k) = \min \lbrace n \ge 1 \mid X_{n+k} = i \rbrace
$$

be the return time of state $i$ as measured from time $k$. For example, for the sample
trajectory $B, C, B, C, D, C, A, \dots$, $R_B(0) = 2$ and $R_C(1) = 2$.

It can be shown that $R_i(0) = 1 + T_i(1)$ for each state $i$, which will be useful
for the derivations that follow.

In the special case that $i = A,k = 0,$ and $j = A$, we want to
compute $r_A = E[R_A(0) \mid X_0 = A]$, the average time required to return to
vertex $A$ from vertex $A$ starting at time $0$. We derive an expression for $r_A$
as follows.

$$
\begin{align}
r_A &= E[R_A(0) \mid X_0 = A] \nonumber \\
&= E[1 + T_A(1) \mid X_0 = A] \nonumber \\
&= 1 + E[T_A(1) \mid X_0 = A] \nonumber \\
&= 1 + E[E[T_A(1) \mid X_0 = A, X_1]] \label{rA-4}
\end{align}
$$

Next, using a similar set of steps as the ones shown in the previous section, the expression
$E[E[T_A(1) \mid X_0 = A, X_1]]$ in $\eqref{rA-4}$ expands to $\sum_{s \in \lbrace A, B, C, D\rbrace} p_{As} \cdot \tau_{sA}$ such that

$$
r_A = 1 + \sum_{s \in \lbrace A, B, C, D\rbrace} p_{As} \cdot \tau_{sA}
$$

which is the expression for $r_A$ that we wanted to derive. By definition, $\tau_{AA} = 0$,
and we can compute $\tau_{BA}, \tau_{CA},$ and $\tau_{DA}$ using the method described
in the previous section to obtain $\tau_{BA} = \tau_{DA} = 3$ and $\tau_{CA} = 4$. Hence,

$$
\begin{align*}
r_A &= 1 + p_{AA} \cdot \tau_{AA} + p_{AB} \cdot \tau_{BA} + p_{AC} \cdot \tau_{CA} + p_{AD} \cdot \tau_{DA} \\
&= 1 + p_{AB} \cdot \tau_{BA} + p_{AD} \cdot \tau_{DA} \\
&= 1 + \frac{1}{2} \cdot 3 + \frac{1}{2} \cdot 3 \\
&= 4
\end{align*}
$$

That is, the average time required to return to vertex $A$, starting from vertex $A$, is
$4$ seconds.
