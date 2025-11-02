---
title: "IIR filters in practice"
layout: post
excerpt: "We derive the 4 direct form structures of a discrete-time infinite impulse response (IIR) filter."
tags:
  - signal-processing
  - control-theory
---
We derive the 4 direct form structures of a discrete-time infinite impulse response (IIR) filter. These 4 structues are: Direct Form I, Direct Form II, Transposed Direct Form I, and Transposed Direct Form II.

When I first encouuntered these structures in a signal processing course, I could not find a derivation for where they came from. Moreover, if I did find a derivation, it would often be in graphical form without a mathematical derivation to accompany it.

The purpose of this post is to provide a clear and comprehensive derivation of these 4 structures.

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

In a Direct Form I implementation of an IIR filter, the input $u[k]$ is passed into the all-zero filter $H_b(z)$ first and the output of the all-zero filter is input to the all-pole filter $H_a(z)$ to produce the output $y[k]$. In other words, $u_b[k] = u[k]$, $y_b[k] = u_a[k]$, and $y_a[k] = y[k]$ for each $k$ such that

$$
\begin{align}
u_a[k] &= b_0u[k] + \sum_{i=1}^{N-1} b_i \cdot u[k - i] \label{iir_df1_in} \\
y[k] &= u_a[k] + \sum_{j = 1}^M -a_j \cdot y[k - j] \label{iir_df1_out}
\end{align}
$$

Note that the terms $u[k - 1],\dots,u[k-(N-1)]$ and $y[k - 1],\dots,y[k-M]$ are obtained from memory from a previous iteration. Hence, the Direct Form I structure requires memory for $N - 1 + M$ floating-point numbers.

## Direct Form II

In a Direct Form II implementation of an IIR filter, the input $u[k]$ is passed into the all-pole filter $H_a(z)$ first and the output of the all-pole filter is input to the all-zero filter $H_b(z)$ to produce the output $y[k]$.

In contrast to the Direct Form I structure, the order of the all-pole and all-zero IIR filters are reversed. Additionally, we have that $u_a[k] = u[k]$, $y_a[k] = u_b[k]$, and $y_b[k] = y[k]$ for each $k$ such that

$$
\begin{align}
u_b[k] &= u[k] + \sum_{j = 1}^M -a_j \cdot u_b[k - j] \label{iir_df2_in} \\
y[k] &= b_0u_b[k] + \sum_{i=1}^{N-1} b_i \cdot u_b[k - i] \label{iir_df2_out}
\end{align}
$$

Note that the terms $u_b[k - 1],\dots,u_b[k-M],\dots,u_b[k-(N-1)]$ are obtained from memory from a previous iteration.

In contrast to the Direct Form I structure, which requires memory for $N - 1 + M$ floating-point numbers, the Direct Form II structure requires memory for only $\max(N-1, M)$ floating-point numbers. Because $\max(N-1, M) < N - 1 + M$ when $N - 1 > 0$ and $M > 0$, then the Direct Form II structure requires less memory than the Direct Form I structure.

## Transposed Direct Forms

The main advantage of Transposed Direct Forms I and II is that no temporary storage is required when computing the outputs. Recall from $\eqref{iir_allzero_out}$ that the all-zero filter $H_b(z)$ is defined as

$$
\begin{align*}
H_b(z) = \frac{Y_b(z)}{U_b(z)} &= b_0 + b_1z^{-1} + \cdots + b_{N-1}z^{-(N-1)} \\

Y_b(z) &= b_0U_b(z) + \cdots + b_{N-3}U_b(z)z^{-(N-3)} + b_{N-2}U_b(z)z^{-(N-2)} + b_{N-1}U_b(z)z^{-(N-1)}
\end{align*}
$$

We will show that $H_b(z)$ can be evaluated recursively backwards. First, let $V_{N-1}(z) = b_{N-1}U_b(z)$ be the terminal condition for the recursion. Then, $Y_b(z)$ can be re-written as

