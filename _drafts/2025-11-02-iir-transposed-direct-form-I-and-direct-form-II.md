---
title: "Transposed Direct Form I and Direct Form II implementations of IIR filters"
layout: post
excerpt: "We derive the Transposed Direct Form I and Direct Form II implementations of a discrete-time infinite impulse response (IIR) filter via linear algebra."
tags:
  - signal-processing
  - control-theory
---

In a [previous post](/blog/iir-direct-form-I-and-direct-form-II), we derived the
time-domain difference equations for the Direct Form I (DF1) and Direct Form II (DF2)
implementations of an IIR filter. In this post, we derive the time-domain difference
equations for the Transposed Direct Form I (TDF1) and Transposed Direct Form II (TDF2)
implementations of an IIR filter.

Given the transfer function of a general discrete-time IIR filter, the time-domain
difference equatiosn for the TDF1 and TDF2 implementations can be derived in four steps:

1. Determine the time-domain difference equations for the DF1 and DF2 implementations.
2. Convert the DF1 and DF2 time-domain difference equations from step 1 into state-space
form.
3. Transpose the DF1 state-space equations and the DF2 state-space equations to obtain
the TDF1 state-space equations and the TDF2 state-space equations, respectively.
4. Convert the TDF1 and TDF2 state-space equations into their corresponding time-domain
difference equations.

Step 3 in particular explains why these implementations are _transposed_. For the sake
of clarity, we will follow steps 1-4 first to obtain the TDF2 difference equations and
then follow them again in a separate section to obtain the TDF1 difference equations.

## TDF2

Recall from [Direct Form I and Direct Form II implementations of IIR filters](/blog/iir-direct-form-I-and-direct-form-II)
that the difference equations that describe a DF2 IIR filter are

$$
\begin{align}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot u_b[k - j] \label{iir_df2_in} \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_df2_out}
\end{align}
$$

where $b_0,\dots,b_{N-1}$ are the $N$ feedforward coefficients, $a_1,\dots,a_M$ are the
$M$ feedback coefficients, and at the $k$th time-step,

* $u[k]$ is the input to the IIR filter,
* $u_b[k]$ is an intermediate variable, and
* $y[k]$ is the output of the IIR filter.

Without loss of generality, suppose that $M > N - 1$. Then, define the $M$ states
$x_1[k],x_2[k],\dots,x_{N-1}[k],\dots,x_{M}[k]$ at the $k$th time-step as

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
&= Ax[k] + Bu[k]
\end{align}
$$

where

$$
\begin{align*}
A &= \begin{bmatrix}
-a_1 & -a_2 & -a_3 & \cdots & -a_M \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
B &= \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}
\end{align*}
$$

Additionally, the output equation is

$$
\begin{align*}
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0\left[u[k] + \sum_{j = 1}^M -a_j \cdot x_j[k]\right] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0u[k] + \sum_{j = 1}^M -b_0a_j \cdot x_j[k] + \sum_{i=1}^{N-1} b_i \cdot x_i[k] \\
&= b_0u[k] + \sum_{j = N}^M -b_0a_j \cdot x_j[k] + \sum_{i=1}^{N-1} (b_i - b_0a_i) \cdot x_i[k] \\
&= \begin{bmatrix}b_1 - b_0a_1 & b_2 - b_0a_2 & \cdots & b_{N-1} - b_0a_{N-1} & -b_0a_N & \cdots & -b_0a_{M}\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} + b_0u[k] \\
&= Cx[k] + Du[k]
\end{align*}
$$

where

$$
\begin{align*}
C &= \begin{bmatrix}b_1 - b_0a_1 & b_2 - b_0a_2 & \cdots & b_{N-1} - b_0a_{N-1} & -b_0a_N & \cdots & -b_0a_{M}\end{bmatrix} \\
D &= b_0
\end{align*}
$$

Then, the state equations that describe the DF2 implementation are

$$
\begin{equation*}
\begin{bmatrix}x[k+1] \\ y[k]\end{bmatrix} = \begin{bmatrix}A & B \\ C & D\end{bmatrix}\begin{bmatrix}x[k] \\ u[k]\end{bmatrix}
\end{equation*}
$$

Thus, the equivalent transpose system is

$$
\begin{equation*}
\begin{bmatrix}x[k+1] \\ y[k]\end{bmatrix} = \begin{bmatrix}A^T & C^T \\ B^T & D^T\end{bmatrix}\begin{bmatrix}x[k] \\ u[k]\end{bmatrix}
\end{equation*}
$$

such that

$$
\begin{align*}
x[k+1] &= A^Tx[k] + C^Tu[k] \\
&= \begin{bmatrix}
-a_1 & 1 & 0 & 0 & \cdots & 0 \\
-a_2 & 0 & 1 & 0 & \cdots & 0 \\
-a_3 & 0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \vdots & \ddots & \vdots \\
-a_M & 0 & 0 & \cdots & 0 & 0
\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} + \begin{bmatrix}b_1 - b_0a_1 \\ b_2 - b_0a_2 \\ \vdots \\ b_{N-1} - b_0a_{N-1} \\ -b_0a_N \\ \vdots \\ -b_0a_{M}\end{bmatrix}u[k] \\

y[k] &= B^Tx[k] + D^Tu[k] \\
&= \begin{bmatrix}1 & 0 & \cdots & 0\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k] \\ \vdots \\ x_{M}[k]\end{bmatrix} + b_0u[k] \\
&= x_1[k] + b_0u[k]
\end{align*}
$$

**TODO**: fix the below math. Something is not right...

We then write each of the individual state equations as

$$
\begin{align*}
x_1[k+1] &= u_b[k] \\
&= -a_1x_1[k] + (b_1 - b_0a_1)u[k] \\
&= -a_1u_b[k-1] + (b_1 - b_0a_1)u[k] \\
x_2[k+1] &= u_b[k-1] \\
&= -a_2x_1[k]
\end{align*}
$$






### DF1

Recall from [Direct Form I and Direct Form II implementations of IIR filters](/blog/iir-direct-form-I-and-direct-form-II)
that the difference equations that describe a DF1 IIR filter are

$$
\begin{align}
u_a[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] \label{iir_df1_in} \\
y[k] &= u_a[k] + \sum_{j = 1}^M -a_j \cdot y[k - j] \label{iir_df1_out}
\end{align}
$$

where $b_0,\dots,b_{N-1}$ are the $N$ feedforward coefficients, $a_1,\dots,a_M$ are the
$M$ feedback coefficients, and at the $k$th time-step,

* $u[k]$ is the input to the IIR filter,
* $u_a[k]$ is an intermediate variable, and
* $y[k]$ is the output of the IIR filter.

Looking at $(1)$, define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

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

Substituting $\eqref{iir_df1_in}$ into $\eqref{iir_df1_out}$ and then substituting the
aforementioned states into the resulting equation yields

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

