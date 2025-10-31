---
title: "FIR and IIR filters in state-space form"
layout: post
tags:
  - signal-processing
  - control-theory
---
**TODO: give up on direct form II derivation. Focus on direct form I.**

We show how to express an infinite impulse response (IIR) filter in state-space form.

Linear and time-invariant (LTI) filters are not often expressed in state-space form, since in filtering applications, we have control over the filter and not the input signal, while in control applications, the opposite is true. Nonetheless, expressing LTI filters in state-space form offers another way of analyzing the filter.

Let $u[0],\dots,u[L-1]$ be the input signal. Let the corresponding output signal of the IIR filter be $y[0],\dots,y[L-1]$, such that

$$
\begin{align}
y[k] &= h[0] \cdot u[k] + \sum_{i=1}^{N-1} h[i] \cdot u[k - i] + \sum_{j=1}^{M} g[j] \cdot y[k - j] \label{iir_out}
\end{align}
$$

> **TODO**: Express (1) recursively by assigning w[k - M] = ... then assigning w[k-(M-1)] = ... then assigning w[k-(M-2)] = ... and so on.

for $k = 0,\dots,L-1$, where $h[0], \dots, h[N-1]$ are the $N$ feedforward coefficients and $g[1], \dots, g[M]$ are the $M$ feedback coefficients. We convert $\eqref{iir_out}$ into state-space form as follows. Define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u[k-1] \\
x_2[k] &= u[k-2] \\
&\vdots \\
x_{N-1}[k] &= u[k-(N-1)]
\end{align*}
$$

Then, define the $M$ states $z_1[k],\dots,z_M[k]$ at the $k$th time-step as

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

Note that $z_i[k]$ is related to $x_1[k],\dots,x_{N-1}[k]$ via

$$
\begin{align}
z_i[k] &= y[k - i] \\

&= h[0] \cdot u[k-i] + \sum_{j=1}^{N-1} h[j] \cdot x_j[k-i] + \sum_{\ell=1}^{M} g[\ell] \cdot z_\ell[k-i] \\

&= h[0] \cdot x_i[k] + \sum_{j=1}^{N-1} h[j] \cdot x_{j+i}[k] + \sum_{\ell=1}^{M} g[\ell] \cdot z_{\ell+i}[k]
\end{align}
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

&= A_zz[k] + B_z^xx[k] + B_zu[k]
\end{align}
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
