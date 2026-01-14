---
title: "150. Evaluate Reverse Polish Notation"
layout: post
tags:
  - stack
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

## Solution

We can solve this problem using a stack as follows:

1. Create an empty list called `stack`.
2. For each `token` in `tokens`:
    1. If `token` is an operator, then we must have iterated through at least two other
    tokens that are numbers. So, we pop the two top elements on the `stack` and apply
    the corresponding operator to them. We then append the result to the `stack`.
    2. Otherwise, if `token` is a number, convert it into an integer and push it onto
    the `stack`.
3. After processing all tokens, the stack contains exactly one value, the result, so
return it.

In Python, this solution can be implemented as follows:

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            # if token is an operator
            if token == "+":
                stack.append(stack.pop() + stack.pop())
            elif token == "-":
                operand2 = stack.pop()
                operand1 = stack.pop()
                stack.append(operand1 - operand2)
            elif token == "*":
                stack.append(stack.pop() * stack.pop())
            elif token == "/":
                operand2 = stack.pop()
                operand1 = stack.pop()
                stack.append(int(float(operand1) / operand2))
            # otherwise, if token is a number
            else:
                stack.append(int(token))
        return stack[0]
```
