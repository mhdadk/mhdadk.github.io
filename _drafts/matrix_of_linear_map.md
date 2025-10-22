---
tags:
  - linear-algebra
  - geometry
---
$\newcommand\c[2]{c_{#1}^{#2}}$We describe how to derive the matrix representation of a linear map.

Consider the vector spaces $(V,\mathbb F_V, +, \cdot)$ and $(W, \mathbb F_W,+,\cdot)$ of dimensions $\dim(V) = n$ and $\dim(W) = m$. Let $v_1,\dots,v_n \in V$ be a basis for $V$ and let $w_1,\dots,w_m \in W$ and $e_1,\dots,e_m \in W$ be two different bases for $W$.

Consider the linear map $T : V \to W$. Our goal is to determine the matrix representation of the linear map $T$, which we denote as $M(T)$.

Consider the vector $v \in V$ that can be expressed with respect to the basis vectors $v_1,\dots,v_n$ as
$$
v = \c{v}{v_1}v_1 + \cdots + \c{v}{v_n}v_n \tag{1}
$$
Because $T$ is linear,
$$
\begin{align}
T(v) = w' &= \c{w'}{w_1'}T(v_1) + \cdots + \c{w'}{w_n'}T(v_n) \\
&= \c{w'}{w_1'}w_1' + \cdots + \c{w'}{w_n'}w_n' \tag{2}
\end{align}
$$
where $T(v_i) = w_i'$ for $i \in \{1,\dots,n\}$. Our ultimate goal is to express $w'$ with respect to the basis vectors $w_1,\dots,w_m \in W$, which will naturally lead to the definition of $M(T)$.

 We first assume that $w_1',\dots,w_n'$, as defined in $(2)$, can be expressed with respect to the basis vectors $w_1,\dots,w_m \in W$ as
$$
\begin{align}
w_1' &= \c{w_1'}{w_1}w_1 + \cdots + \c{w_1'}{w_m}w_m \\
&\vdots \\
w_n' &= \c{w_n'}{w_1}w_1 + \cdots + \c{w_n'}{w_m}w_m \\
\end{align} \tag{3}
$$
Substituting $(3)$ into $(2)$ yields
$$
\begin{align}
w' &= \c{w'}{w_1'}w_1' + \cdots + \c{w'}{w_n'}w_n' \\
&= \c{w'}{w_1'}\left(\c{w_1'}{w_1}w_1 + \cdots + \c{w_1'}{w_m}w_m\right) + \cdots + \c{w'}{w_n'}\left(\c{w_n'}{w_1}w_1 + \cdots + \c{w_n'}{w_m}w_m\right) \\
&= \c{w'}{w_1'}\c{w_1'}{w_1}w_1 + \cdots + \c{w'}{w_1'}\c{w_1'}{w_m}w_m + \cdots + \c{w'}{w_n'}\c{w_n'}{w_1}w_1 + \cdots + \c{w'}{w_n'}\c{w_n'}{w_m}w_m \\
&= \left(\c{w'}{w_1'}\c{w_1'}{w_1} + \cdots + \c{w'}{w_n'}\c{w_n'}{w_1}\right)w_1 + \cdots + \left(\c{w'}{w_1'}\c{w_1'}{w_m} + \cdots + \c{w'}{w_n'}\c{w_n'}{w_m}\right)w_m \tag{4}
\end{align}
$$
The goal then is to determine the $m\times n$ unknown scalars $\c{w_1'}{w_1}, \dots,\c{w_n'}{w_1},\dots,\c{w_1'}{w_m},\dots,\c{w_n'}{w_m}$ in $(4)$, which make up $M(T)$. The process for computing these unknown scalars will be explored in the rest of this note.

We first assume that each basis vector $w_1,\dots,w_m \in W$ can be expressed with respect to the basis vectors $e_1,\dots,e_m \in W$ as
$$
\begin{align}
w_1 &= \c{w_1}{e_1}e_1 + \cdots + \c{w_1}{e_m}e_m \\
&\vdots \\
w_m &= \c{w_m}{e_1}e_1 + \cdots + \c{w_m}{e_m}e_m \\
\end{align} \tag{5}
$$
Similarly, we assume that the vectors $w_1',\dots,w_n'$ can be expressed with respect to the basis vectors $e_1,\dots,e_m \in W$ as
$$
\begin{align}
w_1' &= \c{w_1'}{e_1}e_1 + \cdots + \c{w_1'}{e_m}e_m \\
&\vdots \\
w_n' &= \c{w_n'}{e_1}e_1 + \cdots + \c{w_n'}{e_m}e_m \\
\end{align} \tag{6}
$$
Substituting $(5)$ and $(6)$ into $(3)$ yields
$$
\begin{align}
\c{w_1'}{e_1}e_1 + \cdots + \c{w_1'}{e_m}e_m &= \c{w_1'}{w_1}\left[\c{w_1}{e_1}e_1 + \cdots + \c{w_1}{e_m}e_m\right] + \cdots + \c{w_1'}{w_m}\left[\c{w_m}{e_1}e_1 + \cdots + \c{w_m}{e_m}e_m\right] \\
&\vdots \\
\c{w_n'}{e_1}e_1 + \cdots + \c{w_n'}{e_m}e_m &= \c{w_n'}{w_1}\left[\c{w_1}{e_1}e_1 + \cdots + \c{w_1}{e_m}e_m\right] + \cdots + \c{w_n'}{w_m}\left[\c{w_m}{e_1}e_1 + \cdots + \c{w_m}{e_m}e_m\right] \\ \\ \\

\c{w_1'}{e_1}e_1 + \cdots + \c{w_1'}{e_m}e_m &= \c{w_1'}{w_1}\c{w_1}{e_1}e_1 + \cdots + \c{w_1'}{w_1}\c{w_1}{e_m}e_m + \cdots + \c{w_1'}{w_m}\c{w_m}{e_1}e_1 + \cdots + \c{w_1'}{w_m}\c{w_m}{e_m}e_m \\
&\vdots \\
\c{w_n'}{e_1}e_1 + \cdots + \c{w_n'}{e_m}e_m &= \c{w_n'}{w_1}\c{w_1}{e_1}e_1 + \cdots + \c{w_n'}{w_1}\c{w_1}{e_m}e_m + \cdots + \c{w_n'}{w_m}\c{w_m}{e_1}e_1 + \cdots + \c{w_n'}{w_m}\c{w_m}{e_m}e_m \\ \\ \\

\c{w_1'}{e_1}e_1 + \cdots + \c{w_1'}{e_m}e_m &= \left[\c{w_1'}{w_1}\c{w_1}{e_1} + \cdots + \c{w_1'}{w_m}\c{w_m}{e_1}\right]e_1 + \cdots + \left[\c{w_1'}{w_1}\c{w_1}{e_m} + \cdots + \c{w_1'}{w_m}\c{w_m}{e_m}\right]e_m \\
&\vdots \tag{7} \\
\c{w_n'}{e_1}e_1 + \cdots + \c{w_n'}{e_m}e_m &= \left[\c{w_n'}{w_1}\c{w_1}{e_1} + \cdots + \c{w_n'}{w_m}\c{w_m}{e_1}\right]e_1 + \cdots + \left[\c{w_n'}{w_1}\c{w_1}{e_m} + \cdots + \c{w_n'}{w_m}\c{w_m}{e_m}\right]e_m
\end{align}
$$
Equating coefficients in $(7)$, we obtain the following $m \times n$ equations (since there are $m \times n$ unknowns in $M(T)$):
$$
\begin{align}
\c{w_1'}{e_1} &= \c{w_1'}{w_1}\c{w_1}{e_1} + \cdots + \c{w_1'}{w_m}\c{w_m}{e_1} \\
&\vdots \\
\c{w_1'}{e_m} &= \c{w_1'}{w_1}\c{w_1}{e_m} + \cdots + \c{w_1'}{w_m}\c{w_m}{e_m} \\
&\vdots\vdots \\
\c{w_n'}{e_1} &= \c{w_n'}{w_1}\c{w_1}{e_1} + \cdots + \c{w_n'}{w_m}\c{w_m}{e_1} \\
&\vdots \\
\c{w_n'}{e_m} &= \c{w_n'}{w_1}\c{w_1}{e_m} + \cdots + \c{w_n'}{w_m}\c{w_m}{e_m}
\end{align}
$$
In matrix form, we can express these $n$ collections of equations as
$$
\begin{align}
\begin{bmatrix}\c{w_1'}{e_1} \\ \vdots \\ \c{w_1'}{e_m}\end{bmatrix} &=
\begin{bmatrix}
\c{w_1}{e_1} & \c{w_2}{e_1} & \cdots & \c{w_m}{e_1} \\
\c{w_1}{e_2} & \c{w_2}{e_2} & \cdots & \c{w_m}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1}{e_m} & \c{w_2}{e_m} & \cdots & \c{w_m}{e_m}
\end{bmatrix}
\begin{bmatrix}\c{w_1'}{w_1} \\ \vdots \\ \c{w_1'}{w_m}\end{bmatrix} \\
&\vdots\vdots \\
\begin{bmatrix}\c{w_n'}{e_1} \\ \vdots \\ \c{w_n'}{e_m}\end{bmatrix} &=
\begin{bmatrix}
\c{w_1}{e_1} & \c{w_2}{e_1} & \cdots & \c{w_m}{e_1} \\
\c{w_1}{e_2} & \c{w_2}{e_2} & \cdots & \c{w_m}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1}{e_m} & \c{w_2}{e_m} & \cdots & \c{w_m}{e_m}
\end{bmatrix}
\begin{bmatrix}\c{w_n'}{w_1} \\ \vdots \\ \c{w_n'}{w_m}\end{bmatrix}
\end{align} \tag{8}
$$
The $n$ equations in $(8)$ can be stacked together to form the following linear matrix equation:
$$
\begin{align}
\begin{bmatrix}
\c{w_1}{e_1} & \c{w_2}{e_1} & \cdots & \c{w_m}{e_1} \\
\c{w_1}{e_2} & \c{w_2}{e_2} & \cdots & \c{w_m}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1}{e_m} & \c{w_2}{e_m} & \cdots & \c{w_m}{e_m}
\end{bmatrix}
\begin{bmatrix}
\c{w_1'}{w_1} & \c{w_2'}{w_1} & \cdots & \c{w_n'}{w_1} \\
\c{w_1'}{w_2} & \c{w_2'}{w_2} & \cdots & \c{w_n'}{w_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1'}{w_m} & \c{w_2'}{w_m} & \cdots & \c{w_n'}{w_m}
\end{bmatrix} &=
\begin{bmatrix}
\c{w_1'}{e_1} & \c{w_2'}{e_1} & \cdots & \c{w_n'}{e_1} \\
\c{w_1'}{e_2} & \c{w_2'}{e_2} & \cdots & \c{w_n'}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1'}{e_m} & \c{w_2'}{e_m} & \cdots & \c{w_n'}{e_m}
\end{bmatrix} \\
A\cdot M(T) &= B
\end{align}
$$
where
$$
\begin{align}
A &= \begin{bmatrix}
\c{w_1}{e_1} & \c{w_2}{e_1} & \cdots & \c{w_m}{e_1} \\
\c{w_1}{e_2} & \c{w_2}{e_2} & \cdots & \c{w_m}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1}{e_m} & \c{w_2}{e_m} & \cdots & \c{w_m}{e_m}
\end{bmatrix} \tag{9} \\
M(T) &= \begin{bmatrix}
\c{w_1'}{w_1} & \c{w_2'}{w_1} & \cdots & \c{w_n'}{w_1} \\
\c{w_1'}{w_2} & \c{w_2'}{w_2} & \cdots & \c{w_n'}{w_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1'}{w_m} & \c{w_2'}{w_m} & \cdots & \c{w_n'}{w_m}
\end{bmatrix} \tag{10} \\
B &= \begin{bmatrix}
\c{w_1'}{e_1} & \c{w_2'}{e_1} & \cdots & \c{w_n'}{e_1} \\
\c{w_1'}{e_2} & \c{w_2'}{e_2} & \cdots & \c{w_n'}{e_2} \\
\vdots & \vdots & \ddots & \vdots \\
\c{w_1'}{e_m} & \c{w_2'}{e_m} & \cdots & \c{w_n'}{e_m}
\end{bmatrix} \tag{11}
\end{align}
$$
Looking at $(3)$, we see that $M(T)$, as defined in $(10)$, contains all the unknown scalars we seek to determine. That is, the $i$th column vector of $M(T)$ contains the coordinate vector for $w_i'$ expressed with respect to the basis vectors $w_1,\dots,w_m \in W$.

Additionally, note that the $i$th column of $B$ contains the coordinate vector for $w_i'$ expressed with respect to the basis vectors $e_1,\dots,e_m$.

Fortunately, because the $i$th column of $A$ contains the coordinate vector for $w_i$ expressed with respect to the basis vectors $e_1,\dots,e_m$, and because $w_1,\dots,w_m$ form a basis, then $A^{-1}$ will always exist.

Hence, $M(T)$ can be obtained by computing $M(T) = A^{-1}B$. Note that $M(T)$ will depend on the choice of basis $w_1,\dots,w_m \in W$ and the vectors $w_1',\dots,w'_n$, which in turn depend on the choice of basis $v_1,\dots,v_n \in V$. Therefore, $M(T)$ is known as the *matrix representation of $T$ with respect to the bases $v_1,\dots,v_n$ and $w_1,\dots,w_m$*.

To summarize, given the vector spaces $(V,\mathbb F_V, +, \cdot)$ and $(W, \mathbb F_W,+,\cdot)$ of dimension $\dim(V) = n$ and $\dim(W) = m$ and given the basis $v_1,\dots,v_n$ for $V$ and the bases $w_1,\dots,w_m$ and $e_1,\dots,e_m$ for $W$, the matrix representation of the linear map $T : V \to W$, denoted as $M(T)$, can be computed as follows:
1. Compute the coordinate vectors of $T(v_1) = w_1',\dots,T(v_n) = w_n'$ with respect to the basis $e_1,\dots,e_m$.
2. Given the coordinate vectors from step 1, concatenate them to form the $B$ matrix as defined in $(11)$.
3. Compute the coordinate vectors of $w_1,\dots,w_m$ with respect to the basis $e_1,\dots,e_m$.
4. Given the coordinate vectors from step 3, concatenate them to form the $A$ matrix as defined in $(9)$.
5. Compute $M(T) = A^{-1}B$.

> [!IMPORTANT]
> The crucial step for computing $M(T)$ was to define *two* sets of basis vectors for the co-domain $W$ of $T$ instead of only one.
> 
> Additionally, unless otherwise stated, if $V$ and $W$ are Euclidean vector spaces and the definition of the linear map $T$ is given without mention of a basis, then it can be assumed that $w_1 = e_1,\dots,w_m = e_m$ and that $e_1,\dots,e_m$ is the standard basis in $W$. In this case, the matrix $A$ is simply the identity matrix, such that $M(T) = B$.
# Change of basis
In the special case that the vector space $W$ is the same as the vector space $V$, such that $\dim(V) = \dim(W) = n$, $T : V \to V$ is a linear operator over $V$, and all three bases $v_1,\dots,v_n$, $w_1,\dots,w_n$, and $e_1,\dots,e_n$ are in $V$, then $M(T)$ is known as the *change of basis matrix from $v_1,\dots,v_n$ to $w_1,\dots,w_n$*.

