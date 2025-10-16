---
title: "Fencepost errors"
layout: post
tags:
  - combinatorics
---
We describe what fencepost errors are, how to avoid them, and provide a precise derivation for why they occur.

Suppose that you're working on a large codebase in your favorite editor, and suppose that you want to find out how many lines of code a particular class in this codebase takes up.

To do this, you find the first line of code that the class starts on, say line $7$, and then you find the last line of code that the class ends on, say line $373$. You then compute $373 - 7 = 366$ and conclude that this class takes up $366$ lines of code.

However, the correct answer is actually $367$ lines of code, since the last line, line $373$, was not counted in the original answer.

This is an example of a *fencepost* error. The word "fencepost" comes from one of the simplest examples where fencepost errors occur:

> If you build a fence that is $10$ feet long with posts spaced $1$ foot apart, and assuming that each fence section consists of $2$ posts, how many posts do you need to build the fence?

If we divide $10$ feet of fence by $1$ foot per fence section, then we obtain $10$ fence sections. However, each fence section consists of $2$ posts, so while there will be $10$ fence sections, there will be $11$ posts.

We can resolve the fencepost error in the example above by writing down the sequence of lines of code as

$$
\begin{equation}
7, 8, \dots, 373 \label{loc_seq}
\end{equation}
$$

