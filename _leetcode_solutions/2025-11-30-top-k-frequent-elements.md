---
title: "347. Top K Frequent Elements"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/top-k-frequent-elements/description/)

## Naive solution

The simplest approach to solving this problem consists of 3 steps:

1. Compute the histogram `hist` for `nums`.
2. Sort the `(num, freq)` pairs in `hist` by `freq` in descending order.
3. Return the first `k` elements of the sorted result.

This solution can be implemented in Python as follows:

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Build the histogram
        hist = {}
        for num in nums:
            hist[num] = 1 + hist.get(num, 0)
        # Sort the (num, freq) pairs in the histogram by "freq" in descending
        # order
        x = []
        for num, freq in hist.items():
            x.append((freq, num))
        x.sort(reverse=True)
        # Return the first k elements of the sorted histogram
        l = []
        for i in range(k):
            l.append(x[i][1])
        return l
```

Let $n$ be the length of the `nums` array. We first iterate over the `nums` array,
requiring $n$ random accesses. Then, we iterate over the histogram, which, in the worst
case (when all elements of `nums` are unique), also requires $n$ random accesses.

Next, we sort the histogram, which, in the worst case, requires $O(n \log n)$ operations.
Finally, we iterate over the first `k` elements of the sorted histogram, which requires
$k$ random accesses and $n$ random accesses in the worst case.

Hence, this solution requires $O(n \log n)$ operations in the worst case.

In terms of space, we require $O(n)$ space in the worst case, since the histogram, the
sorted histogram, and the list containing the top $k$ most frequent elements all contain
$O(n)$ elements in the worst case.

## Key insights

We make use of an [inverse histogram](/blog/list-of-special-data-structures#inverse-histogram)
to efficiently determine the top $K$ most frequent elements of the `nums` array.

## Pseudocode

```
1. Compute the histogram hist for the nums array that maps each unique integer in the
nums array to its frequency of occurrence.
2. Compute the inverse histogram inv_hist of the histogram hist that maps each frequency
of occurrence to the list of integers in the nums array that occur that many times.
3. Create an empty list called result.
4. Traverse inv_hist backwards (since the most frequent items will appear at the end of
inv_hist) while appending each element of the non-empty lists in inv_hist to result.
5. Stop appending to result when it has a length of k and return it.
```

Let $n$ be the length of the `nums` array. Computing the histogram and inverse histogram
each take $O(n)$ time. Also, traversing `inv_hist` backwards will take at most $O(n)$ time
in the worst case. Hence, this solution requires $O(n)$ time.

In the worst case, when $k = n$, $O(n)$ space will be required.

## Python code

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Build the histogram
        hist = {}
        for num in nums:
            hist[num] = 1 + hist.get(num, 0)
        # Build the inverse histogram
        inv_hist = [[] for _ in range(len(nums) + 1)]
        for num, freq in hist.items():
            inv_hist[freq].append(num)
        result = []
        for freq in range(len(inv_hist) - 1, 0, -1):
            for num in inv_hist[freq]:
                result.append(num)
                if len(result) == k:
                    return result
```
