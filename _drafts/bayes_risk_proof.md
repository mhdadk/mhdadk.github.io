---
title: "The optimal decision rule"
layout: post
tags:
  - probability
  - machine-learning
---
Given an image of a number in the range 0 - 9, what is the optimal way to decide which number is in the image?

If this question sounds familiar, that's because it is. This is the standard [MNIST](https://en.wikipedia.org/wiki/MNIST_database) classification task that is often introduced as a toy example in introductory Machine Learning (ML) classes.

Surprisingly, this question is never explicitly answered in these classes, and one is instead taught how to apply some ML technique (including, but not limited to, deep learning) to solve this classification problem.

Let's review the classical deep learning approach to solving this problem. Each image in the MNIST dataset has a size of 28x28. We first flatten each image into a 784-dimensional vector $x$.

Outline:

* Review deep learning approach.
* Training and testing splits.
* Consider first the testing approach and assume net is fully trained so that we 
have a perfect p(c | x).
* Mention that we take argmax of output. Why do we do this? That's the purpose of this post.
* Applying softmax in the end and then using cross-entropy loss to train.
* Mention how this is essentially maximum-likelihood estimation.

---

**OLD**

Let $Y$ and $\hat Y$ be Bernoulli random variables that represent the hypotheses and decisions respectively. The decision $\hat Y$ is a function of the observation $X$, such that if $\mathcal X = R \cup \bar R$ is the set of all possible observations, then
$$
\hat Y = \begin{cases} 0, X \in R \\ 1, X \in \bar R\end{cases}
$$
where $\bar R$ is the complement of the set $R$. The observation $X$, in turn, is a probabilistic function of $Y$ described by the conditional probability density $p(x \mid Y = y)$. Therefore,
$$
\begin{align}
P(\hat Y = 0 \mid Y = 0) = P(X \in R \mid Y = 0) &= \int_R p(x \mid Y = 0) \, \text{d}x \\
P(\hat Y = 0 \mid Y = 1) = P(X \in R \mid Y = 1) &= \int_R p(x \mid Y = 1) \, \text{d}x \\
P(\hat Y = 1 \mid Y = 0) = P(X \in \bar R \mid Y = 0) &= \int_{\bar R} p(x \mid Y = 0) \, \text{d}x \\
P(\hat Y = 1 \mid Y = 1) = P(X \in \bar R \mid Y = 1) &= \int_{\bar R} p(x \mid Y = 1) \, \text{d}x
\end{align}
$$
Let $C(Y,\hat Y)$ be a cost function that is defined as
$$
C(Y,\hat Y) =
\begin{cases}
C_{00}, &Y = 0, \hat Y = 0 \\
C_{01}, &Y = 0, \hat Y = 1 \\
C_{10}, &Y = 1, \hat Y = 0 \\
C_{11}, &Y = 1, \hat Y =1 \\
\end{cases}
$$
We define the *risk* as
$$
E_{Y,\hat Y}[C(Y,\hat Y)] = C_{00}P(Y = 0,\hat Y = 0) + C_{01}P(Y = 0,\hat Y = 1) + C_{10}P(Y = 1,\hat Y = 0) + C_{11}P(Y = 1,\hat Y = 1)
$$
Recall that
$$
\hat Y = \begin{cases} 0, X \in R \\ 1, X \in \bar R\end{cases}
$$
Note that $\hat Y$ is a function of both the observation $X$ **and** the set $R$. Therefore, $\hat Y = \hat Y(X,R)$. We therefore want to solve the following optimization problem
$$
\min_{R}{E_{Y,\hat Y}[C(Y,\hat Y(X,R))]}
$$
To do so, we write out the risk as follows
$$
\begin{align}
E_{Y,\hat Y}[C(Y,\hat Y)] &= C_{00}P(Y = 0,\hat Y = 0) + C_{01}P(Y = 0,\hat Y = 1) + C_{10}P(Y = 1,\hat Y = 0) + C_{11}P(Y = 1,\hat Y = 1) \\
&= C_{00}P(Y = 0)P(\hat Y = 0 \mid Y = 0) + C_{01}P(Y = 1)P(\hat Y = 0 \mid Y = 1) + C_{10}P(Y = 0)P(\hat Y = 1 \mid Y = 0) + C_{11}P(Y = 1)P(\hat Y = 1 \mid Y = 1) \\
&= C_{00}P(Y = 0)P(\hat Y = 0 \mid Y = 0) + C_{01}P(Y = 1)P(\hat Y = 0 \mid Y = 1) + C_{10}P(Y = 0)(1 - P(\hat Y = 0 \mid Y = 0)) + C_{11}P(Y = 1)(1 - P(\hat Y = 0 \mid Y = 1)) \\
&= P(\hat Y = 0 \mid Y = 0)(C_{00}P(Y = 0) - C_{10}P(Y = 0)) + P(\hat Y = 0 \mid Y = 1)(C_{01}P(Y = 1) - C_{11}P(Y = 1)) + C_{10}P(Y = 0) + C_{11}P(Y = 1) \\
&= \left[\int_R p(x \mid Y = 0) \, \text{d}x\right](C_{00}P(Y = 0) - C_{10}P(Y = 0)) + \left[\int_R p(x \mid Y = 1) \, \text{d}x\right](C_{01}P(Y = 1) - C_{11}P(Y = 1)) + C_{10}P(Y = 0) + C_{11}P(Y = 1) \\
&= \int_R p(x \mid Y = 0)(C_{00}P(Y = 0) - C_{10}P(Y = 0)) + p(x \mid Y = 1)(C_{01}P(Y = 1) - C_{11}P(Y = 1)) \, \text{d}x + C_{10}P(Y = 0) + C_{11}P(Y = 1) \\
&= \int_R p(x \mid Y = 0)P(Y = 0)(C_{00} - C_{10}) + p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) \, \text{d}x + C_{10}P(Y = 0) + C_{11}P(Y = 1) \\
\end{align}
$$
We see that the term
$$
\int_R p(x \mid Y = 0)P(Y = 0)(C_{00} - C_{10}) + p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) \, \text{d}x
$$
is the only one that depends on $R$, so we will only focus on it. We will assume that $C_{10} > C_{00}$ and $C_{01} > C_{11}$, which means that the cost of making a mistake is always higher than the cost of not making a mistake. Therefore, the term $C_{00} - C_{10}$ is negative, while the term $C_{01} - C_{11}$ is positive. Therefore,
$$
\begin{align}
&\int_R p(x \mid Y = 0)P(Y = 0)(C_{00} - C_{10}) + p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) \, \text{d}x \\
&= \int_R p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) - p(x \mid Y = 0)P(Y = 0)(C_{10} - C_{00}) \, \text{d}x \\
\end{align}
$$
We need to choose which points to include in $R$ so that this integral is minimized. To do so, we assign points $x$ to $R$ when the integrand is negative and we assign them to $\bar R$ when the integrand is positive, because points in $\bar R$ do not contribute to the integral. In other words, when
$$
\begin{align}
p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) - p(x \mid Y = 0)P(Y = 0)(C_{10} - C_{00}) &< 0 \\
p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) &< p(x \mid Y = 0)P(Y = 0)(C_{10} - C_{00}) \\
\frac{p(x \mid Y = 1)}{p(x \mid Y = 0)} &< \frac{P(Y = 0)(C_{10} - C_{00})}{P(Y = 1)(C_{01} - C_{11})}
\end{align}
$$
then assign $x$ to $R$. Otherwise, when
$$
\begin{align}
p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) - p(x \mid Y = 0)P(Y = 0)(C_{10} - C_{00}) &> 0 \\
p(x \mid Y = 1)P(Y = 1)(C_{01} - C_{11}) &> p(x \mid Y = 0)P(Y = 0)(C_{10} - C_{00}) \\
\frac{p(x \mid Y = 1)}{p(x \mid Y = 0)} &> \frac{P(Y = 0)(C_{10} - C_{00})}{P(Y = 1)(C_{01} - C_{11})}
\end{align}
$$
then assign $x$ to $\bar R$. We have derived the decision rule that minimizes the risk $E_{Y,\hat Y}[C(Y,\hat Y)]$.