$$
\begin{align*}
Y_b(z) &= b_0U_b(z) + \cdots + b_{N-3}U_b(z)z^{-(N-3)} + b_{N-2}U_b(z)z^{-(N-2)} + z^{-(N-1)}V_{N-1}(z) \\
&= b_0U_b(z) + \cdots + b_{N-3}U_b(z)z^{-(N-3)} + z^{-(N-2)}\left[b_{N-2}U_b(z) + z^{-1}V_{N-1}(z)\right] \\
&= b_0U_b(z) + \cdots + b_{N-3}U_b(z)z^{-(N-3)} + z^{-(N-2)}V_{N-2}(z) \\
&= b_0U_b(z) + \cdots + z^{-(N-3)}\left[b_{N-3}U_b(z) + z^{-1}V_{N-2}(z)\right] \\
&= b_0U_b(z) + \cdots + z^{-(N-3)}V_{N-3}(z)
\end{align*}
$$

where $V_{N-2}(z) = b_{N-2}U_b(z) + z^{-1}V_{N-1}(z)$ and $V_{N-3}(z) = b_{N-3}U_b(z) + z^{-1}V_{N-2}(z)$. More generally, for $\ell = N-2,N-3,\dots,0$, let $V_\ell(z) = b_{\ell}U_b(z) + z^{-1}V_{\ell+1}(z)$, such that the recursion that describes the output $Y_b(z)$ of the all-zero IIR filter is

$$
\begin{align*}
V_\ell(z) &= \begin{cases}b_{N-1}U_b(z),&\ell = N-1 \\ b_{\ell}U_b(z) + z^{-1}V_{\ell+1}(z),&\ell = N-2,N-3,\dots,0\end{cases} \\
Y_b(z) &= V_0(z)
\end{align*}
$$

and the corresponding difference equations in the time domain are

$$
\begin{align*}
v_\ell[k] &= \begin{cases}b_{N-1}u_b[k],&\ell = N-1 \\ b_\ell u_b[k] + v_{\ell+1}[k-1],&\ell = N-2,N-3,\dots,0\end{cases} \\
y_b[k] &= v_0[k]
\end{align*}
$$

**TODO: continue from here with the all-pole filter derivation as well**

One of the main advantages of transposed direct forms is that there are no temporary storage requirements, since...

### Transposed Direct Form I

sdfvdfvd

### Transposed Direct Form II


**TODO**: don't say that it can be derived from direct form I. Just derive it from $H(z)$.

The Transposed Direct Form II structure can be derived directly from the Direct Form I structure (personally, I think the name "Transposed Direct Form II" is a misnomer and should instead be called "Transposed Direct Form I").

Looking at



Looking at $\eqref{iir_df1_in}$, define the $N-1$ states $x_1[k],\dots,x_{N-1}[k]$ at the $k$th time-step as

$$
\begin{align*}
x_1[k] &= u[k-1] \\
x_2[k] &= u[k-2] \\
&\vdots \\
x_{N-1}[k] &= u[k-(N-1)]
\end{align*}
$$

Additionally, looking at $\eqref{iir_df1_out}$, define the $M$ states $z_1[k],\dots,z_M[k]$ at the $k$th time-step as

**TODO**: continue from here

$$
\begin{align*}
z_{1}[k] &= y[k-1] \\
&\vdots \\
z_{M}[k] &= y[k-M] \\
\end{align*}
$$

Substituting these states into $\eqref{iir_df1_in}$ and $\eqref{iir_df1_out}$ yields

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

In a Direct Form II IIR filter, the input $u[k]$ is passed into the all-pole filter first and the output of the all-pole filter is input to the all-zero filter to produce the output $y[k]$. In other words, $u[k] = u_a[k]$, $y_a[k] = u_b[k]$, and $y[k] = y_b[k]$ for each $k$ such that

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
