---
title: "Fencepost errors"
layout: post
tags:
  - combinatorics
---
We describe what fencepost errors are, how to avoid them, and provide a precise derivation for why they occur.

# Motivation
Suppose that you're working on a large codebase in your favorite editor and suppose that you want to find out how many lines of code a particular class in this codebase takes up.

To do this, you find the first line of code that the class starts on, say line $7$, and then you find the last line of code that the class ends on, say line $373$. You then compute $373 - 7 = 366$ and conclude that this class takes up $366$ lines of code.

However, this answer is incorrect. The class actually takes up $367$ lines of code, since the last line, line $373$, was not counted in the original answer.

This is an example of a *fencepost* error. The word "fencepost" comes from one of the simplest examples where fencepost errors occur:

> If you build a fence that is $10$ feet long with posts spaced $1$ foot apart, and assuming that each fence section consists of $2$ posts, how many posts do you need to build the fence?

If we divide $10$ feet of fence by $1$ foot per fence section, then we obtain $10$ fence sections. However, each fence section consists of $2$ posts, so while there will be $10$ fence sections, there will be $11$ posts.

# Generalization
Consider the integers $a$ and $b$ such that $b > a$. What is the length of the sequence $a, \dots, b$? That is, how many integers are there in this sequence?

The length of this sequence can be determined by finding a bijection between this sequence and the sequence $1,\dots,c$, where $c \in \mathbb N$ is the length of the sequence and $c > 1$, as follows:

$$
\begin{align}
&a,\dots,b \tag{1.1} \\
&0,\dots,(b-a) \tag{1.2} \\
&1,\dots,(b-a) + 1 \tag{1.3}
\end{align}
$$

where $(1.2)$ is obtained from $(1.1)$ by subtracting $a$ from each element in the sequence and $(1.3)$ is obtained from $(1.2)$ by adding $1$ to each element in the sequence. Hence, the length of the sequence $a,\dots,b$ is $(b-a) + 1$.

If we let $a = 7$ and $b = 373$, then we see that the length of the sequence $7, \dots, 373$ is $(373 - 7) + 1 = 367$ lines of code.

Next, suppose that you are trying to run a numerical simulation over time. You have the desired total simulation time $T > 0$ and a fixed time step size $\Delta t > 0$. Given this information, you would like to compute the points in time that the simulation will step through, denoted $n$.

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

# Summary
To correctly compute the correct number of objects and avoid fencepost errors, do the following:
1. Given the end-points of the sequence of objects, write down the full sequence of objects that you would like to count. Be sure to avoid including any objects that you do not want to count.
2. Transform the sequence from step 1 into the canonical sequence $1,\dots,c$, where $c \in \mathbb N$ and $c > 1$.
3. The desired number of objects is $c$.

Fencepost errors occur when the sequence that is written down in step 1 includes objects that should not be counted or does not include objects that should be counted.

For further reading on fencepost errors, see [this article](https://betterexplained.com/articles/learning-how-to-count-avoiding-the-fencepost-problem/).