## Special case

Consider the special case when $C_{10} = C_{01} = 1$ and $C_{00} = C_{11} = 0$. The risk $E_{Y,\hat Y}[C(Y,\hat Y)]$ in this case is
$$
\begin{align}
E_{Y,\hat Y}[C(Y,\hat Y)] &= C_{00}P(Y = 0,\hat Y = 0) + C_{01}P(Y = 0,\hat Y = 1) + C_{10}P(Y = 1,\hat Y = 0) + C_{11}P(Y = 1,\hat Y = 1) \\
&= P(Y = 0,\hat Y = 1) + P(Y = 1,\hat Y = 0)
\end{align}
$$
which is just the probability of error. Similarly, the decision rule above that minimizes the risk is
$$
\begin{align}
\frac{p(x \mid Y = 1)}{p(x \mid Y = 0)} &\gtrless \frac{P(Y = 0)(C_{10} - C_{00})}{P(Y = 1)(C_{01} - C_{11})} \\
\frac{p(x \mid Y = 1)}{p(x \mid Y = 0)} &\gtrless \frac{P(Y = 0)}{P(Y = 1)}
\end{align}
$$
which is just the Maximum a Posteriori (MAP) decision rule. Technically, we can get the MAP rule when $C_{10} = C_{10}$, regardless of their values, and $C_{00} = C_{11}$. However, the risk function will be different from the probability of error.
