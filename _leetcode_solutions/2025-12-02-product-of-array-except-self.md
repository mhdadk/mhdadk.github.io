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

TODO: continue

We can reduce the time

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        prefix = [1] * (len(nums) + 1)
        suffix = [1] * (len(nums) + 1)
        for i in range(1, len(nums)+1):
            prefix[i] = prefix[i-1] * nums[i-1]
            suffix[-i-1] = suffix[-i] * nums[-i]
        output = []
        for i in range(len(nums)):
            output.append(prefix[i] * suffix[i+1])
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
