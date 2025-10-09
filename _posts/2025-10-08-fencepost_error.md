---
title: Fencepost errors
layout: post
tags:
  - arithmetic
  - combinatorics
---
We describe the fencepost error, what it is, and how to avoid it. More details on the fencepost error can be found [here](https://betterexplained.com/articles/learning-how-to-count-avoiding-the-fencepost-problem/).

## Formal explanation
Consider the integers $a$ and $b$ such that $b > a$. What is the length of the sequence $a, \dots, b$? The length of this sequence can be determined by finding a bijection between this sequence and the sequence $1,\dots,x$ of natural numbers, where $x \in \mathbb N$ is the length of the sequence and $x > 1$, as follows $\frac{3}{4}$:

$$
\begin{align}
&a,\dots,b \\
&0,\dots,(b-a) \\
&1,\dots,(b-a) + 1
\end{align}
$$

Hence, the length of the sequence $a,\dots,b$ is $(b-a) + 1$.

Similarly, consider the integers $a$ and $b$ such that $b > a$ and $a \neq 0$. Suppose that we want to compute $\frac{b}{a}$, but we wanted to include the dividend $b$ in the calculation as an extra "post" (see the informal explanation in the next section). This is equivalent to computing the length of the sequence $r, 1 \cdot a + r, 2 \cdot a + r, \dots, q \cdot a + r$, where
* $q$ is the integer quotient obtained after dividing $b$ by $a$.
* $r$ is the remainder obtained after dividing $b$ by $a$.
* $q \cdot a + r = b$.

As done above, to compute the length of this sequence, we transform it into the sequence $1,\dots,x$ of natural numbers, where $x \in \mathbb N$ is the length of the sequence and $x > 1$, as follows:

$$
\begin{align}
&r, 1 \cdot a + r, 2 \cdot a + r, \dots, q \cdot a + r \\
&0, 1 \cdot a, 2 \cdot a, \dots, q \cdot a \\
&0, 1, 2, \dots, q \\
&1, 2, 3, \dots, q + 1
\end{align}
$$

Hence, the length of the sequence $r, 1 \cdot a + r, 2 \cdot a + r, \dots, q \cdot a + r$ is $q + 1$. Because $q \cdot a + r = b$, we can also write $q + 1$ as $\frac{b-r}{a} + 1$.
## Informal explanation
Consider the sequence of numbers $2,3,4,5$. What is the length of this sequence? The answer depends on how "length" is defined. If the length of a sequence is defined as the amount of numbers in the sequence, then the answer is $4$. This is equivalent to $5 - 2 + 1 = 4$.

If the length of a sequence is defined as the number of additions by $1$ needed to get from the first number in the sequence to the last, then the answer is $3$. This is equivalent to $5 - 2 = 3$.

> [!IMPORTANT]  
> Hence, the result of $a - b$, where $a$ and $b$ are integers, can be interpreted as "the number of increments by $1$ needed to go from $b$ to $a$". Note that this interpretation does not include either $a$ or $b$. This is why we need to add $1$ to the final answer if needed.

We see from this thought experiment that the subtraction operation counts the number of "gaps" between numbers in a sequence and not the numbers themselves. 
<figure>
<img src="figures/fencepost.png">
<figcaption>Fig. 1</figcaption>
</figure>
To see why, note from Fig. 1 that for every integer, we associate the interval between that integer and the integer after it to the integer itself. That is, there is a bijection between the integer and the interval between the integer and integer after it. This means that there will always be a number remaining at the end of a sequence.

Hence, the result of subtraction will always result in the number of gaps between the numbers and not the number of numbers. To compute the number of numbers, we need to add $1$ to the subtraction result.

As an example, consider the classic fencepost problem:

> If you build a fence that is $100$ feet long with posts spaced $10$ feet apart, how many posts do you need?

If we divide $100$ feet of fence by $10$ feet per fence section, then we will obtain $10$ fence sections and **not** $10$ posts. That is, the division operation also computes the number of gaps (or fences) between numbers and not the numbers themselves.

Using Fig. 1, we can match each of the $10$ fence sections to the post on its left, which means there is $1$ post remaining at the very end that needs to be counted, which is similar to how the number $8$ in Fig. 1 also needs to be counted.

To summarize, when counting how many things there are of an object, do the following:
1. Ask yourself: is the object in question a fence or a post (i.e. is what we are counting a number or a gap between numbers)?
2. Ask yourself: is the desired quantity a fence or a post?
3. Compute the desired quantity using the information given. Note that both subtraction and division will lead to fence quantities and not posts.
4. If the desired quantity is a fence, then we are done. Otherwise, if the desired quantity is a post, then we must add $1$ to the final answer.


