---
title: "217. Contains Duplicate"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-easy
---
## [Problem statement](https://leetcode.com/problems/contains-duplicate/description/)

## Naive solution

Let $n$ be the length of the `nums` array. The simplest way to solve this problem is to
iterate over the `nums` array once, and then for each integer in the `nums` array, iterate
over the `nums` array again, ensuring to skip the current integer, to check if it contains
the same integer.

So, we will require $n$ iterations over the `nums` array, and for each iteration, we
perform $n-1$ more iterations. Hence, we require $n(n-1) = n^2 - n$, or $O(n^2)$, iterations.

## Key insights

The naive solution leaves performance on the table by not remembering each integer as we iterate
over the `nums` array. Specifically, by keeping track of which integers we have already
iterated over, we can reduce the number of iterations required to detect if there is a
duplicate.

To keep track of which integers we have already iterated over, we initialize an empty hash
set, which offers $O(1)$ look-up and $O(1)$ insertion costs.

Then, we iterate over the `nums` array once and for each integer, we check if the integer
is already in the hash set. If it is, we return `true`. Otherwise, we store the current
integer in the hash set and continue iterating over the `nums` array.

If we reach the end of the `nums` array, then there must have been no duplicates, so we
return `false`.

## Pseudocode

Here is what this optimized solution looks like:
```
1. hash_set = {}
2. For each integer in nums,
  a. If integer in hash_set, return true.
  b. Else, add integer to hash_set.
3. return false
```

We iterate over the `nums` array once, which requires $n$ iterations. Additionally, the hash set can have at most $n$ integers. So, this optimized solution requires $O(n)$ iterations and $O(n)$ integers to be stored.

## Python code

```python
class Solution:
  def containsDuplicate(self, nums: List[int]) -> bool:
    seen = set()
    for num in nums:
      if num in seen:
        return True
      seen.add(num)
    return False
```

