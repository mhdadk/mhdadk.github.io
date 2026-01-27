---
title: "128. Longest Consecutive Sequence"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/longest-consecutive-sequence/description/)

## Brute force

The simplest way to solve this problem is to first convert the `nums` array into a hash
set `nums_unique` that contains the unique elements of `nums` to count the lengths of
consecutive sequences.

Then, we simply let each `num` in `nums` be the start of a sequence and check if the
succeeding numbers, `num + 1`, `num + 2`, and so on, exist in `nums_unique`. If they
do, we count how many successive numbers exist in `nums_unqique`, which is equivalent
to the length of this consecutive sequence with a starting number of `num`.

We then compute the maximum length of all of these conscutive sequences. In Python, this
brute force solution would be implemented as follows:

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        max_len = 0
        nums_unique = set(nums)
        for num in nums:
            streak, curr = 0, num
            while curr in nums_unique:
                streak += 1
                curr += 1
            max_len = max(max_len, streak)
        return max_len
```

Even though this method works, it does unnecessary repeated work because many sequences
get recomputed multiple times. Can we do better?

## Solution

We can solve this problem more efficiently by noticing that if a `num` in `nums` is part
of a longer consecutive sequence, then the length of the consecutive sequence starting
from `num` will always be shorter or equal in length to the sequence starting from
`num - 1`, `num - 2`, and so on.

So, to save on computation, for each `num` in `nums`, we first check if `num` is part
of a longer consecutive sequence. This is the case if `num - 1` is in `nums_unique`.

If `num` is not part of a longer consecutive sequence, then we consider `num` to be part
of a new sequence. We then proceed as before by counting the length of the consecutive
sequence starting from `num` and determine the longest length of these consecutive
sequences.

In Python, this solution can be implemented as follows:

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        nums_unique = set(nums)
        max_length = 0
        for num in nums_unique:
            # if num is not part of a longer consecutive sequence and is the start of
            # a new sequnce, measure the length of the consecutive sequence starting
            # from num
            if (num - 1) not in nums_unique:
                length = 1
                while (num + length) in nums_unique:
                    length += 1
                max_length = max(length, max_length)
        return max_length
```
