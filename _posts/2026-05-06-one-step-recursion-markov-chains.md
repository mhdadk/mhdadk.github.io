---
title: "One-step recursion in Markov chains"
layout: post
excerpt: "How to intuitively solve Markov chain expectation problems, illustrated using a classic coin-flipping puzzle."
tags:
  - probability
  - puzzle
---

Here's a fun question. You're flipping a fair coin and you stop the moment
you see two heads in a row. Let $N$ be the number of flips that takes. On
average, how big is $N$? In other words, what is $E[N]$?

In a [previous post](/blog/expected-hitting-and-return-times-on-a-square.html),
we computed the mean hitting time of a state in a Markov chain by conditioning
on the first transition $X_1$, a trick we'll call _one-step recursion_. In
this post, we'll apply the same trick to compute $E[N]$, following the
alternative solution given in
[this Math StackExchange answer](https://math.stackexchange.com/a/364257).
We'll work with a general probability $p$ of heads, and only at the end
plug in $p = 1/2$ for the fair-coin case.

There's one big difference in style between this post and the previous one.
In the previous post, we were careful to justify each step rigorously, naming
the law of total expectation, the Markov property, time-homogeneity, and so
on. This post leans much more heavily on _intuition_.

We won't write the chain $\mathcal{X} = \lbrace X_0, X_1, \dots\rbrace$ out
explicitly, and we won't invoke the law of total expectation or the Markov
property by name. Instead, we'll just reason directly about what conditional
expectations like $E[N \mid HT]$ "should" equal, leaning on the simple
observation that a tail wipes out any in-progress streak.

Once we have the answer, we'll go back and look at the problem through the
rigorous Markov chain lens. We'll see that the intuitive manipulations were
really the same mean hitting time recursion from the previous post in
disguise.

Why bother working intuitively first? Terry Tao has a wonderful short essay,
[_There's more to mathematics than rigour and proofs_](https://terrytao.wordpress.com/career-advice/theres-more-to-mathematics-than-rigour-and-proofs/),
where he describes mathematical maturity as a three-stage progression.

First comes a "pre-rigorous" stage, where you work by intuition and informal
manipulation. Then a "rigorous" stage, where you learn to be careful and
precise. And finally a "post-rigorous" stage, where intuition comes back, but
now you trust it because you know any informal step _could_ be made rigorous
if you had to.

Both intuition and rigour matter, and neither replaces the other. Intuition
is what lets you guess the right identity, pick a productive way to
condition, or notice that two problems are really the same problem in
disguise. Rigour is what lets you trust the answer once you have it. The
previous post was an exercise in rigour; this one is an exercise in
intuition.

## Setup and notation

Let $p \in (0, 1)$ be the probability of heads on a single flip. We want
$E[N]$, the expected number of flips until we see two heads in a row.

To set up the one-step recursion, we'll introduce a few conditional
expectations, each one indexed by the prefix of flips we've already observed:

* $E[N \mid H]$: the expected number of flips, given that the first flip has
already happened and was heads.
* $E[N \mid T]$: same idea, but the first flip was tails.
* $E[N \mid HH]$: the expected number of flips, given that the first two
flips have already happened and were both heads.
* $E[N \mid HT]$: the expected number of flips, given that the first flip
was heads and the second was tails.

Here's the key insight: a few of these conditional expectations collapse to
something we already know.

$$
\begin{align}
E[N \mid HH] &= 0 \label{ENHH} \\
E[N \mid HT] &= E[N] \label{ENHT} \\
E[N \mid T] &= E[N] \label{ENT}
\end{align}
$$

Why?

* $\eqref{ENHH}$ is easy: once we've seen $HH$, we're done. No more flips
needed.
* $\eqref{ENHT}$ and $\eqref{ENT}$ are the interesting ones. The moment we
flip a tail, any in-progress streak of heads is gone, and we're effectively
back at the start. Coin flips are independent, so the expected number of
_additional_ flips after a tail is the same as the expected number of flips
starting from scratch, which is just $E[N]$.

## One-step recursion

Now we condition on the first flip. With probability $p$ it's heads, and
with probability $1-p$ it's tails:

$$
E[N] = 1 + p \cdot E[N \mid H] + (1-p) \cdot E[N \mid T] \label{EN-1}
$$

The leading $1$ counts the first flip itself, and the rest counts the
expected number of _additional_ flips after that first flip.

We still need $E[N \mid H]$, so we condition again, this time on the second
flip:

$$
E[N \mid H] = 1 + p \cdot E[N \mid HH] + (1-p) \cdot E[N \mid HT] \label{ENH-1}
$$

Plugging $\eqref{ENHH}$ and $\eqref{ENHT}$ into $\eqref{ENH-1}$,

$$
E[N \mid H] = 1 + (1-p) \cdot E[N] \label{ENH-2}
$$

and plugging $\eqref{ENH-2}$ and $\eqref{ENT}$ into $\eqref{EN-1}$,

$$
E[N] = 1 + p \cdot \left[1 + (1-p) \cdot E[N]\right] + (1-p) \cdot E[N] \label{EN-2}
$$

That's one linear equation in one unknown, $E[N]$, which we'll solve in a
moment.

## Markov chain interpretation

Equations $\eqref{EN-1}$ and $\eqref{ENH-1}$ aren't coincidences. They are
mean hitting time recursions on a 3-state Markov chain. The states are:

* $S_0$: "no head pending"; either we haven't flipped yet, or the most
recent flip was a tail.
* $S_1$: "one head pending"; the most recent flip was a head, but we
haven't seen two in a row yet.
* $S_2$: "two heads in a row"; absorbing.

The transitions are:

* From $S_0$: go to $S_1$ with probability $p$, stay at $S_0$ with
probability $1-p$.
* From $S_1$: go to $S_2$ with probability $p$, go back to $S_0$ with
probability $1-p$.
* From $S_2$: stay at $S_2$ with probability $1$.

{% include figure.html
   filename="one-step-recursion-markov-chains/mc-two-heads.svg"
   caption="Discrete-time MC for the two-consecutive-heads problem. State $S_2$ is absorbing."
   fignum=1
   scale=50
%}

Using the $\tau_{ij}$ notation from the previous post, the mean hitting
times of $S_2$ from $S_0$ and $S_1$ are

$$
\begin{align*}
E[N] &= \tau_{S_0 S_2} \\
E[N \mid H] &= \tau_{S_1 S_2}
\end{align*}
$$

and equations $\eqref{EN-1}$ and $\eqref{ENH-1}$ are exactly the mean hitting
time recursion $\tau_{ij} = 1 + \sum_s p_{is} \cdot \tau_{sj}$ (with
$\tau_{S_2 S_2} = 0$) instantiated at $i = S_0$ and $i = S_1$. The
"memoryless restart" identities $E[N \mid T] = E[N]$ and $E[N \mid HT] = E[N]$
are just the chain's transitions back into $S_0$.

## Solving for $E[N]$

Expanding $\eqref{EN-2}$,

$$
\begin{align*}
E[N] &= 1 + p + p(1-p) \cdot E[N] + (1-p) \cdot E[N] \\
E[N] \cdot \left[1 - p(1-p) - (1-p)\right] &= 1 + p \\
E[N] \cdot \left[1 - (1-p)(p + 1)\right] &= 1 + p \\
E[N] \cdot p^2 &= 1 + p
\end{align*}
$$

so

$$
\boxed{E[N] = \frac{1 + p}{p^2}}
$$

Specializing to a fair coin, $p = 1/2$,

$$
E[N] = \frac{3/2}{1/4} = 6
$$

So on average, you need $6$ flips of a fair coin to see two heads in a row.

## Conclusion

The same one-step recursion that handled the random walk on a square in the
previous post handles this coin-flipping problem too. The only thing that
changes is the underlying Markov chain. A 4-state walk on a square gets
replaced by a 3-state absorbing chain that tracks the current head streak.

The closed form $E[N] = (1 + p)/p^2$ also generalizes nicely. If you want
$k$ heads in a row instead of $2$, you get a $(k+1)$-state absorbing chain,
and the same one-step recursion gives you a closed-form mean hitting time
of the absorbing state.
