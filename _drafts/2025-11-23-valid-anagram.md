---
title: "242. Valid Anagram"
layout: post
tags:
  - arrays-and-hashing
---
## [Problem statement](https://leetcode.com/problems/valid-anagram/description/)

## Naive solution

A simple approach is to sort both strings and compare the results. For strings `s` and `t` of lengths $n$ and $m$, this runs in $O(n \log n + m \log m)$ time and uses either $O(1)$ or $O(n + m)$ extra space depending on the sorting method.

## Key insights

Two strings are anagrams if and only if they contain the same characters with identical frequencies. Sorting enforces this, but counting is cheaper.

For example, `anagram` and `nagaram` match because every letter appears the same number of times. In contrast, `rat` and `car` do not since one has a `t` and the other has a `c`.

A frequency map (histogram) captures this directly:

1. Count each character in `s`.
2. Decrease the corresponding counts while scanning `t`.
3. Any missing or negative count means they differ.
4. If all counts return to zero, the strings are anagrams.

If the alphabet is fixed (e.g., 26 lowercase letters), this uses $O(1)$ space and $O(n)$ time. A length mismatch between `s` and `t` can be rejected immediately.

## Pseudocode

```
1. If len(s) != len(t): return False
2. Build a frequency map from characters in s
3. For each character in t:
       decrement its count
       if count < 0: return False
4. Return True
```

## Python code

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        # Create histogram for 26 lowercase English letters
        hist = [0] * 26

        for ch in s:
            hist[ord(ch) - ord('a')] += 1
        for ch in t:
            i = ord(ch) - ord('a')
            hist[i] -= 1
            if hist[i] < 0:
                return False

        return True
```

You will notice in the Python code above that the histogram was implemented using a
list and not a hash map. This is because the 26 lowercase English letters have corresponding
ASCII representations that can be used as indices for the list.

Specifically, the letters `a` and `z` correspond to 97 and 122, respectively, in ASCII.
