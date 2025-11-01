---
title: "FIR and IIR filters in state-space form"
layout: post
tags:
  - signal-processing
  - control-theory
---
We show how to express a discrete-time infinite impulse response (IIR) filter in state-space form.

Linear and time-invariant (LTI) filters are not often expressed in state-space form since in filtering applications, we have control over the filter and not the input signal, while in control applications, the opposite is true. Nonetheless, expressing LTI filters in state-space form offers another way of analyzing the filter and bridges the fields of signal processing and control theory.

**TODO**: fix this part because it seems redundant

We first derive the difference equation that describes a general discrete-time IIR filter from the general discrete-time IIR filter transfer function, which is

$$
\begin{align}
H(z) = \frac{Y(z)}{U(z)} &= \frac{b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)}}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \label{iir_tf} \\
Y(z)\left[1 + a_1z^{-1} + \cdots + a_Mz^{-M}\right] &= U(z)\left[b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)}\right] \nonumber \\
Y(z) + a_1Y(z)z^{-1} + \cdots + a_MY(z)z^{-M} &= b_0U(z) + b_1U(z)z^{-1} + \cdots + b_{N-1}U(z)z^{-(N-1)} \label{iir_tf1}
\end{align}
$$

Computing the inverse Z-transform of both sides of $\eqref{iir_tf1}$ yields

$$
\begin{align}
y[k] + a_1y[k-1] + \cdots + a_My[k-M] &= b_0u[k] + b_1u[k-1] + \cdots + b_{N-1}u[k-(N-1)] \nonumber \\
y[k] &= b_0u[k] + b_1u[k-1] + \cdots + b_{N-1}u[k-(N-1)] - a_1y[k-1] - \cdots - a_My[k-M] \nonumber \\
&= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] + \sum_{j=1}^M -a_j \cdot y[k-j] \label{iir_out}
\end{align}
$$

where $u[k]$ and $y[k]$ are the input and output signals at the $k$th time-step, respectively, $b_0, \dots,b_{N-1}$ are the $N$ feedforward coefficients, and $a_1,\dots,a_M$ are the $M$ feedback coefficients.

## Cascade structure of an IIR filter

There are two main ways of expressing an IIR filter in state-space form. Each method has its pros and cons. These two methods can be derived as follows.

Looking at $\eqref{iir_tf}$ and $\eqref{iir_out}$, we see that an IIR filter consists of an all-zero IIR filter and an all-pole IIR filter in cascade. That is, let

$$
\begin{align}
H_b(z) &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \label{iir_allzero} \\
H_a(z) &= \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \label{iir_allpole}
\end{align}
$$

such that $H(z) = H_a(z) \cdot H_b(z) = H_b(z) \cdot H_a(z)$. This provides us with two ways for expressing an IIR filter: as an all-zero IIR filter followed by an all-pole IIR filter, called _Direct Form I_, or as an all-pole IIR filter followed by an all-zero IIR filter, called _Direct Form II_.

That is, from $\eqref{iir_allzero}$,

$$
\begin{align}
H_b(z) = \frac{Y_b(z)}{U_b(z)} &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \\
Y_b(z) &= b_0U_b(z) + b_1U_b(z)z^{-1} + \cdots + b_{N-1}U_b(z)z^{-(N-1)} \\
y_b[k] &= b_0u_b[k] + b_1u_b[k-1] + \cdots + b_{N-1}u_b[k-(N-1)] \\
&= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_allzero_out}
\end{align}
$$

and from $\eqref{iir_allpole}$,

$$
\begin{align}
H_a(z) = \frac{Y_a(z)}{U_a(z)} &= \frac{1}{1 + a_1z^{-1} + \cdots + a_Mz^{-M}} \\
Y_a(z) + a_1Y_a(z)z^{-1} + \cdots + a_MY_a(z)z^{-M} &= U_a(z) \\
y_a[k] + a_1y_a[k-1] + \cdots + a_My_a[k-M] &= u_a[k] \\
y_a[k] &= u_a[k] - a_1y_a[k-1] - \cdots - a_My_a[k-M] \\
&= u_a[k] + \sum_{j = 1}^M -a_j \cdot y_a[k - j] \label{iir_allpole_out}
\end{align}
$$

## Direct Form I

In a Direct Form I IIR filter, the input $u[k]$ is passed into the all-zero filter first and the output of the all-zero filter is input to the all-pole filter to produce the output $y[k]$. In other words, $u[k] = u_b[k]$, $y_b[k] = u_a[k]$, and $y[k] = y_a[k]$ for each $k$ such that

