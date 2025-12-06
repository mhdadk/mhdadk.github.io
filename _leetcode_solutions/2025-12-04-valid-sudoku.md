---
title: "36. Valid Sudoku"
layout: post
tags:
  - arrays-and-hashing
  - leetcode-medium
---
## [Problem statement](https://leetcode.com/problems/valid-sudoku/description/)

## Naive solution

The simplest way to solve this problem is as follows:

1. For each row in the Sudoku board, iterate over the cells in the row. Then, for each
cell in the row, iterate over all other 8 cells in the row to ensure that there are no
duplicates.
2. Repeat step 1 for all 9 columns.
3. Repeat step 1 for all 9 $3 \times 3$ sub-boxes.

Although this approach is simple, it is inefficient. Specifically, if there are $n$ rows and columns on the Sudoku board, then step 1 will require $n(n-1) = n^2 - n$ iterations for
each row, and since there are $n$ rows, the total number of iterations will be
$n^2(n-1) = n^3 - n^2$, which translates to $O(n^3)$ time. A similar analysis can be
done for the columns and sub-boxes.

## Better solution

In the aforementioned solution, we iterate over the rows, columns, and sub-boxes without
attempting to "remember" which numbers we already iterated over. This seems wasteful.

Hence, a more efficient approach to solving this problem would be as follows:

1. Initialize three dictonaries: `rows`, `cols`, and `boxes`.\
    a. These dictionaries will be used to recall which numbers we already iterated over in the rows, columns, and sub-boxes, respectively.\
    b. Specifically, `rows[i]` and `cols[i]` will each contain a hash set that contains
    all the unique elements of the `(i+1)`th row and column, respectively, in the Sudoku
    board.\
    c. Similarly, `boxes[(i, j)]` will contain a hash set that contains the unique
    elements in the sub-box located in the `(i+1)`th row and `(j+1)`th column in the
    `3 x 3` grid of sub-boxes.\
    d. These hash sets will be used to check for duplicates.
2. For each cell in the Sudoku board:\
    a. Check if the cell contains ".". If it doees, skip to the next cell.\
    b. If the cell is in the first row, check if the value in the cell is in `rows[0]`.
    If it is, return `False`. Otherwise, add the value in the cell to `rows[0]` to
    remember it for later.\
    c. Repeat step 2(b) for rows 2-9.\
    d. If the cell is in the first column, check if the value in the cell is in `cols[0]`.
    If it is, return `False`. Otherwise, add the value in the cell to `cols[0]` to
    remember it for later.\
    e. Repeat step 2(d) for columns 2-9.\
    f. If the cell is in the `(0, 0)` sub-box, check if the value in the cell is in
    `boxes[(0, 0)]`. If it is, return `False`. Otherwise, add the value in the cell to
    `boxes[(0, 0)]` to remember it for later.\
    g. Repeat step 2(f) for sub-boxes `(0, 1), (0, 2), (1, 0), ..., (2, 2)`.\
3. Return `True`, which means that no duplicates were found.

This can be naively implemented in Python as follows:

```python
from collections import defaultdict
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rows = defaultdict(set)
        cols = defaultdict(set)
        boxes = defaultdict(set)
        for r in range(len(board)):
            for c in range(len(board[0])):
                if board[r][c] == ".":
                    continue
                if r == 0:
                    if board[0][c] in rows[0]:
                        return False
                    else:
                        rows[0].add(board[r][c])
                if r == 1:
                    if board[1][c] in rows[1]:
                        return False
                    else:
                        rows[1].add(board[r][c])
                ...
                if c == 0:
                    if board[r][0] in cols[0]:
                        return False
                    else:
                        cols[0].add(board[r][c])
                if c == 1:
                    if board[r][1] in cols[1]:
                        return False
                    else:
                        cols[1].add(board[r][c])
                ...
                if r // 3 == 0 and c // 3 == 0:
                    if board[r][c] in boxes[(0, 0)]:
                        return False
                    else:
                        boxes[(0, 0)].add(board[r][c])
                if r // 3 == 0 and c // 3 == 1:
                    if board[r][c] in boxes[(0, 1)]:
                        return False
                    else:
                        boxes[(0, 1)].add(board[r][c])
                if r // 3 == 0 and c // 3 == 2:
                    if board[r][c] in boxes[(0, 2)]:
                        return False
                    else:
                        boxes[(0, 2)].add(board[r][c])
                if r // 3 == 1 and c // 3 == 0:
                    if board[r][c] in boxes[(1, 0)]:
                        return False
                    else:
                        boxes[(1, 0)].add(board[r][c])
                ...
        return True
```

However, this code contains redundancies and can be optimized further to be:

```python
from collections import defaultdict
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        rows = defaultdict(set)
        cols = defaultdict(set)
        boxes = defaultdict(set)
        for r in range(len(board)):
            for c in range(len(board[0])):
                if board[r][c] == ".":
                    continue
                if board[r][c] in rows[r]:
                    return False
                else:
                    rows[r].add(board[r][c])
                if board[r][c] in cols[c]:
                    return False
                else:
                    cols[c].add(board[r][c])
                if board[r][c] in boxes[(r // 3, c // 3)]:
                    return False
                else:
                    boxes[(r // 3, c // 3)].add(board[r][c])
        return True
```

If the Sudoku board contains `n` rows and columns, then this solution runs in $O(n^2)$
time and requires $O(n^2)$ space.
