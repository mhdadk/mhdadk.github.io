---
title: "271. Encode and Decode Strings"
layout: post
tags:
  - two-pointers
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/encode-and-decode-strings/description/)

## Solution

To solve this problem, we need to come up with a way to separate the strings that have
been encoded into a single string.

We could try to separate the strings using a simple separator such as a comma `,` or
hash symbol `#`. However, because the strings can contain any UTF-8 character, then any
single separator we use will have the same problem: the same character used to separate
the strings can exist in one of the strings, so decoding the strings would not work.

To fix this problem, we include an additional symbol next to the separator for each string.
This symbol must be a property of the string that is next to it to ensure we don't face
the same aforementioned problem.

One possible property to use is the length of the current string, which is what we will
use in our solution. So, we encode each string as `<length of string><separator><string>`.

## Pseudocode

Encoding:
1. Initialize an empty list.
2. For each string `s` in `strs`:\
    a. Append the string that consists of the length of `s`, followed by the delimiter
    `"#"`, and then the string `s` itself to the empty list.
3. Return the encoded string.

Decoding:
1. Initialize an empty list `res` to store the decoded strings.
2. Initialize pointers `i` and `j` as `i = 0` and `j = 0`.
3. While `i < len(s)`:\
    a. Move pointer `j` to the next instance of the delimiter `"#"` using a while loop.\
    b. Extract the length of the current substring as `int(s[i : j])` (which was
       previously encoded into `s`).\
    c. Let the start and end of the substring be `j + 1` and `j + 1 + length of substring`, respectively.\
    d. Extact the substring as `s[start of substring : end of substring]` and append it to `res`.\
    e. Move pointers `i` and `j` to the end of the substring.
4. Return `res`.

Let $m$ be the sum of the lengths of all the strings.

For encoding, we require $O(m)$ time because we iterate over each character once to
build the output and computing lengths and appending separators is constant-time per
string. We also need $O(m)$ space because output storage is proportional to the total
length of the final encoded string.

For decoding, we need $O(m)$ time because the decoding pointer moves through the
encoded string linearly and without revisiting any character. We also need $O(m)$ space
because we rebuild the original list of strings, whose cumulative size is $m$.

## Python code

```python
class Solution:
    def encode(self, strs: list[str]) -> str:
        encoded = []
        for s in strs:
            encoded.append(f"{len(s)}#{s}")
        return "".join(encoded)

    def decode(self, s: str) -> list[str]:
        strs = []
        i = j = 0
        while i < len(s):
            while s[j] != "#":
                j += 1
            len_s = int(s[i:j])
            start = j + 1
            end = start + len_s
            strs.append(s[start:end])
            i = j = end
        return strs
```