$$
\begin{align}
u_a[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] \\
y[k] &= u_a[k] + \sum_{j = 1}^M -a_j \cdot y[k - j]
\end{align}
$$

Looking at $\eqref{iir_out}$, define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u[k-1] \\
x_2[k] &= u[k-2] \\
&\vdots \\
x_{N-1}[k] &= u[k-(N-1)]
\end{align*}
$$

and define the $M$ states $z_1[k],\dots,z_M[k]$ at the $k$th time-step as

$$
\begin{align*}
z_{1}[k] &= y[k-1] \\
&\vdots \\
z_{M}[k] &= y[k-M] \\
\end{align*}
$$

Substituting these states into $\eqref{iir_out}$ yields

$$
\begin{equation}
y[k] = h[0] \cdot u[k] + \sum_{i=1}^{N-1} h[i] \cdot x_i[k] + \sum_{j=1}^{M} g[j] \cdot z_j[k]
\end{equation}
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
x[k+1] &= \begin{bmatrix}u[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k]\end{bmatrix} \\

&= \begin{bmatrix}0 \\ x_1[k] \\ \vdots \\ x_{N-2}[k]\end{bmatrix} + \begin{bmatrix}u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \\

&= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} + \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \\

&= A_x x[k] + B_x u[k]\\

z[k+1] &= \begin{bmatrix}y[k] \\ z_1[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} \\

&= \begin{bmatrix}h[0] \cdot u[k] + \sum_{i=1}^{N-1} h[i] \cdot x_i[k] + \sum_{j=1}^{M} g[j] \cdot z_j[k] \\ z_1[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} \\

&= \begin{bmatrix}\sum_{j=1}^{M} g[j] \cdot z_j[k] \\ z_1[k] \\ \vdots \\ z_{M-1}[k]\end{bmatrix} + \begin{bmatrix}\sum_{i=1}^{N-1} h[i] \cdot x_i[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} + \begin{bmatrix}h[0] \cdot u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \\

&= \begin{bmatrix}
g[1] & g[2] & g[3] & \cdots & g[M] \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix}\begin{bmatrix}z_1[k] \\ \vdots \\ z_{M}[k]\end{bmatrix} + \begin{bmatrix}h[1] & h[2] & \cdots & h[N-1] \\ 0 & 0 & \cdots & 0 \\ \vdots & \vdots & \vdots & 0 \\ 0 & 0 & \cdots & 0\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} + \begin{bmatrix}h[0] \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k]  \\

&= A_zz[k] + A_{zx}x[k] + B_zu[k]
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
g[1] & g[2] & g[3] & \cdots & g[M] \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \\
A_{zx} &= \begin{bmatrix}h[1] & h[2] & \cdots & h[N-1] \\ 0 & 0 & \cdots & 0 \\ \vdots & \vdots & \vdots & 0 \\ 0 & 0 & \cdots & 0\end{bmatrix} \\
B_z &= \begin{bmatrix}h[0] \\ 0 \\ \vdots \\ 0\end{bmatrix}
\end{align*}
$$

Notice that the state $x$ evolves independently of $z$, while $z$ evolves as a function of both $u[k]$ and $x[k]$. Hence, $x[k]$ is an input to the system with state $z$ along with $u[k]$.

Notice also that:

$$
\begin{align}
z[k+1] &= A_zz[k] + B_z^xx[k] + B_zu[k] \\
z[k+1] - A_zz[k] - B_z^xx[k] &= B_zu[k] \\
z[k+1] - A_zz[k] - B_z^xx[k] &= h[0]B_xu[k] \\
z[k+1] - A_zz[k] - B_z^xx[k] + A_xx[k] &= A_xx[k] + h[0]B_xu[k] \\
z[k+1] - A_zz[k] - B_z^xx[k] + A_xx[k] &= A_xx[k] + h[0]B_xu[k] \\
\end{align}
$$

Finally, the output equation is

$$
\begin{align}
y[k] &= h[0] \cdot u[k] + \sum_{i=1}^{N-1} h[i] \cdot x_i[k] + \sum_{j=1}^{M} g[j] \cdot z_j[k] \\

&= \begin{bmatrix}h[1] & \cdots & h[N-1]\end{bmatrix}\begin{bmatrix}x_1[k] \\ \vdots \\ x_{N-1}[k]\end{bmatrix} + \begin{bmatrix}g[1] & \cdots & g[M]\end{bmatrix}\begin{bmatrix}z_1[k] \\ \vdots \\ z_{M}[k]\end{bmatrix} + h[0] \cdot u[k] \\
&= C_xx[k] + C_zz[k] + Du[k]
\end{align}
$$


