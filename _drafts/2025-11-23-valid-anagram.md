---
title: "242. Valid Anagram"
layout: post
tags:
  - arrays-and-hashing
---
## [Problem statement](https://leetcode.com/problems/valid-anagram/description/)

## Naive solution

A straightforward approach is to sort both strings `s` and `t` and then compare the
results. If `s` has length $n$ and `t` has length $m$, then this takes
$O(n \log n + m \log m)$ time and uses either $O(1)$ or $O(n + m)$ space,
depending on the sorting implementation.

## Key insights

Two strings are anagrams (permutations) if and only if they contain exactly the same
characters with the same frequencies. Sorting enforces this, but counting is cheaper.

For example, “anagram” and “nagaram” match because the frequency of each letter is
identical. “rat” and “car” do not, because “rat” contains a “t” while “car” contains a
“c”.

A frequency map (a simple histogram) captures these counts:

1. Create an empty hash map.
2. Traverse `s` and increment the count for each character.
3. Traverse `t` and decrement the count for each character.

During the second pass, any character that is missing from the map or whose count drops
below zero immediately proves the strings are not anagrams. If all counts end at zero,
the strings match.

This avoids building two separate histograms and runs in $O(n + m)$ time with
$O(1)$ auxiliary space if the alphabet size is fixed (e.g., lowercase English letters).

Additionally, if `s` and `t` have different lengths, they cannot be anagrams. The length
check should be done immediately so you can return early.

## Pseudocode

Here is what this optimized solution looks like at a high level:
```
1. If strings s and t have different lengths, return False .
2. Compute the histogram for s.
3. Iterate over the letters of t while decrementing the the histogram computed in step 2
accordingly.
4. Check that the resulting histogram has all zeros for values.
```

Assuming that strings `s` and `t` have the same length, we iterate over them once, which
requires $O(n)$ time ($n$ is the length of strings `s` and `t`).

TODO: continue

hash set can have at most $n$ integers. So, this optimized solution requires $O(n)$
iterations and $O(n)$ integers to be stored.

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

