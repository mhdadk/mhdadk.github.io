---
title: "A list of special data structures"
layout: post
tags:
  - data-structures
  - leetcode
---
We provide a list of data structures that serve specific purposes when solving LeetCode
problems.

These data structures are used often enough in a diverse set of problems that they
deserve to be formally recognized.

## Inverse array

Given an array of arbitrary items, this array maps a given index to a corresponding
item in the array. The inverse of this array maps the item to its corresponding index.

An inverse array can be used to efficiently recall the items in an array that were already
iterated over. This is demonstrated in [217. Contains Duplicate](/leetcode-solutions/valid-anagram) and [1. Two Sum](/leetcode-solutions/two-sum). Specifically, rather than iterating over the array again to check if we already iterated over a specific element, we can query the inverse array.

An inverse array is most often implemented using a hash map, where the keys are the elements
of an array and the values are the indices corresponding to these elements. If we are not
interested in the indices corresponding to these elements, then we can use a hash set instead.

## Histogram

Given an array of arbitrary items, we can use a histogram to count how man times each
unique element of this array occurs. That is, a histogram maps each unique element in
the array to the number of times it occurs in the array.

A histogram can be implemented using a hash map, where the keys are the unique elements
of the array and the values are the counts of the corresponding key.

If the elements of the array are integers, then the histogram can be implemented
using a list, where the indices of the list are the unique integers in the array and the
values corresponding to these indices are their counts.

Note that a hash set can be thought of as a special case of a histogram, where the keys
are the elements of the hash set and the values are all $1$.

For example, a histogram is used in [242. Valid Anagram](/leetcode-solutions/valid-anagram).

## Inverse histogram

A histogram maps an item in a collection to its frequency of occurrence in the collection.
An inverse histogram, on the other hand, maps the frequency of occurence to multiple items
in the collection.

An inverse histogram is useful when attempting to determine which items in a collection
occur a certain number of times. For example, an inverse histogram is used in
[347. Top K Frequent Elements](/leetcode-solutions/top-k-frequent-elements).

An inverse histogram is often implemented using a list, where the index is the
frequency of occurrence and the corresponding value is a list that contains the items in
the collection that occur `<corresponding index>` many times in the collection.

Note that, in contrast to the histogram, an inverse histogram is not a function, since
it maps an index to several values.

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
