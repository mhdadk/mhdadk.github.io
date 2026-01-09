---
title: "20. Valid Parentheses"
layout: post
tags:
  - stack
  - leetcode-easy
---
## [Problem statement](https://leetcode.com/problems/valid-parentheses/description/)

## Brute force

We can solve this problem in a brute force manner by repeatedly removing all pairs of
`()`, `{}`, and `[]`. For example consider the valid string `"{[()]}"`. We first remove
`()` to be left with `"{[]}"`. We then remove `[]` to be left with `"{}"`. Finally, we
remove `{}` to be left with `""`. This empty string indicates that the string is valid.

In contrast, consider the invalid string `"{[()}]"`. We first remove `()` to be left
with `"{[}]"`. In this remaining string, there are no instances of `()`, `{}`, or `[]`,
so we cannot remove any more pairs of parentheses. Hence, this is an invalid string.

In Python, this brute force solution would be implemented as

```python
class Solution:
    def isValid(self, s: str) -> bool:
        while '()' in s or '{}' in s or '[]' in s:
            s = s.replace('()', '')
            s = s.replace('{}', '')
            s = s.replace('[]', '')
        return s == ''
```

In the worst case, `s = "()()()()..."`, so we will require $n/2 = O(n)$ replacements.
Each replacement requires $O(n)$ time to complete since strings in Python are immutable
and they need to be rebuilt and so the total time complexity of this solution is $O(n^2)$.

Additionally, if `s` is an invalid string that cannot be reduced in length at all, then
we will require $O(n)$ space to operate on the string in the worst case.

Can we do better?

## Optimized solution

We can solve this problem in $O(n)$ time by noting that a stack can be used to match
pairs of parentheses together. For example, consider the input string `s = "{[()]}"`.
By scanning through this string from left to right, we observe the folloowing sequence
of substrings:

1. `"{"`
2. `"{["`
3. `"{[("`
4. `"{[()"`
5. `"{[()]"`
6. `"{[()]}"`

In step 4, the first pair of parentheses, `()`, are matched, so we can remove them. Then,
this leaves us with the substring in step 2. We then append `]` to the substring in step 2
to obtain the string `"{[]"`.

We can remove the pair `[]` to obtain the substring in step 1. Finally, we append `}` to
the substring in step 1 to be left with the string `"{}"`. We remove this final pair of
parentheses to obtain the empty string `""`, at which point we can conclude that the
input string is valid.

In pseudocode, this solution proceeds as follows:

1. Initialize an empty list `stack` that represents a stack.
2. For each character `c` in the input string `s`:
    1. If `stack` is empty or if `c` is not a delimiter (i.e. it is not one of
    `)`, `}`, or `]`), append `c` to the `stack`.
    2. Otherwise, if `c` is a delimiter , check if `c` is the closing parenthesis for
    the character on the top of the `stack`. If it isn't, return `False`. Otherwise, remove
    the character that is on top of the stack.
3. Return `True` if the `stack` is empty and `False` otherwise.

In Python, this solution can be implemented as follows:

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        delimiters = {")" : "(", "}" : "{", "]" : "["}
        for c in s:
            # if the stack is empty or if c is not a delimiter
            if len(stack) == 0 or c not in delimiters:
                stack.append(c)
            # if c is a delimiter
            elif c in delimiters:
                if stack[-1] != delimiters[c]:
                    return False
                else:
                    stack.pop()
        return len(stack) == 0
```

We iterate only once over the input string `s`, and during each iteration, we perform
all operations in $O(1)$ time. Hence, the time complexity of this solution is $O(n)$
where $n$ is the length of the input string $s$.

The space complexity is still $O(n)$ because the stack will scale linearly with the size
of the input string `s` in the worst case.
