---
title: "238. Product of Array Except Self"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/product-of-array-except-self/description/)

## Naive solution

The simplest way to solve this problem would be to loop once over the `nums` array, then
for each iteration, loop once again over the `nums` array and compute the cumulative
product of the entire array while skipping the current element.

In Python, this can be done as follows:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        output = []
        for i in range(len(nums)):
            prod = 1
            for j in range(len(nums)):
                if i == j:
                    continue
                prod *= nums[j]
            output.append(prod)
        return output
```

Let $n$ be the length of the `nums` array. We iterate over the `nums` array once, which
requires $n$ iterations. Additionally, for each element in the `nums` array, we iterate
over the `nums` array again while skipping the current element, which requires $n-1$
iterations. Hence, in total, we need $n(n-1) = n^2 - n$ iterations, which corresponds to
$O(n^2)$ time. We also only require $O(n)$ space for the `output` array.

## Better solution

For convenience, let $n[i]$ and $o[i]$ correspond to `nums[i]` and `output[i]`, respectively, and let $N$ be the length of the `nums` array.

Then, notice in the previous solution that

$$
\begin{align}
o[i] &= (n[0] \times n[1] \times \cdots \times n[i-1]) \times (n[i+1] \times n[i+2] \times \cdots \times n[N-1]) \\
&= \left(\prod_{k=0}^{i-1} n[k]\right) \times \left(\prod_{k=i+1}^{N-1} n[k]\right)
\end{align}
$$

Let $p[0] = 1$ and for $i = 1, \dots, N-1$, let $p[i] = \prod_{k=0}^{i-1} n[k]$. Similarly,
let $s[N-1] = 1$ and for $i = N-2, \dots, 0$, let $s[i] = \prod_{k=i+1}^{N-1} n[k]$.
Then, note that

$$
\begin{align}
p[i] &= \prod_{k=0}^{i-1} n[k] \\
&= \left(\prod_{k=0}^{i-2} n[k]\right) \cdot n[i-1] \\
&= p[i-1] \cdot n[i-1]
s[i] &= \prod_{k=i+1}^{N-1} n[k] \\
&= \left(\prod_{k=i+2}^{N-1} n[k]\right) \cdot n[i+1] \\
&= s[i+1] \cdot n[i+1]
\end{align}
$$

Hence, $p[i]$ can be computed for $i = 1,\dots,N-1$ via forward induction with the initial condition $p[0] = 1$ and $s[i]$ can be computed for $i = N-2, \dots, 0$ via backward induction with the terminal condition $s[N-1] = 1$. Finally, we compute $o[i]$ for $i = 0,\dots,N-1$ as $o[i] = p[i] * s[i]$.

This is done in the Python code below, where the $p$ array corresponds to a **p**refix product array and the $s$ array corresponds to a **s**uffix product array.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        for i in range(len(nums)-1):
            prefix[i+1] = prefix[i] * nums[i]
        suffix = [1] * len(nums)
        for i in range(len(nums)-1, 0, -1):
            suffix[i-1] = suffix[i] * nums[i]
        output = []
        for i in range(len(nums)):
            output.append(prefix[i] * suffix[i])
        return output
```

## Best solution


## Pseudocode



## Python code

NOTE: the code below is optimal.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        output = []
        prefix = 1
        for i in range(1, len(nums)+1):
            output.append(prefix)
            prefix *= nums[i-1]
        suffix = 1
        for i in range(1, len(nums)+1):
            output[-i] *= suffix
            suffix *= nums[-i]
        return output
```