## Direct Form II

For Direct Form II,

$$
\begin{align}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot u_b[k - j] \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i]
\end{align}
$$



<!-- $$
\begin{align}
x[k+1] &= \begin{bmatrix}u[k] \\ u[k-1] \\ \vdots \\ u[k - (N-2)] \\ y[k] \\ y[k-1] \\ \vdots \\ y[k-(M-1)]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}u[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ y[k] \\ x_N[k] \\ \vdots \\ x_{N+M-2}[k]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}u[k] \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ h[0] \cdot u[k] + \sum_{i=1}^{N-1} h[i] \cdot x_i[k] + \sum_{j=N}^{N+M-1} g[j] \cdot x_j[k] \\ x_N[k] \\ \vdots \\ x_{N+M-2}[k]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}0 \\ x_1[k] \\ \vdots \\ x_{N-2}[k] \\ \sum_{i=1}^{N-1} h[i] \cdot x_i[k] + \sum_{j=N}^{N+M-1} g[j] \cdot x_j[k] \\ x_N[k] \\ \vdots \\ x_{N+M-2}[k]\end{bmatrix} + \begin{bmatrix}u[k] \\ 0 \\ \vdots \\ 0 \\ h[0] \cdot u[k] \\ 0 \\ \vdots \\ 0\end{bmatrix} \nonumber \\
&= \begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
1 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
\vdots & \vdots & \vdots & \cdots & \vdots \\
0 & 0 & \cdots & 1 & 0 \\
0 & 0 & \cdots & 1 & 0
\end{bmatrix} \begin{bmatrix}x_1[k] \\ x_2[k] \\ \vdots \\ x_{N-2}[k] \\ x_{N-1}[k] \\ x_{N}[k] \\ \vdots \\ x_{N+M-1}[k]\end{bmatrix} + \begin{bmatrix}1 \\ 0 \\ \vdots \\ 0 \\ h[0] \\ 0 \\ \vdots \\ 0\end{bmatrix}u[k] \nonumber \\
&= Ax[k] + Bu[k] \label{state_eq}
\end{align}
$$ -->

## TODO

Finish the derivation above

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

Next, we show how to express an IIR filter in state space form. Given an input $u[k]$ at the $k$th time-step, the output $y[k]$ at the $k$th time-step, for $k = 0,\dots,L-1$, is

$$
\begin{align}
y[k] &= h[0] \cdot u[k] + h[1] \cdot u[k - 1] + \cdots + h[N-1] \cdot u[k - (N-1)] + g[1] \cdot y[k-1] + g[2] \cdot y[k-2] + \cdots + g[M] \cdot y[k - M] \nonumber \\
&= \sum_{i=0}^{N-1} h[i] \cdot u[k - i] + \sum_{j=1}^{M} g[j] \cdot y[k - j] \label{iir_out}
\end{align}
$$

In contrast to an FIR filter, an IIR filter involves feeding back previous outputs as inputs. With the introduction of feedback, an IIR filter can be unstable. That is, given a bounded input sequence $u[0],u[1],\dots$, as $k \to \infty$, $\| y[k] \|\to \infty$.

Define the states $x_1[k], \dots, x_M[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= y[k-1] \\
x_2[k] &= y[k-2] \\
&\vdots \\
x_{M}[k] &= y[k-M]
\end{align*}
$$

Stack these states to form the state vector at the $k$th time-step,

$$
\begin{equation}
x[k] = \begin{bmatrix}y[k-1] \\ y[k-2] \\ \vdots \\ y[k - M]\end{bmatrix} \label{state_vec_iir}
\end{equation}
$$

Then,

$$
\begin{align}
x[k+1] &= \begin{bmatrix}y[k] \\ y[k-1] \\ \vdots \\ y[k-(M-1)]\end{bmatrix} \nonumber \\
&= \begin{bmatrix}\sum_{i=0}^{N-1} h[i] \cdot u[k - i] + \sum_{j=1}^{M} g[j] \cdot y[k - j] \\ y[k-1] \\ \vdots \\ y[k-(M-1)]\end{bmatrix} \\
&= \begin{bmatrix}\sum_{j=1}^{M} g[j] \cdot y[k - j] \\ y[k-1] \\ \vdots \\ y[k-(M-1)]\end{bmatrix} + \begin{bmatrix}\sum_{i=0}^{N-1} h[i] \cdot u[k - i] \\ 0 \\ \vdots \\ 0\end{bmatrix} \\
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
