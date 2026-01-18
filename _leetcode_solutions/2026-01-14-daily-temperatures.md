---
title: "739. Daily Temperatures"
layout: post
tags:
  - stack
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/daily-temperatures/description/)

## Brute force

We can solve this problem in a brute force manner by iterating over the `temperatures`
array once, and then for each iteration, we iterate again through the rest of the
array to find the number of days after the `i`th day before a warmer temperature appears.
In Python, this would be implemented as follows:

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        result = [0] * len(temperatures)
        for i in range(len(temperatures)):
            j = i + 1
            while j < len(temperatures) and temperatures[j] <= temperatures[i]:
                j += 1
            if j < len(temperatures):
                result[i] = j - i
        return result
```

This solution requires $O(n)$ time and $O(1)$ space, where $n$ is the length of the
`temperatures` array. Can we do better?

## Optimized solution

Unfortunately, this is one of those LeetCode problems where the optimized solution cannot
easily be derived by optimizing the brute force solution. Instead, we have to know a
trick to be used in these situations where we are looking for the next greater element
or next smaller element in an array.

Specifically, we will use a monotonic stack. A monotonic stack is a specialized stack
data structure that maintains its elements in a specific sorted order, either increasing
or decreasing.

Standard stacks simply push and pop. A monotonic stack adds logic before pushing a new
element. That is, when pushing this new element, if it violates the monotonic property
(e.g. the new element is bigger than the element at the top of the decreasing stack),
keep popping elements off the stack until the monotonic property is restored or the
stack is empty. Finally, push the new element.

The monotonic stack is useful for "next greater element" or "next smaller element"
problems. It allows you to find the nearest element to the left or right that is larger
or smaller than the current element in $O(n)$ time.

In our case, because we are interested in the next greater element in the `temperatures`
array, we will use a monotonic decreasing stack.

In Python, the optimized solution for this problem can be implemented as follows:

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        result = [0] * len(temperatures)
        stack = []
        for i in range(len(temperatures)):
            while len(stack) > 0 and temperatures[stack[-1]] < temperatures[i]:
                idx = stack.pop()
                result[idx] = i - idx
            stack.append(i)
        return result
```

To better understand how this solution works, it helps to walk through an example.
Consider the input `temperatures = [30,38,30,36,35,40,28]`. Here is how this optimized
solution evolves:

1. `result = [0] * len(temperatures) = [0] * 7 = [0, 0, 0, 0, 0, 0, 0]`.
2. `stack = []`.
3. For `i = 0`:
    1. The while loop is not executed because `len(stack) == 0`.
    2. `stack.append(0)` such that `stack = [0]`.
4. For `i = 1`:
    1. The while loop executes because `len(stack) == 1` and `temperatures[stack[-1]] < temperatures[1] := 30 < 38`.
        1. `idx = 0` and `stack = []`.
        2. `result[0] = 1 - 0 = 1` such that `result = [1, 0, 0, 0, 0, 0, 0]`.
        3. The while loop stops executing because `len(stack) == 0`.
    2. `stack.append(1)` such that `stack = [1]`.
5. For `i = 2`:
    1. The while loop does not execute because `len(stack) == 1` but `temperatures[stack[-1]] < temperatures[2] := 38 < 30`.
    2. `stack.append(2)` such that `stack = [1, 2]`.
6. For `i = 3`:
    1. The while loop executes because `len(stack) == 2` and `temperatures[stack[-1]] < temperatures[3] := 30 < 36`.
        1. `idx = 2` and `stack = [1]`.
        2. `result[2] = 3 - 2 = 1` such that `result = [1, 0, 1, 0, 0, 0, 0]`.
    2. The while loop stops executing because `len(stack) == 1` but `temperatures[stack[-1]] < temperatures[3] := 38 < 36`.
    3. `stack.append(3)` such that `stack = [1, 3]`.
7. For `i = 4`:
   1. The while loop does not execute because `len(stack) == 2` but `temperatures[stack[-1]] < temperatures[4] := temperatures[3] = 36 < 35`.
   2. `stack.append(4)` such that `stack = [1, 3, 4]`.
8. For `i = 5`:
   1. The while loop executes because `len(stack) == 3` and `temperatures[stack[-1]] < temperatures[5] := 35 < 40`.
       1. `idx = 4` such that `stack = [1, 3]`.
       2. `result[4] = 5 - 4 = 1` such that `result = [1, 0, 1, 0, 1, 0, 0]`.
   2. The while loop executes again because `len(stack) == 2` and `temperatures[stack[-1]] < temperatures[5] := 36 < 40`.
       1. `idx = 3` such that `stack = [1]`.
       2. `result[3] = 5 - 3 = 2` such that `result = [1, 0, 1, 2, 1, 0, 0]`.
   3. The while loop executes again because `len(stack) == 1` and `temperatures[stack[-1]] < temperatures[5] := 38 < 40`.
       1. `idx = 1` such that `stack = []`.
       2. `result[1] = 5 - 1 = 4` such that `result = [1, 4, 1, 2, 1, 0, 0]`.
   4. The while loop stops executing because `len(stack) == 0`.
   5. `stack.append(5)` such that `stack = [5]`.
9. For `i = 6`:
   1. The while loop does not execute because `len(stack) == 1` but `temperatures[stack[-1]] < temperatures[6] := 40 < 28`.
   2. `stack.append(6)` such that `stack = [5, 6]`.
10. We have now reached the end of the `for` loop. At this stage, `stack = [5, 6]` because
the last two elements of `temperatures` do not have warmer future days. Also,
`result = [1, 4, 1, 2, 1, 0, 0]`, as desired.

This optimized solution requires $O(n)$ time and $O(n)$ space, where $n$ is the length
of the `temperatures` array.
