---
title: "On the time complexity of computing the $n$th Fibonnaci number"
layout: post
excerpt: "We derive an interesting relationship between the number of additions required to compute the $n$th Fibonnaci number and the golden ratio $\varphi$."
tags:
  - algorithms
  - puzzle
---
A friend recently asked me the following question: "What is the time complexity of computing the $n$th Fibonnaci number without caching?" Although this question may seem arbitrary at first, its solution motivates the need for a closed-form expression for the $n$th term in a Fibonacci sequence and the interesting relationship between this closed-form expression and the golden ratio $\varphi$.

## Preliminaries

First, recall that the $n$th number in the Fibonnaci sequence $F_n : \mathbb N \to \mathbb N$ is

$$
\begin{equation}
F_n =
\begin{cases}
1, &n = 1 \\
1, &n = 2 \\
F_{n-1} + F_{n-2}, &n > 2
\end{cases} \label{fib-seq}
\end{equation}
$$

In Python, we would compute $F_n$ as follows:

```python
def fib(n):
    if n == 1:
        return 1
    elif n == 2:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```

Evaluating `fib(100)` takes several minutes on my PC, and if I try to evaluate `fib(1200)`, I get a `RecursionError: maximum recursion depth exceeded`, which suggests a stack overflow. Clearly, computing $F_n$ requires a non-trivial amount of computations.

## Number of additions

The only operation required to compute $F_n$ is addition, so determining the time complexity of computing $F_n$ requires determining how many additions are needed to compute $F_n$.

Notice that to compute $F_n$, we decompose it into exactly $F_n$ ones and then sum them together. For example, $F_5 = 5$ is decomposed as

$$
\begin{align*}
F_5 &= F_4 + F_3 \\
&= (F_3 + F_2) + (F_2 + F_1) \\
&= ((F_2 + F_1) + F_2) + (F_2 + F_1) \\
&= ((1 + 1) + 1) + (1 + 1)
\end{align*}
$$

If $F_n$ is the sum of $F_n$ terms, then $F_n - 1$ additions are required to compute $F_n$. Interestingly, the number of additions required to compute $F_n$ is itself a Fibonacci sequence!

We could stop here, but this does not answer our original question. Specifically, although we know that $F_n - 1$ additions are required to compute $F_n$, we don't know how $F_n$ is a function of $n$. In other words, we don't know a closed-form expression for $F_n$, so we can't conclude what the time complexity of computing $F_n$ is. Can we derive one?

## A closed-form expression for $F_n$

There are many ways to derive a closed-form expression for $F_n$. My favorite way is using ordinary generating functions (OGFs). An OGF allows us to convert a linear recurrencce relation, like the one given in $\eqref{fib-seq}$, into an algebraic equation. OGFs are to linear recurrence relations what the Laplace Transform is to linear ordinary differential equations (ODEs).

Once this algebraic equation is manipulated into a certain form, its inverse is identified to obtain the closed-form expression, as done for the Laplace Transform of linear ODEs.

One of the most famous examples of OGFs is the [Z-transform](https://en.wikipedia.org/wiki/Z-transform). Given a sequence $x_n$ for $n \in \mathbb N$, the Z-transform of $x_n$ is defined as

$$
X(z) = \sum_{n = 1}^\infty x_n z^{-n}
$$

Hence, to determine a closed-form expression for $F_n$, we compute its Z-transform, $F(z)$, re-arrange terms into a specific form, and then identify the inverse Z-transform to obtain the closed-form expression. For convenience, we will let $F_0 = 0$ to be able to use the one-sided Z-transform.

Recall from $\eqref{fib-seq}$ that for $n > 2$, $F_n = F_{n-1} + F_{n-2}$ and the initial conditions are $F_0 = 0$ and $F_1 = F_2 = 1$. Computing the one-sided Z-transform of both sides of this recurrence relation yields

$$
\begin{align}
F(z) &= z^{-1}F(z) + F_0 + z^{-2}F(z) + z^{-1}F_0 + F_1 \nonumber \\
F(z) - z^{-1}F(z) - z^{-2}F(z) &= F_0 + z^{-1}F_0 + F_1 \nonumber \\
F(z)(1 - z^{-1} - z^{-2}) &= 1 \nonumber \\
F(z) &= \frac{1}{1 - z^{-1} - z^{-2}} \nonumber \\
&= \frac{z^2}{z^2 - z - 1} \label{fib-z}
\end{align}
$$

We now want to decompose $\eqref{fib-z}$ into a sum of fractions using a partial fractions decomposition.

<details>
  <summary>Full derivation of the partial fraction decomposition of $F(z)$</summary>
  We first factorize the denominator in $\eqref{fib-z}$ such that $z^2 - z - 1 = (z-\varphi)(z-\psi)$, where $\varphi = \frac{1 + \sqrt{5}}{2}$ is the golden ratio and $\psi = \frac{1 - \sqrt{5}}{2}$ is its conjugate, such that $\varphi - \psi = \sqrt 5$. Then,

  $$
  F(z)=\frac{z^2}{(z-\varphi)(z-\psi)}
  $$

  Because the numerator and denominator both have degree 2, we set up the decomposition as

  $$
  \frac{z^2}{(z-\varphi)(z-\psi)} = \frac{A z}{z-\varphi}+\frac{B z}{z-\psi}
  $$

  We then solve for $A$ and $B$ by multiplying through by the denominator to obtain:

  $$
  z^2 = A z (z-\psi)+B z (z-\varphi)
  $$

  Setting $z = \psi$, we get

  $$
  \begin{align*}
  B &= \frac{\psi}{\psi - \varphi} \\
    &= -\frac{1}{\sqrt 5}
  \end{align*}
  $$

  Setting $z = \varphi$, we get

  $$
  \begin{align*}
  A &= \frac{\varphi}{\varphi - \psi} \\
    &= \frac{1}{\sqrt 5}
  \end{align*}
  $$

  So,

  $$
  F(z) = \frac{1}{\sqrt5}\left(\frac{z}{z-\varphi}-\frac{z}{z-\psi}\right)
  $$
</details>

It can be shown that

$$
\begin{equation}
F(z) = \frac{1}{\sqrt5}\left(\frac{z}{z-\varphi}-\frac{z}{z-\psi}\right) \label{fib-z-trans}
\end{equation}
$$

where

$$
\varphi = \frac{1 + \sqrt{5}}{2}, \quad \psi = \frac{1 - \sqrt{5}}{2}
$$

are the golden ratio and its conjugate, respectively. By inspection of $\eqref{fib-z-trans}$, we can deduce that the inverse Z-transform of this expression is

$$
\begin{equation}
F_n = \frac{1}{\sqrt5}\left(\varphi^n - \psi^n\right) \label{binet-formula}
\end{equation}
$$

which is the closed-form expression of $F_n$ that we were looking for. $\eqref{binet-formula}$ is also known as _Binet's formula_.

## Conclusion

Because $\varphi > \psi$, then $F_n = O(\varphi^n)$. To summarize, the number of additions
required to compute the $n$th Fibonnaci term grows exponentially at a rate of the golden
ratio $\varphi$.

Personally, I found this problem interesting for several reasons:

1. We discovered that the number of additions required to compute $F_n$ is itself a
Fibonnaci sequence.
2. Deriving the closed-form expression for $F_n$ involved solving a linear recurrence
relation using OGFs, specifically the Z-transform.
3. We found that $F_n$ can be computed as a function of the golden ratio $\varphi$ and its
conjugate $\psi$.
