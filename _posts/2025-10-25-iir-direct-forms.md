---
title: "Direct Form I and Direct Form II"
layout: post
excerpt: "We derive the Direct Form I and Direct Form II implementations of a discrete-time infinite impulse response (IIR) filter."
tags:
  - signal-processing
---
We derive the Direct Form I and Direct Form II implementations of a discrete-time infinite impulse response (IIR) filter.

When I first encouuntered the Direct Form I (DF1) and Direct Form II (DF2) structures in a signal processing course, I could not find a derivation for where they came from. Moreover, if I did find a derivation, it would often be in graphical form without a mathematical derivation to accompany it.

The purpose of this post is to provide a clear and comprehensive derivation of these two implementations.

## Preliminaries

Consider the transfer function $H(z)$ of a general discrete-time IIR filter, such that

$$
\begin{equation}
H(z) = \frac{Y(z)}{U(z)} = \frac{b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)}}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \label{iir_tf}
\end{equation}
$$

Note that $H(z)$ consists of an all-zero IIR filter (i.e., an FIR filter) cascaded with an all-pole IIR filter, such that

$$
\begin{align*}
H(z) &= \frac{b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)}}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \\
&= \left(b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)}\right) \cdot \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \\
&= H_b(z) \cdot H_a(z)
\end{align*}
$$

where

$$
\begin{align}
H_b(z) &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \label{iir_allzero} \\
H_a(z) &= \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \label{iir_allpole}
\end{align}
$$

We now map $H_b(z)$ and $H_a(z)$ to their corresponding difference equations in the time domain as follows.

$$
\begin{align}
H_b(z) = \frac{Y_b(z)}{U_b(z)} &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \nonumber \\

Y_b(z) &= b_0U_b(z) + b_1U_b(z)z^{-1} + \cdots + b_{N-1}U_b(z)z^{-(N-1)} \nonumber \\

y_b[k] &= b_0u_b[k] + b_1u_b[k-1] + \cdots + b_{N-1}u_b[k-(N-1)] \nonumber \\

&= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_allzero_out} \\

H_a(z) = \frac{Y_a(z)}{U_a(z)} &= \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \nonumber \\

Y_a(z) + a_1Y_a(z)z^{-1} + \cdots + a_MY_a(z)z^{-M} &= U_a(z) \nonumber \\

y_a[k] + a_1y_a[k-1] + \cdots + a_My_a[k-M] &= u_a[k] \nonumber \\

y_a[k] &= u_a[k] - a_1y_a[k-1] - \cdots - a_My_a[k-M] \nonumber \\

&= u_a[k] + \sum_{j = 1}^M -a_j \cdot y_a[k - j] \label{iir_allpole_out}
\end{align}
$$

where $u_b[k], y_b[k],$ and $b_0,\dots,b_{N-1}$ are the input, output, and $N$ feedforward coefficients, respectively, associated with the all-zero IIR filter and $u_a[k], y_a[k],$ and $a_1,\dots,a_M$ are the input, output, and $M$ feedback coefficients, respectively, associated with the all-pole IIR filter.

To summarize, the input $u_b[k]$ and output $y_b[k]$ of the all-zero IIR filter $H_b(z)$ are related by the difference equation,

$$
\begin{equation}
y_b[k] = b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_allzero_out2}
\end{equation}
$$

and the input $u_a[k]$ and output $y_a[k]$ of the all-pole IIR filter $H_a(z)$ are related by the difference equation,

$$
\begin{equation}
y_a[k] = u_a[k] + \sum_{j = 1}^M -a_j \cdot y_a[k - j] \label{iir_allpole_out2}
\end{equation}
$$

## Direct Form I

In a DF1 implementation of an IIR filter, the input $u[k]$ is passed into the all-zero filter $H_b(z)$ first and the output of the all-zero filter is input to the all-pole filter $H_a(z)$ to produce the output $y[k]$. In other words, $u_b[k] = u[k]$, $y_b[k] = u_a[k]$, and $y_a[k] = y[k]$ for each $k$ such that

$$
\begin{align}
u_a[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] \label{iir_df1_in} \\
y[k] &= u_a[k] + \sum_{j = 1}^M -a_j \cdot y[k - j] \label{iir_df1_out}
\end{align}
$$

Note that the terms $u[k - 1],\dots,u[k-(N-1)]$ and $y[k - 1],\dots,y[k-M]$ are obtained from memory from a previous iteration. Hence, the DF1 structure requires memory for $N - 1 + M$ floating-point numbers.

## Direct Form II

In a DF2 implementation of an IIR filter, the input $u[k]$ is passed into the all-pole filter $H_a(z)$ first and the output of the all-pole filter is input to the all-zero filter $H_b(z)$ to produce the output $y[k]$.

In contrast to the DF1 structure, the order of the all-pole and all-zero IIR filters are reversed. Moreover, $u_a[k] = u[k]$, $y_a[k] = u_b[k]$, and $y_b[k] = y[k]$ for each $k$ such that

$$
\begin{align}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot u_b[k - j] \label{iir_df2_in} \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_df2_out}
\end{align}
$$

Note that the terms $u_b[k - 1],\dots,u_b[k-M],\dots,u_b[k-(N-1)]$ are obtained from memory from a previous iteration.

In contrast to the DF1 structure, which requires memory for $N - 1 + M$ floating-point numbers, the DF2 structure requires memory for only $\max(N-1, M)$ floating-point numbers. Because $\max(N-1, M) < N - 1 + M$ when $N - 1 > 0$ and $M > 0$, then the DF2 structure requires less memory than the DF1 structure.
