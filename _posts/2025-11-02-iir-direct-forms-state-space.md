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

Looking at ..., define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

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
\begin{align}
A_x &= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \nonumber \\
B_x &= \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\
A_z &= \begin{bmatrix}
-a_1 & -a_2 & -a_3 & \cdots & -a_M \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \label{A_z} \\
A_{zx} &= \begin{bmatrix}b_1 & b_2 & \cdots & b_{N-1} \\ 0 & 0 & \cdots & 0 \\ \vdots & \vdots & \vdots & 0 \\ 0 & 0 & \cdots & 0\end{bmatrix} \nonumber \\
B_z &= \begin{bmatrix}b_0 \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber
\end{align}
$$

We can stack $\eqref{x_state_eq}$ and $\eqref{z_state_eq}$ to form the combined state update equation

$$
\begin{equation}
\begin{bmatrix}x[k+1] \\ z[k+1] \end{bmatrix} = \begin{bmatrix}A_x & 0 \\ A_{zx} & A_z \end{bmatrix}\begin{bmatrix}x[k] \\ z[k] \end{bmatrix} + \begin{bmatrix}B_x \\ B_z \end{bmatrix}u[k] \label{xz_state_eq}
\end{equation}
$$

Finally, the output equation is

$$
\begin{align}
y[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] + \sum_{j = 1}^M -a_j \cdot z_j[k] \nonumber \\
&= \begin{bmatrix}-a_1 & \cdots & -a_M\end{bmatrix}\begin{bmatrix}z_1[k] \\ \vdots \\ z_M[k]\end{bmatrix} + \begin{bmatrix}b_1 & \cdots & b_{N-1}\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} + b_0u[k] \nonumber \\
&= C_zz[k] + C_xx[k] + Du[k] \label{output_eq_iir_df1} \\
&= \begin{bmatrix}C_z & C_x\end{bmatrix}\begin{bmatrix}x[k] \\ z[k] \end{bmatrix} + Du[k] \nonumber
\end{align}
$$

where

$$
\begin{align*}
C_z &= \begin{bmatrix}-a_1 & \cdots & -a_M\end{bmatrix} \\
C_x &= \begin{bmatrix}b_1 & \cdots & b_{N-1}\end{bmatrix} \\
D &= b_0
\end{align*}
$$

Notice that in $\eqref{xz_state_eq}$, the state vector $x$ evolves independently of the state vector $z$, while the state vector $z$ evolves as a function of both $x$ and $z$. Hence, the system represented by $\eqref{xz_state_eq}$ can be split into two subsystems: the first subsystem takes in $u[k]$ as an input and outputs $x[k]$, while the second subsystem takes in both $x[k]$ (the output of the first subsystem) and $u[k]$ as inputs and outputs $z[k]$. $x[k]$ and $z[k]$ are then output via $\eqref{output_eq_iir_df1}$.

Additionally, the state transition matrix in $\eqref{xz_state_eq}$ is block lower-triangular, so its eigenvalues are the eigenvalues of the diagonal blocks, $A_x$ and $A_z$. Because $A_x$ is a lower triangular matrix with zeros on its diagonal, then its eigenvalues are all zero. Hence, we are only interested in the eigenvalues of $A_z$.

Of course, in signal processing applications, we do not have control over the input $u[k]$, so this control-theoretic analysis can be limited.

## Direct Form II

The difference equations that describe a DF2 IIR filter are

$$
\begin{align}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot u_b[k - j] \label{iir_df2_in} \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_df2_out}
\end{align}
$$

where $u_b[k]$ is an intermediate variable at the $k$th time-step and all other terms are defined as before.

Without loss of generality, suppose that $M > N - 1$. Then, define the $M$ states $x_1[k],x_2[k],\dots,x_{N-1}[k],\dots,x_{M}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u_b[k-1] \\
x_2[k] &= u_b[k-2] \\
&\vdots \\
x_{N-1}[k] &= u_b[k - (N-1)] \\
&\vdots \\
x_{M}[k] &= u_b[k - M]
\end{align*}
$$

Substituting these states into $\eqref{iir_df2_in}$ and $\eqref{iir_df2_out}$ yields

$$
\begin{align*}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot x_j[k] \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k]
\end{align*}
$$

We can also stack these states to form the state vector

$$
x[k] = \begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} \\
$$

such that the state update equation is

$$
\begin{align}
x[k+1] &= \begin{bmatrix}u_b[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ \vdots \\ x_{M-1}[k]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}u[k] + \sum_{j = 1}^M -a_j \cdot x_j[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ \vdots \\ x_{M-1}[k]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}\sum_{j = 1}^M -a_j \cdot x_j[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ \vdots \\ x_{M-1}[k]\end{bmatrix} + \begin{bmatrix}u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\
&= \begin{bmatrix}
-a_1 & -a_2 & -a_3 & \cdots & -a_M \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} + \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \nonumber \\
&= A_zx[k] + B_ru[k]
\end{align}
$$

where $A_z$ is defined in $\eqref{A_z}$ and $B_r$ is an $M$-dimensional column vector with $1$ at the top and zeros everywhere else.

Finally, the output equation is

$$
\begin{align*}
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0\left[u[k] + \sum_{j = 1}^M -a_j \cdot x_j[k]\right] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0u[k] + \sum_{j = 1}^M -b_0a_j \cdot x_j[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0u[k] + \sum_{j = N}^M -b_0a_j \cdot x_j[k] + \sum_{i=1}^{N-1} (b_i - b_0a_i) \cdot x_i[k] \\
&= \begin{bmatrix}b_1 - b_0a_1 & b_2 - b_0a_2 & \cdots & b_{N-1} - b_0a_{N-1} & -b_0a_N & \cdots & -b_0a_{M}\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} + b_0u[k] \\
&= C_rx[k] + Du[k]
\end{align*}
$$
where $D = b_0$ and

$$
C_r = \begin{bmatrix}b_1 - b_0a_1 & b_2 - b_0a_2 & \cdots & b_{N-1} - b_0a_{N-1} & -b_0a_N & \cdots & -b_0a_{M}\end{bmatrix}
$$
