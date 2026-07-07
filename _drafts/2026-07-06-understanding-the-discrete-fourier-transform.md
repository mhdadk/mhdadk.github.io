---
title: "Understanding the discrete Fourier transform"
layout: post
excerpt: "We explore the quirks of the discrete Fourier transform"
tags:
  - signal-processing
---

Consider the continuous-time signal
$x(t) = 5 \sin(10 \pi t) + 15 \sin(30 \pi t)$ for $t \geq 0$. Let's sample this
signal at $100$ Hz such that $t = n / 100$ for $n \in \mathbb N \cup \{0\}$ and
$x(n) = 5 \sin(0.1\pi n) + 15 \sin(0.3\pi n)$. This means that $x(n)$ contains
the normalized angular frequencies of $0.1\pi$ and $0.3\pi$ radians/sample.

We should then expect that a plot of the magnitude spectrum of this signal to
show peaks of 5 and 15 at $0.1\pi$ and $0.3\pi$ respectively. Let's plot this
magnitude spectrum in MATLAB using the code below.

```matlab

```
