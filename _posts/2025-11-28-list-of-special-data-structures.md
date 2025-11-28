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
iterated over. This is demonstrated in [217. Contains Duplicate](/_leetcode_solutions/2025-11-23-contains-duplicate.md) and [1. Two Sum](/_leetcode_solutions/2025-11-28-two-sum.md). Specifically, rather than iterating over the array again to check if we already iterated
over a specific element, we can query the inverse array.

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

For example, a histogram is used in [242. Valid Anagram](/_leetcode_solutions/2025-11-27-valid-anagram.md).
