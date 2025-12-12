---
title: "125. Valid Palindrome"
layout: post
tags:
  - two-pointers
  - leetcode-easy
---
## [Problem statement](https://leetcode.com/problems/valid-palindrome/description/)

## Solution

We solve this problem by verifying that the input string `s` reads the same forward and backward after ignoring non-alphanumeric characters and their case. The most direct way
to do this is with two pointers, one starting at the left and another starting at the
right.

The psuedocode for this solution is as follows:

1. Set `l = 0` and `r = length(s) - 1`. These are the two pointers.
2. While the left pointer `l` is smaller than the right pointer `r`:\
    a. If `s[l]` is not an alphanumeric character, keep moving `l` to the right until
    `s[l]` is an alphanumeric character. Make sure that `l < r` while doing this.\
    b. If `s[r]` is not an alphanumeric character, keep moving `r` to the left until
    `s[r]` is an alphanumeric character. Make sure that `l < r` while doing this.\
    c. If the lowercase version of `s[l]` is equal to the lowercase version of `s[r]`,
    then return move `l` to the right by `1` and move `r` to the left by `1`.\
    d. Otherwise, return `False`, which means that `s` is not a valid palindrome.
3. Return `True`, which implies that `s` is a valid palindrome.

In Python, this can be implemented as:

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1
        while l < r:
            # move the left pointer to the first alphanumeric
            # character
            while not s[l].isalnum() and l < r:
                l += 1
            # move the right pointer to the first alphanumeric
            # character
            while not s[r].isalnum() and l < r:
                r -= 1
            if s[l].lower() == s[r].lower():
                l += 1
                r -= 1
            else:
                return False
        return True
```

Let $n$ be the length of the input string `s`. Because

* every character is visited at most once by either pointer,
* skipped characters are processed in constant time, and
* each pointer only moves forward or backward,

this solution requires $O(n)$ time. Additionally, because only two integer variables
(the pointers) are used and there are no copies of the input string `s`, this solution
requires $O(1)$ space.
