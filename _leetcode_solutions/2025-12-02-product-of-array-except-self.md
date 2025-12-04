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
\begin{align*}
o[i] &= (n[0] \cdot n[1] \cdot \cdots \cdot n[i-1]) \cdot (n[i+1] \cdot n[i+2] \cdot \cdots \cdot n[N-1]) \\
&= \left(\prod_{k=0}^{i-1} n[k]\right) \cdot \left(\prod_{k=i+1}^{N-1} n[k]\right)
\end{align*}
$$

Let $p[0] = 1$ and for $i = 1, \dots, N-1$, let $p[i] = \prod_{k=0}^{i-1} n[k]$. Similarly,
let $s[N-1] = 1$ and for $i = N-2, \dots, 0$, let $s[i] = \prod_{k=i+1}^{N-1} n[k]$.
Then, note that

$$
\begin{align*}
p[i] &= \prod_{k=0}^{i-1} n[k] \\
&= \left(\prod_{k=0}^{i-2} n[k]\right) \cdot n[i-1] \\
&= p[i-1] \cdot n[i-1] \\
s[i] &= \prod_{k=i+1}^{N-1} n[k] \\
&= \left(\prod_{k=i+2}^{N-1} n[k]\right) \cdot n[i+1] \\
&= s[i+1] \cdot n[i+1]
\end{align*}
$$

Hence, $p[i]$ can be computed for $i = 1,\dots,N-1$ via forward induction with the initial condition $p[0] = 1$ and $s[i]$ can be computed for $i = N-2, \dots, 0$ via backward induction with the terminal condition $s[N-1] = 1$. Finally, we compute $o[i]$ for $i = 0,\dots,N-1$ as $o[i] = p[i] \cdot s[i]$.

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
        output = [1] * len(nums)
        for i in range(len(nums)):
            output[i] = prefix[i] * suffix[i]
        return output
```

## Best solution

Although the solution in the previous section requires $O(N)$ time, where $N$ is the length
of the `nums` array, it still requires $O(N)$ space due to the `prefix`, `suffix`, and `output` arrays.
If we ignore the `output` array, can we get rid of the `prefix` and `suffix` arrays altogether to save space?

The crucial insights for reducing the space required are:

1. We only need either the `prefix` array or the `suffix` array, but not both.
2. Once we've chosen which array to keep, the other product can be computed using a
running variable. For example, if we choose to keep the `suffix` array, the prefix product
can be computed using a running variable.
3. We can reuse the same `prefix` (or `suffix`) array to store the final output values.

Without loss of generality, suppose we choose to keep the `prefix` array. We first
compute the elements of this array as before:

```python
prefix = [1] * len(nums)
for i in range(len(nums)-1):
    prefix[i+1] = prefix[i] * nums[i]
```

We then compute the suffix product using a running variable and compute the final
output value:

```python
suffix = 1
for i in range(len(nums)-1, -1, -1):
    # compute output value and put it into prefix[i]
    prefix[i] = prefix[i] * suffix
    # update suffix running variable
    suffix = suffix * nums[i]
```

We are essentially performing a forward and backward pass over the `prefix` array.
Finally, we return the `prefix` array as the `output` array. The full Python code is
given below.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        for i in range(len(nums)-1):
            prefix[i+1] = prefix[i] * nums[i]
        suffix = 1
        for i in range(len(nums)-1, -1, -1):
            # compute output value and put it into prefix[i]
            prefix[i] = prefix[i] * suffix
            # update suffix running variable
            suffix = suffix * nums[i]
        return prefix
```
