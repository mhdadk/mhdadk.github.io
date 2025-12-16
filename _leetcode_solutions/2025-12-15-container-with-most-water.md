---
title: "11. Container With Most Water"
layout: post
tags:
  - two-pointers
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/container-with-most-water/description/)

## Brute force

We can solve this problem via brute force as follows. First, let $N$ be the length of the
`heights` array and define the set $S = \{(m, n) \mid (m, n) \in \{0, \dots, N-1\}^2, m < n\}$. Then, for each $(i, j) \in S$, let

$$
A(i,j) = (j-i)\min(h_i,h_j)
$$

be the area of water contained between the $i$th and $j$th bars. Our goal is to compute

$$
A^* = \max_{(i, j) \in S} A(i,j)
$$

The simplest way to do this is to iterate over every pair $(i, j)$ in $S$, compute $A(i, j)$,
and then pick the largest area. However, this requires $O(N^2)$ iterations over the
`heights` array. Can we do better?

## Optimized solution

Note that

$$
\begin{align*}
A^* &= \max_{(i, j) \in S} A(i,j) \\
&= \max\{A(i, j) \mid (i, j) \in S\} \\
&= \max Q
\end{align*}
$$

where $Q = \{A(i, j) \mid (i, j) \in S\}$. The key insight that leads us to the optimized solution is that we can remove elements from set $Q$ without affecting $A^*$. The
following results will allow us to do this.

First, let $h_i = \texttt{heights[i]}$ for each $i = 0,\dots,N-1$. Then, if
$h_i \leq h_j$, $A(i,k) \leq A(i,j)$ for every $k$ such that $i < k < j$. In other words,
if $h_i \leq h_j$, $A(i, k)$ is dominated by $A(i, j)$ for every $k$ that is between
$i$ and $j$.

We prove this statement as follows. Suppose that $h_i \leq h_j$ such that
$A(i, j) = (j-i)\min(h_i,h_j) = (j-i)h_i$. Then, choose any $k$ such that $i < k < j$.
Hence,

$$
A(i, k) = (k-i)\min(h_i,h_k) \leq (k-i)h_i \leq (j-i)h_i = A(i,j)
$$

Similarly, if $h_i > h_j$, $A(k, j) \leq A(i,j)$ for every $k$ such that $i < k < j$ and
a similar proof follows.

These dominance results allow us to remove elements from set $Q$ as follows:

1. Pick any $(i, j) \in S$.
2. If $h_i \leq h_j$, remove all elements $A(i,k)$ from set $Q$ for $i < k < j$.
3. Otherwise, if $h_i > h_j$, remove all elements $A(k, j)$ from set $Q$ for $i < k < j$.
4. Repeat steps 1-3 as needed using different choices of $(i, j) \in S$.

Additionally, because $A(i, j)$ dominates both $A(i,k)$ and $A(k, j)$ for $i < k < j$,
we should aim to choose $(i, j) \in S$ such that $|j - i|$ is as large as possible. This
choice allows us to remove as many elements from set $Q$ as possible. Notice that $A^*$
is not removed from set $Q$ because it dominates all other $A(i, j)$.

Putting these results all together, we can solve this problem as follows:

1. Initialize two pointers `l` and `r` such that `l = 0` and `r = len(heights) - 1`.
Choosing `l` and `r` this way allows us to compute an $A(l, r)$ that dominates many
other $A(i, j)$, as shown above.
2. Initialize a running maximum `max_area`. As we eliminate $A(i, j)$ from set $Q$, we
keep track of which $A(i, j)$ was the largest we've seen so far.
3. While `l < r`:\
    a. Compute `area = min(heights[l], heights[r]) * (r - l)`.\
    b. Update the largest area seen so far by assigning `max_area = max(area, max_area)`.\
    c. If `heights[l] <= heights[r]`, we can remove all elements $A(l,k)$ from set $Q$
    for $l < k < r$. So, to move to another $A(i, j)$, we increment `l` and avoid
    decrementing `r`, since this will only lead to a smaller area.\
    d. Otherwise, if `heights[l] > heights[r]`, we can remove all elements $A(k, r)$ from
    set $Q$ for $l < k < r$. So, to move to another $A(i, j)$, we decrement `r` and avoid
    incrementing `l`, since this will only lead to a smaller area.
4. Return `max_area`.

Because we iterate over the `heights` array only once, this solution requires $O(N)$ time.
Because we do not create any new data structures that scale in size as a function of $N$,
we use $O(1)$ space.

This optimized solution can be implemented in Python as follows:
```python
class Solution:
    def maxArea(self, heights: List[int]) -> int:
        l, r = 0, len(heights) - 1
        max_area = -1
        while l < r:
            area = min(heights[l], heights[r]) * (r - l)
            max_area = max(area, max_area)
            if heights[l] < heights[r]:
                l += 1
            else:
                r -= 1
        return max_area
```
