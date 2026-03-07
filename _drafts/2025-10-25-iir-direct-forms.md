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

## Transposed Direct Form I

Because the outputs of the all-zero and all-pole filters in the previous sections are
polynomial functions of the input and past outputs, we can use [Horner's method](https://en.wikipedia.org/wiki/Horner%27s_method) to speed up the evaluation of this polynomial function. This is exactly
what the Transposed Direct Forms I and II accomplish: they reduce the number of multiplications,
additions, and updates of temporary variables.

Recall from $\eqref{iir_allzero_out}$ that the output of the all-zero filter $H_b(z)$ is
defined as

$$
\begin{align*}
H_b(z) = \frac{Y_b(z)}{U_b(z)} &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \\

Y_b(z) &= U_b(z)\left(b_0 + \cdots + b_{N-3}z^{-(N-3)} + b_{N-2}z^{-(N-2)} + b_{N-1}z^{-(N-1)}\right)

\end{align*}
$$

$Y_b(z)$ can be evaluated recursively backwards as follows.

$$
\begin{align*}
Y_b(z) &= U_b(z)V_0(z) \\

V_0(z) &= b_0 + b_1z^{-1} + b_2z^{-2} + b_3z^{-3} + \cdots + b_{N-1}z^{-(N-1)} \\

&= b_0 + z^{-1}\left(b_1 + b_2z^{-1} + b_3z^{-2} + \cdots + b_{N-1}z^{-(N-2)}\right) \\

&= b_0 + z^{-1}V_1(z) \\

V_1(z) &= b_1 + b_2z^{-1} + b_3z^{-2} + \cdots + b_{N-1}z^{-(N-2)} \\

&= b_1 + z^{-1}\left(b_2 + b_3z^{-1} + \cdots + b_{N-1}z^{-(N-3)}\right) \\

&= b_1 + z^{-1}V_2(z) \\

V_2(z) &= b_2 + b_3z^{-1} + \cdots + b_{N-1}z^{-(N-3)} \\

&= b_2 + z^{-1}\left(b_3 + \cdots + b_{N-1}z^{-(N-4)}\right) \\

&= b_2 + z^{-1}V_3(z) \\

&\vdots

\end{align*}
$$

More generally,

$$
\begin{align*}
Y_b(z) &= U_b(z)V_0(z) \\
V_\ell(z) &= \begin{cases}b_{N-1},&\ell = N-1 \\ b_{\ell} + z^{-1}V_{\ell+1}(z),&\ell = N-2,N-3,\dots,0\end{cases}
\end{align*}
$$

and the corresponding difference equations in the time domain are

$$
\begin{align*}
y_b[k] &= (u_b * v_0)[k] \\
&= \sum_{i=1}^Q u_b[k-i]v_0[i] \\
v_\ell[k] &= \begin{cases}b_{N-1},&\ell = N-1 \\ b_\ell + v_{\ell+1}[k-1],&\ell = N-2,N-3,\dots,0\end{cases}
\end{align*}
$$

Similarly, recall from $\eqref{iir_allpole_out}$ that the output of the all-pole filter
$H_a(z)$ is defined as

$$
\begin{align*}
H_a(z) = \frac{Y_a(z)}{U_a(z)} &= \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \\

Y_a(z) + a_1Y_a(z)z^{-1} + \cdots + a_MY_a(z)z^{-M} &= U_a(z) \\

Y_a(z) &= U_a(z) + Y_a(z)\left(-a_1z^{-1} - \cdots - a_Mz^{-M}\right)
\end{align*}
$$

$Y_a(z)$ can be evaluated recursively backwards as follows.

$$
\begin{align*}
Y_a(z) &= U_a(z) + Y_a(z)\left(-a_1z^{-1} - a_2z^{-2} - a_3z^{-3} - \cdots - a_Mz^{-M}\right) \\

&= U_a(z) + z^{-1}Y_a(z)\left(-a_1 - a_2z^{-1} - a_3z^{-2} - \cdots - a_Mz^{-(M-1)}\right) \\

&= U_a(z) + z^{-1}Y_a(z)W_1(z) \\

W_1(z) &= -a_1 - a_2z^{-1} - a_3z^{-2} - \cdots - a_Mz^{-(M-1)} \\

&= -a_1 + z^{-1}\left(- a_2 - a_3z^{-1} - \cdots - a_Mz^{-(M-2)}\right) \\

&= -a_1 + z^{-1}W_2(z) \\

W_2(z) &= - a_2 - a_3z^{-1} - \cdots - a_Mz^{-(M-2)} \\

&= - a_2 + z^{-1}\left(- a_3 - \cdots - a_Mz^{-(M-3)}\right) \\

&= -a_2 + z^{-1}W_3(z) \\

&\vdots
\end{align*}
$$

More generally,

$$
\begin{align*}
Y_a(z) &= U_a(z) + z^{-1}Y_a(z)W_1(z) \\
W_\ell(z) &= \begin{cases}-a_{M},&\ell = M \\ -a_\ell + z^{-1}W_{\ell+1}(z),&\ell = M-1,M-2,\dots,1\end{cases}
\end{align*}
$$

and the corresponding difference equations in the time domain are

$$
\begin{align*}
y_a[k] &= u_a[k] + (y_a * w_1)[k-1] \\
&= u_a[k] + \sum_{i=1}^Q y_a[k-1-i]w_1[i] \\
w_\ell[k] &= \begin{cases}-a_{M},&\ell = M \\ -a_\ell + w_{\ell+1}[k-1],&\ell = M-1,M-2,\dots,1\end{cases}
\end{align*}
$$

**TODO**: combine the all-pole and all-zero filter outputs and explain how the computational
savings happen.

## Transposed Direct Form II