Then, [because](https://en.wikipedia.org/wiki/Cardinality#Equinumerosity) two sequences have the same length if there exists a bijection between them, we can determine the length $N$ of $\eqref{loc_seq}$ by finding a bijection between it and the sequence $1,2,\dots,N$ as follows,

$$
\begin{align*}
&7, 8, \dots, 373 \\
&0, 1, \dots, 366 \\
&1, 2, \dots, 367
\end{align*}
$$

Hence, the length $N$ of $\eqref{loc_seq}$ is $367$. 

More formally, consider the integers $r$ and $a$ such that $a > r$. What is the length of the sequence $r, r+1, r+2, \dots, a$? That is, how many integers are there in this sequence?

As done above, the length of this sequence can be determined by finding a bijection between this sequence and the sequence $1,2,\dots,N$, where $N \in \mathbb N$ is the length of the sequence and $N > 1$, as follows,

$$
\begin{align}
&r,r+1,r+2,\dots,a \label{gen_seq1} \\
&0,1,2,\dots,(a-r) \label{gen_seq2} \\
&1,2,3,\dots,(a-r) + 1 \label{gen_seq3}
\end{align}
$$

where $\eqref{gen_seq2}$ is obtained from $\eqref{gen_seq1}$ by subtracting $r$ from each element in $\eqref{gen_seq2}$ and $\eqref{gen_seq3}$ is obtained from $\eqref{gen_seq2}$ by adding $1$ to each element in $\eqref{gen_seq2}$. Hence, the length of the sequence $r,r+1,r+2,\dots,a$ is $(a-r) + 1$.

We can generalize this reasoning to the more general sequence $r,r+b,r+2b,\dots,a$, such that

* $r \in \mathbb N \cup \{0\}$,
* $b \in \mathbb Z \setminus \{0\}$,
* the difference between consecutive elements in the sequence is $k \in \mathbb N$, not $1$, and
* $a = r + q \cdot b$ for some $q \in \mathbb Z$.

The length of this more general sequence can be obtained as follows,

$$
\begin{align*}
&r,r+b,r+2b,\dots,a \\
&0,b,2b,\dots,(a-r) \\
&0,1,2,\dots,\frac{a-r}{b} \\
&1,2,3,\dots,\frac{a-r}{b} + 1
\end{align*}
$$

Hence, the length of the sequence $r,r+b,r+2b,\dots,a$ is $\frac{a-r}{b} + 1$. It is interesting to note that because $a = r + q \cdot b$, then $a, r, q,$ and $b$ can be interpreted as the dividend, remainder, quotient, and divisor, respectively, when $a$ is divided by $b$ via Euclidean division.

<!-- <label for="sn-euclidean-division" class="margin-toggle sidenote-number"></label><input type="checkbox" id="sn-euclidean-division" class="margin-toggle"><span class="sidenote">Because $a = r + q \cdot b$, then $a, r, q,$ and $b$ can be interpreted as the dividend, remainder, quotient, and divisor, respectively</span> -->

This general case is useful when determining how many "points" there are in a given interval. For example, suppose that you are trying to run a numerical simulation over time. You have the desired total simulation time $T > 0$ and a fixed time step size $\Delta t > 0$. Given this information, you would like to compute how many points in time that the simulation will step through, denoted $N$.

# TODO

That is, you would like to determine the length of the sequence $0, \Delta t, 2\Delta t, \dots, T$. As done above, we can determine the length of this sequence by finding a bijection between this sequence and the sequence $1,\dots,c$, where $c \in \mathbb N$ is the length of the sequence and $c > 1$, as follows:

$$
\begin{align}
&0, \Delta t, 2\Delta t, \dots, T \tag{2.1} \\
&0, 1, 2, \dots, \frac{T}{\Delta t} \tag{2.2} \\
&1, 2, 3, \dots, \frac{T}{\Delta t} + 1 \tag{2.3}
\end{align}
$$

where $(2.2)$ is obtained from $(2.1)$ by dividing each element in the sequence by $\Delta t$ and $(2.3)$ is obtained from $(2.2)$ by adding $1$ to each element in the sequence. Hence, the number of time points that this simulation will step through is $\frac{T}{\Delta t} + 1$.

A similar but slightly different problem is to determine the number of time-steps, not time points, that the simulation will step through. That is, to determine the length of the sequence $\Delta t, 2\Delta t, \dots, T$

By following the steps above, we can transform this sequence into the sequence $1,2,\dots,\frac{T}{\Delta t}$, such that the number of time-steps is $\frac{T}{\Delta t}$.

More generally, consider the integers $a, b, q,$ and $r$ such that $b = q \cdot a + r$ and $a \neq 0$ (for those with a keen eye, the equation $b = q \cdot a + r$ represents the Euclidean division of the dividend $b$ by the divisor $a$, such that the quotient is $q$ and the remainder is $r$).

Suppose that we want to determine the length of the sequence $r, 1 \cdot a + r, 2 \cdot a + r, \dots, q \cdot a + r = b$. As above, we compute the length of this sequence by transforming it into the sequence $1,\dots,c$, where $c \in \mathbb N$ is the length of the sequence and $c > 1$, as follows:

$$
\begin{align}
&r, 1 \cdot a + r, 2 \cdot a + r, \dots, q \cdot a + r \\
&0, 1 \cdot a, 2 \cdot a, \dots, q \cdot a \\
&0, 1, 2, \dots, q \\
&1, 2, 3, \dots, q + 1
\end{align}
$$

Hence, the length of this sequence is $q + 1$. Because $q \cdot a + r = b$, we can also write $q + 1$ as $\frac{b-r}{a} + 1$.

In the numerical simulation over time example above, we let $b = T, r = 0,$ and $a = \Delta t$ to get $\frac{T-0}{\Delta t} + 1 = \frac{T}{\Delta t} + 1$.

To summarize, to compute the correct number of objects and avoid fencepost errors, do the following:
1. Given the end-points of the sequence of objects, write down the full sequence of objects that you would like to count. Be sure to avoid including any objects that you do not want to count.
2. Transform the sequence from step 1 into the canonical sequence $1,\dots,c$, where $c \in \mathbb N$ and $c > 1$.
3. The desired number of objects is $c$.

Fencepost errors occur when the sequence that is written down in step 1 includes objects that should not be counted or does not include objects that should be counted.

For further reading on fencepost errors, see [this article](https://betterexplained.com/articles/learning-how-to-count-avoiding-the-fencepost-problem/).