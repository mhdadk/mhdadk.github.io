---
title: "128. Longest Consecutive Sequence"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/longest-consecutive-sequence/description/)

The key insight needed to solve this problem is that a consecutive sequence will always
look like `num, num + 1, num + 2, ..., num + X`, where the first integer `num` is one of
the unique integers in the `nums` array and `X + 1` is the length of this consecutive
sequence.

Hence, to find the length of the longest consecutive sequence in the `nums` array, we
determine what `X + 1` is for each unique `num` in the `nums` array. We do so as follows:

1. Because the `nums` array can contain duplicates, convert it into a hash set to
determine its unique elements. Additionally, a hash set offers $O(1)$ look-up costs.
Call the resulting hash set `nums_unique`.
2. Initialize a running variable `max_length` to record the length of the longest
consecutive sequence amongst all other consecutive sequences.
3. For each `num` in `nums_unique`:\
    a. If `num - 1` is in `nums_unique`, then we will skip this `num` and move on to the
    next one. This is because if `num - 1` is in `nums_unique`, then `num` is part of
    a larger sequence, so counting from `num` onwards will always lead to a shorter
    sequence. Hence, we skip this `num` to avoid the extra iterations. For example,
    if `nums = [1,2,3,4,5]`, then without this condition, we would check the sequences
    `(2,3,4,5),(3,4,5),(4,5),` and `(5)` when we already know that `(1,2,3,4,5)` is the
    longest sequence.\
    b. Otherwise, initialize a variable `length` to `1`.\
    c. Keep incrementing `length` by `1` until `num + length` is not in `nums_unique`.\
    d. Set `max_length` to `max(max_length, length)` to update it.
4. Return `max_length`.

If $n$ is the length of the `nums` array, then this solution requires $O(n)$ time in the
worst case (because we iterate over the `nums` array once). This solution will also
require $O(n)$ space due to the hash set `nums_unique`.

Here is what this solution looks like in Python:

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums_unique = set(nums)
        max_length = 0
        for num in nums_unique:
            if (num - 1) in nums_unique:
                continue
            length = 1
            while (num + length) in nums_unique:
                length += 1
            max_length = max(length, max_length)
        return max_length
```
