---
layout: post
title: a post with table of contents on a sidebar
date: 2023-04-25 10:14:00-0400
description: an example of a blog post with table of contents on a sidebar
tags: formatting toc sidebar
categories: sample-posts
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

# Curvature
Let $f : \mathbb R^n \to \mathbb R$ be a twice differentiable function, such that its Hessian is denoted as $\nabla^2 f : \mathbb R^n \to \mathbb R^n$. Let $x$ and $d$ be vectors in $\mathbb R^n$. Then, we define the *curvature* $\mathcal C$ of $f$ at $x$ and in the direction $d$ as
$$
\mathcal C(x,d) = \frac{d^T \nabla^2 f(x) d}{d^T d}
$$
## Symmetry
The first interesting property of $\mathcal C$ is that, for every $x \in \mathbb R^n$, $\mathcal C(x,d) = \mathcal C(x,-d)$. That is, the curvature $\mathcal C$ at $x$ is symmetric along the direction $d$.
## Change of basis
Because the Hessian $\nabla^2 f$ is assumed to be real and symmetric everywhere in $\mathbb R^n$, and because of the [spectral theorem](https://math.stackexchange.com/q/82467/652310), it can be diagonalized as $\nabla^2 f(x) = Q(x)\Lambda(x)Q^T(x)$. Therefore, the curvature $\mathcal C$ at $x$ in the direction $d$ can be expressed as
$$
\begin{align}
\mathcal C(x,d) &= \frac{d^T \nabla^2 f(x) d}{d^T d} \\
&= \frac{d^T Q(x)\Lambda(x)Q^T(x) d}{d^T d} \\
&= \frac{w^T(x)\Lambda(x)w(x)}{w^T(x)Q^T(x) Q(x)w(x)} \\
&= \frac{w^T(x)\Lambda(x)w(x)}{w^T(x)w(x)} \\
&= \frac{w^T(x)\Lambda(x)w(x)}{\|w(x)\|^2} \\
&= \frac{w^T(x)}{\|w(x)\|}\Lambda(x)\frac{w(x)}{\|w(x)\|} \\
&= \frac{w^T(x)}{\|w(x)\|}\Lambda(x)^{1/2}\Lambda(x)^{1/2}\frac{w(x)}{\|w(x)\|} \\
&= z^T(x)z(x) \tag{1}
\end{align}
$$
where we set $w(x) = Q^T(x)d$ and $z(x) = \Lambda(x)^{1/2}\frac{w(x)}{\|w(x)\|}$ (to make sure that $\Lambda^{1/2}(x)$ exists, we assumed that $\nabla^2 f(x)$ is positive or negative semi-definite at each $x$). Because $Q(x)$ is an orthonormal matrix for every $x$, then $w(x)$ represents a rotation and/or reflection of the direction $d$.

Therefore, the equation in $(1)$ says that, given a direction $d$ at $x$, we can express this direction $d$ as a linear combination of the eigenvectors of $\nabla^2 f(x)$. This linear combination is contained in $w(x)$, and the eigenvectors are in $Q(x)$. Then, the curvature is obtained by normalizing $w(x)$ to have unit length, scaling each of its entries by the (positive or negative) square roots of the eigenvalues of $\nabla^2 f(x)$, and then computing the resulting length.
## Maximum and minimum curvature
Another interesting property of $\mathcal C$ is that, for every $x \in \mathbb R^n$, the maximum curvature at $x$ occurs in the direction of the eigenvector $v_n(x)$ that corresponds to the largest eigenvalue $\lambda_n(x)$ of the Hessian $\nabla^2 f$ evaluated at $x$. That is,
$$
\begin{align}
\max_d \ \mathcal C(x,d) &= \max_d \ \frac{d^T \nabla^2 f(x) d}{d^T d} \\
&= \frac{v^T_n(x) \nabla^2 f(x) v_n(x)}{v^T_n(x) v_n(x)} \\
&= \lambda_n(x)
\end{align}
$$
Similarly, the minimum curvature at $x$ occurs in the direction of the eigenvector $v_1(x)$ that corresponds to the smallest eigenvalue $\lambda_1(x)$ of the Hessian $\nabla^2 f$ evaluated at $x$. That is,
$$
\begin{align}
\min_d \ \mathcal C(x,d) &= \min_d \ \frac{d^T \nabla^2 f(x) d}{d^T d} \\
&= \frac{v^T_1(x) \nabla^2 f(x) v_1(x)}{v^T_1(x) v_1(x)} \\
&= \lambda_1(x)
\end{align}
$$
