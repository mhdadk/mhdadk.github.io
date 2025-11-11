---
title: "State space representation of IIR filters"
layout: post
tags:
  - signal-processing
  - control-theory
---
We show how to express a discrete-time infinite impulse response (IIR) filter in state-space form.

Linear and time-invariant (LTI) filters are not often expressed in state-space form since in filtering applications, we have control over the filter and not the input signal, while in control applications, the opposite is true. Nonetheless, expressing LTI filters in state-space form offers another way of analyzing the filter and bridges the fields of signal processing and control theory.

Recall from a [previous post]({% post_url 2025-10-25-iir-direct-forms %}) that an IIR filter can be implemented using a Direct Form I (DF1) or Direct Form II (DF2) structure, among other implementations. We will derive the state-space representation for both structures.

## Direct Form I

The difference equations that describe a DF1 IIR filter are

$$
\begin{align}
u_a[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] \label{iir_df1_in} \\
y[k] &= u_a[k] + \sum_{j = 1}^M -a_j \cdot y[k - j] \label{iir_df1_out}
\end{align}
$$

where $b_0,\dots,b_{N-1}$ are the $N$ feedforward coefficients, $a_1,\dots,a_M$ are the $M$ feedback coefficients, and at the $k$th time-step,

* $u[k]$ is the input to the IIR filter,
* $u_a[k]$ is an intermediate variable, and
* $y[k]$ is the output of the IIR filter.

Looking at $\eqref{iir_df1_in}$, define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u[k-1] \\
x_2[k] &= u[k-2] \\
&\vdots \\
x_{N-1}[k] &= u[k-(N-1)]
\end{align*}
$$

Next, looking at $\eqref{iir_df1_out}$, define the $M$ states $z_1[k],\dots,z_M[k]$ at the $k$th time-step as

$$
\begin{align*}
z_{1}[k] &= y[k-1] \\
&\vdots \\
z_{M}[k] &= y[k-M] \\
\end{align*}
$$

Substituting $\eqref{iir_df1_in}$ into $\eqref{iir_df1_out}$ and then substituting the aforementioned states into the resulting equation yields

$$
\begin{equation*}
y[k] = b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] + \sum_{j = 1}^M -a_j \cdot z_j[k]
\end{equation*}
$$

We can stack both sets of states to form the state vectors

$$
\begin{align*}
x[k] &= \begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} \\
z[k] &= \begin{bmatrix}z_1[k] \\ \vdots \\ z_{M}[k]\end{bmatrix}
\end{align*}
$$

Then, the state update equations are

$$
\begin{align}
x[k+1] &= \begin{bmatrix}u[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k]\end{bmatrix} \nonumber \\

&= \begin{bmatrix}0 \\ x_1[k] \\ \vdots \\ x_{N-2}[k]\end{bmatrix} + \begin{bmatrix}u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\

&= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} + \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \nonumber \\

&= A_x x[k] + B_x u[k] \label{x_state_eq} \\

z[k+1] &= \begin{bmatrix}y[k] \\ z_1[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} \nonumber \\

&= \begin{bmatrix}b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] + \sum_{j = 1}^M -a_j \cdot z_j[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} \nonumber \\

&= \begin{bmatrix}\sum_{j = 1}^M -a_j \cdot z_j[k] \\ z_1[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} +
\begin{bmatrix}\sum_{i=1}^{N-1} b_i \cdot x_i[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} +
\begin{bmatrix}b_0u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\

&= \begin{bmatrix}
-a_1 & -a_2 & -a_3 & \cdots & -a_M \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}z_1[k] \\ \vdots \\ z_{M}[k]\end{bmatrix} +
\begin{bmatrix}b_1 & b_2 & \cdots & b_{N-1} \\ 0 & 0 & \cdots & 0 \\ \vdots & \vdots & \vdots & 0 \\ 0 & 0 & \cdots & 0\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} +
\begin{bmatrix}b_0 \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \nonumber \\

&= A_zz[k] + A_{zx}x[k] + B_zu[k] \label{z_state_eq}
\end{align}
$$

where

$$
\begin{align*}
A_x &= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
B_x &= \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix} \\
A_z &= \begin{bmatrix}
-a_1 & -a_2 & -a_3 & \cdots & -a_M \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
A_{zx} &= \begin{bmatrix}b_1 & b_2 & \cdots & b_{N-1} \\ 0 & 0 & \cdots & 0 \\ \vdots & \vdots & \vdots & 0 \\ 0 & 0 & \cdots & 0\end{bmatrix} \\
B_z &= \begin{bmatrix}b_0 \\ 0 \\ \vdots \\ 0\end{bmatrix}
\end{align*}
$$

We can stack $\eqref{x_state_eq}$ and $\eqref{z_state_eq}$ to form the combined state update equation

$$
\begin{equation}
\begin{bmatrix}x[k+1] \\ z[k+1] \end{bmatrix} = \begin{bmatrix}A_x & 0 \\ A_{zx} & A_z \end{bmatrix}\begin{bmatrix}x[k] \\ z[k] \end{bmatrix} + \begin{bmatrix}B_x \\ B_z \end{bmatrix}u[k] \label{xz_state_eq}
\end{equation}
$$

Notice that in $\eqref{xz_state_eq}$, the state vector $x$ evolves independently of the state vector $z$, while the state vector $z$ evolves as a function of both $x$ and $z$. Hence, the system represented by $\eqref{xz_state_eq}$ can be split into two subsystems: the first subsystem takes in $u[k]$ as an input and outputs $x[k]$, while the second subsystem takes in both $x[k]$ (the output of the first subsystem) and $u[k]$ as inputs and outputs $z[k]$.

Additionally, the state transition matrix in $\eqref{xz_state_eq}$ is block lower-triangular, so its eigenvalues are the eigenvalues of the diagonal blocks, $A_x$ and $A_z$. Because $A_x$ is a lower triangular matrix with zeros on its diagonal, then its eigenvalues are all zero. Hence, we are only interested in the eigenvalues of $A_z$.

Of course, in practice, we do not have control over the input $u[k]$, so this control-theoretic analysis can be limited.

## Direct Form II

**TODO: finish the derivation**
