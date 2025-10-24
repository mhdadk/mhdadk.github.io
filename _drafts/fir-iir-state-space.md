---
title: "FIR and IIR filters in state-space form"
layout: post
tags:
  - signal-processing
  - control-theory
---
We show how to express a finite impulse response (FIR) and an infinite impulse response
(IIR) filter in state-space form.

Of course, one may wonder why this is useful in the first place. The reason for this is
that controller design techniques can be applied to achieve some desired output signal
characteristics, which may not easily be available when the filter is expressed as a
standard transfer function.

## FIR

Given an input discrete-time signal $u : \{0,\dots,L-1\} \to \mathbb R$, where $L$ is the total number of samples considered, the output discrete-time signal $y : \{0,\dots,L-1\} \to \mathbb R$ of an FIR filter at the $k$th timestep, for $k = 0,\dots,L-1$, is

$$
\begin{align}
y[k] &= h[0] \cdot u[k] + h[1] \cdot u[k - 1] + \cdots + h[N-1] \cdot u[k - (N-1)] \nonumber \\
&= \sum_{i=0}^{N-1} h[i] \cdot u[k - i] \label{fir_out}
\end{align}
$$

where $N \leq L$ is the filter length (or number of *taps*) and $h[k]$ is the filter coefficient at the $k$th time-step. We convert $\eqref{fir_out}$ into state-space form as follows. Define the states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u[k-1] \\
x_2[k] &= u[k-2] \\
&\vdots \\
x_{N-1}[k] &= u[k-(N-1)]
\end{align*}
$$

Stack these states to form the state vector at the $k$th time-step,

$$
\begin{equation}
x[k] = \begin{bmatrix}u[k-1] \\ u[k-2] \\ \vdots \\ u[k - (N-1)]\end{bmatrix} \label{state_vec}
\end{equation}
$$

Then,

$$
\begin{align*}
x_1[k+1] &= u[k] \\
x_2[k+1] &= u[k-1] \\
&\vdots \\
x_{N-1}[k+1] &= u[k-(N-2))
\end{align*}
$$

Similarly, stack these together to form the state vector at the $(k+1)$th time-step such that,

$$
\begin{align}
x[k+1] &= \begin{bmatrix}u[k] \\ u[k-1] \\ \vdots \\ u[k-(N-2)]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}0 \\ u[k-1] \\ \vdots \\ u[k-(N-2)]\end{bmatrix} + \begin{bmatrix}u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\
&= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}u[k-1] \\ u[k-2] \\ \vdots \\ u[k - (N-1)]\end{bmatrix} + \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \nonumber \\
&= Ax[k] + Bu[k] \label{state_eq}
\end{align}
$$

where

* $\eqref{state_eq}$ is the state equation,
* $x[k]$ is defined in $\eqref{state_vec}$,
* $u[k]$ is the input signal at the $k$th time-step, and

$$
\begin{align*}
A &= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
B &= \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}
\end{align*}
$$

Furthermore, the output equation is

$$
\begin{align}
y[k] &= h[0] \cdot u[k] + h[1] \cdot u[k - 1] + \cdots + h[N-1] \cdot u[k - (N-1)] \\
&= h[1] \cdot u[k - 1] + \cdots + h[N-1] \cdot u[k - (N-1)] + h[0] \cdot u[k] \\
&= \begin{bmatrix}h[1] & \cdots & h[N-1]\end{bmatrix}\begin{bmatrix}u[k - 1] \\ \vdots \\ u[k - (N-1)]\end{bmatrix} + h[0] \cdot u[k] \\
&= Cx[k] + Du[k]
\end{align}
$$

where

$$
\begin{align*}
C &= \begin{bmatrix}h[1] & \cdots & h[N-1]\end{bmatrix} \\
D &= h[0]
\end{align*}
$$

To summarize, for an FIR filter, the state-space form is

$$
\begin{align*}
x[k+1] &= Ax[k] + Bu[k] \\
y[k] &= Cx[k] + Du[k]
\end{align*}
$$

where

$$
\begin{align*}
x[k] &= \begin{bmatrix}u[k-1] \\ u[k-2] \\ \vdots \\ u[k - (N-1)]\end{bmatrix} \\
A &= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
B &= \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix} \\
C &= \begin{bmatrix}h[1] & \cdots & h[N-1]\end{bmatrix} \\
D &= h[0]
\end{align*}
$$

It is interesting to note that the state vector $x[k]$ has a length of $N - 1$, where $N$ is the number of filter coefficients. This means that in real-time filtering applications, we would need to store the previous $N-1$ inputs as we receive the current input $u[k]$.

Additionally, the $A, B,$ and $C$ matrices closely resemble the [controllable canonical form](https://www.mathworks.com/help/control/ug/canonical-state-space-realizations.html#mw_2254b196-369f-4c4f-a44c-d348f5436df9) of a linear system.

## IIR

