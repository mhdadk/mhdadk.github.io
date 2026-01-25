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
&= \left(\prod_{k=0}^{i-1} n[k]\right) \cdot \left(\prod_{k=i+1}^{N-1} n[k]\right) \\
&= p[i] \cdot s[i]
\end{align*}
$$

where $p[i] = \prod_{k=0}^{i-1} n[k]$ for $i = 1, \dots, N-1$ and $p[0] = 1$ and
$s[i] = \prod_{k=i+1}^{N-1} n[k]$ for $i = N-2, \dots, 0$ and $s[N-1] = 1$. Then, note
that

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

Without loss of generality, suppose we choose to reuse the `prefix` array. Then, we can
first get rid of the `output` array in the previous solution as follows:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        for i in range(len(nums)-1):
            prefix[i+1] = prefix[i] * nums[i]
        suffix = [1] * len(nums)
        for i in range(len(nums)-1, 0, -1):
            suffix[i-1] = suffix[i] * nums[i]
        for i in range(len(nums)):
            prefix[i] = prefix[i] * suffix[i]
        return prefix
```

Then, we replace the `suffix` array with a running variable:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        for i in range(len(nums)-1):
            prefix[i+1] = prefix[i] * nums[i]
        suffix = 1
        for i in range(len(nums)-1, 0, -1):
            suffix = suffix * nums[i]
        for i in range(len(nums)):
            prefix[i] = prefix[i] * suffix
        return prefix
```

We then merge the two `for` loops at the end together to obtain:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * len(nums)
        for i in range(len(nums)-1):
            prefix[i+1] = prefix[i] * nums[i]
        suffix = 1
        for i in range(len(nums)-1, -1, -1):
            prefix[i] = prefix[i] * suffix
            suffix = suffix * nums[i]
        return prefix
```

We are essentially performing a forward and backward pass over the `prefix` array. This
solution requires $O(N)$ time and $O(1)$ space.
