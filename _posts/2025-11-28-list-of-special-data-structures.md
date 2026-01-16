---
title: "A list of special data structures"
layout: post
tags:
  - data-structures
  - leetcode
---
If you have spent time grinding LeetCode, you know that certain patterns emerge. While
every problem is unique, the underlying data structures used to solve them often repeat.

In this post, I provide a curated list of data structures that appear frequently
enough in a diverse set of problems that they deserve to be formally recognized in your
toolkit.

## Inverse array (index map)

Standard arrays map an index to an item. An inverse array conceptually does the reverse:
it maps an item to its corresponding index (or indices).

In practice, an Inverse Array is almost always implemented using a hash map, where the
key is the element from the array and the value is the index (or indices) where the
element is found.

If we don't care about the index and just need to know if an element exists, a
hash set serves the same purpose.

The primary use case for an inverse array is efficient lookups. Without an inverse array,
checking if an element exists or finding its location requires scanning the array, which
is an $O(n)$ operation. With an inverse array, this becomes an $O(1)$ operation.

### Example: The "Two Sum" problem

Consider the classic problem _Two Sum_: Given an array of integers and a
`target`, return indices of the two numbers such that they add up to `target`. The
brute force approach involves looping through every pair, which has a time complexity of
$O(n^2)$.

In contrast, using the inverse array approach, we can iterate through each `num` in the
array, calculate the `complement = target - num` needed to hit the target, and then
quickly check the inverse array to see if that `complement` has already been visited.

In Python, this problem can be solved as follows:

```python
def twoSum(nums, target):
    inverse_array = {} # Map value -> index

    for i, num in enumerate(nums):
        complement = target - num
        if complement in inverse_array:
            return [inverse_array[complement], i]
        inverse_array[num] = i

```

See [1. Two Sum](/leetcode-solutions/two-sum) for details.

---

## Histogram (frequency map)

A histogram is used to count the frequency of unique elements within a collection. It
maps each unique element in a collection of items to the number of times it occurs in
the collection.

If the items in the collection are generic, we use a hash map to implement the histogram,
where keys are the unique elements and values are the counts.

If the elements are instead within a small, known range (e.g., lowercase English letters
or a finite number of integers), we can use a fixed-size array or list for better
performance. The index represents the item, and the value at that index represents the
count.

Histograms are essential for problems involving anagrams, duplicates, or substring
constraints.

### Example: Valid anagram

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`.

An anagram implies both strings possess the exact same characters with the exact same
frequencies. We build a histogram for `s` and a histogram for `t` and compare them.

In Python, this would be implemented as follows:

```python
from collections import Counter

def isAnagram(s, t):
    # Counter is a built-in Python Histogram
    return Counter(s) == Counter(t)
```

See [242. Valid Anagram](/leetcode-solutions/valid-anagram) for details.

## Inverse Histogram (frequency buckets)

While a standard histogram maps an item in a collection to the frequency of occurence
in the collection, an inverse histogram maps a frequency of occurrence in a collection
to one or more items in the collection.

Because multiple items can share the same frequency, this is not a one-to-one function.
It maps an integer (frequency) to a list of items that appear that many times.

This is typically implemented as a list of lists (often called "buckets") in Python,
where the index of the outer list represents the frequency.

This structure is the backbone of the Bucket Sort algorithm. It allows you to find items
based on their frequency in $O(n)$ time, avoiding the $O(n \log n)$ cost of sorting the
entire array.

### Example: Top $K$ frequent elements

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

Instead of sorting a dictionary by value, we map frequencies to items. We then iterate
backwards from the highest possible frequency (the length of the array) to find the
most common items.

See [347. Top K Frequent Elements](/leetcode-solutions/top-k-frequent-elements) for details.

In Python, given a collection `items` and its histogram `hist`, an inverse histogram can
be computed as follows:

```python
def compute_inv_hist(items, hist):
    # We need to create len(items) + 1 distinct lists because an item in the collection
    # can appear at most len(items) number of times, but because Python uses 0-based
    # indexing, then the last index will be len(inv_hist) - 1 NOT len(inv_hist). For
    # example, if items = [5, 5, 5, 5], then hist = {5: 4} and
    # inv_hist = [[], [], [], [], [5]].
    #
    # This also means that the first element of inv_hist will always be an empty list,
    # because each value in the histogram "hist" will always be at least 1.
    #
    # Finally, we can't write "inv_hist = [[]] * (len(items) + 1)" because this will
    # create (len(items) + 1) references to the same list, while we need (len(items) + 1)
    # distinct copies
    inv_hist = [[] for _ in range(len(items) + 1)]
    for item, freq in hist.items():
        inv_hist[freq].append(item)
    return inv_hist
```

## Monotonic stack

A monotonic stack is a specialized stack data structure that maintains its elements in a
specific sorted order, either increasing or decreasing.

Standard stacks simply push and pop. A monotonic stack adds logic before pushing a new
element. That is, when pushing this new element, if it violates the monotonic property
(e.g. the new element is bigger than the element at the top of the decreasing stack),
keep popping elements off the stack until the monotonic property is restored or the
stack is empty. Finally, push the new element.

The monotonic stack is useful for "next greater element" or "next smaller element"
problems. It allows you to find the nearest element to the left or right that is larger
or smaller than the current element in $O(n)$ time.

### Example: Daily Temperatures

See [739. Daily Temperatures](/leetcode-solutions/daily-temperatures) for details.
