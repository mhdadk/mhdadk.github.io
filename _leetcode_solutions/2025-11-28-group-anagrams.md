---
title: "49. Group Anagrams"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/group-anagrams/description/)

## Naive solution

The simplest approach to solving this problem is to sort each string and use the sorted
result as the key in a hash map that collects anagrams.

In Python, this would look like

```python
for word in strs:
    key = ''.join(sorted(word))
    map[key].append(word)
return list(map.values())
```

This approach relies on sorting each string, making the runtime roughly
$O(n \cdot k log k)$, where $n$ is the number of strings to iterate over and $k$ is the
length of the longest string.

## Key insights

To solve this problem more efficiently, we make use of the [inverse array](/blog/list-of-special-data-structures#inverse-array) and [histogram](/blog/list-of-special-data-structures#histogram) data structures.

Unlike the naive method, the histogram-based solution avoids sorting. Building a
histogram for each string takes $O(k)$ time, and we do this for $n$ strings, making the
total runtime $O(n \cdot k)$.

Hash map insertion and lookup both average to $O(1)$, so no additional overhead
dominates the cost.

## Pseudocode

```
1. Initialize an empty hash map called anagrams.
2. For each string in strs:
    a. Compute the string's histogram (i.e. how often each unique letter in the string occurs).
    b. If this histogram is not already a key in the anagrams hash map, add this histogram
       as a key and let its corresponding value be a list that contains the string.
    c. Otherwise, append the string to the existing list of strings.
3. Return all the values of the anagrams hash map as a list of lists of strings.
```

## Python code

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        anagrams = {}
        for i in range(len(strs)):
            # Build histogram
            hist = [0] * 26
            for ch in strs[i]:
                hist[ord(ch) - ord('a')] += 1
            # If histogram is a key, append string to existing list
            hist = tuple(hist)
            if hist in anagrams:
                anagrams[hist].append(strs[i])
            else: # Otherwise, create the list
                anagrams[hist] = [strs[i]]
        return list(anagrams.values())
```
