---
title: "Representing Graphs in Python"
layout: post
excerpt: "A simple mental model for working with graphs in Python using pointer-like assignments and node references."
tags:
  - algorithms
  - data-structures
  - leetcode
---

A common source of confusion when first working with graphs in Python is how to
mentally model what the code is actually doing. Unlike low-level languages,
Python hides explicit pointers but the underlying idea is still there: variables
reference objects and objects can reference each other.

This post lays out a clean way to think about graphs using a pointer-style
interpretation and then shows it in action by reversing a linked list.

## Preliminaries

We will treat a graph as a collection of nodes, where each node can have multiple
outgoing directed edges to other nodes. A minimal Python representation might
look like this:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors
```

Each node stores a list of references to its neighboring nodes. The key is how
we interpret assignments involving these references.

## Creating pointers to nodes

Consider the following snippet:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors

X = Node(0, [])
```

The snippet above is represented visually in Fig. 1, where the node contains
the value $0$ and the pointer $X$ points to this node. Note that $X$ itself is
**not** a node and is just a pointer.

{% include figure.html
   filename="graphs-in-python/graphs-in-python-step1.svg"
   caption="Single node and a corresponding pointer."
   fignum=1
   scale=7
%}

Next, let's add the pointer `Y` to this snippet:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors

X = Node(0, [])
Y = X
```

This does not copy anything. It simply makes `Y` refer to the same `Node` as
`X`. Visually, this is shown in Fig. 2.

{% include figure.html
   filename="graphs-in-python/graphs-in-python-step2.svg"
   caption="Single node and two corresponding pointers."
   fignum=2
   scale=7
%}

## Adding neighbors to nodes

Now consider:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors

X = Node(0, [Node(1, []), Node(2, []), Node(3, [])])
Y = X
```

This new setup is shown in Fig. 3. We can see that node $0$ has 3 children: nodes
$1, 2,$ and $3$.

{% include figure.html
   filename="graphs-in-python/graphs-in-python-step3.svg"
   caption="Single node with two corresponding pointers and three child nodes."
   fignum=3
   scale=25
%}

## Moving pointers to nodes

We can then move pointers `X` and `Y` to nodes $1$ and $3$ respectively as follows:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors

X = Node(0, [Node(1, []), Node(2, []), Node(3, [])])
Y = X
X = X.neighbors[0]
Y = Y.neighbors[2]
```

This movement of pointers is shown in Fig. 4.

{% include figure.html
   filename="graphs-in-python/graphs-in-python-step4.svg"
   caption="Moving the X and Y pointers."
   fignum=4
   scale=25
%}

We can then change the values of the nodes that the $X$ and $Y$ pointers reference
as follows:

```python
class Node:
    def __init__(self, val: int, neighbors: list):
        self.val = val
        self.neighbors = neighbors

X = Node(0, [Node(1, []), Node(2, []), Node(3, [])])
Y = X
X = X.neighbors[0]
Y = Y.neighbors[2]
X.val = 7
Y.val = 23
```

and this new configuration is shown in Fig. 5.

{% include figure.html
   filename="graphs-in-python/graphs-in-python-step5.svg"
   caption="Changing the values of the nodes that the X and Y pointers reference."
   fignum=5
   scale=25
%}

Unfortunately, after moving pointers $X$ and $Y$, we can't make them reference
node $0$ again because this would require nodes $7$ and $23$ in Fig. 5 having
directed edges pointing at node $0$. So, we can only move pointers along the
direction indicated by the directed edges in a graph.

## A Concrete Example: Reversing a Linked List

A linked list is just a very restricted graph: each node has exactly one outgoing
edge (traditionally called `next` instead of `children[0]`):

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None
```

Suppose we start with:

```
A → B → C → None
```

and we want:

```
C → B → A → None
```

### The Code

```python
def reverse(head):
    prev = None
    curr = head

    while curr is not None:
        nxt = curr.next      # remember the next node
        curr.next = prev     # reverse the arrow
        prev = curr          # move prev forward
        curr = nxt           # move curr forward

    return prev
```

### Pointer-Level Interpretation

Let’s translate each line into the pointer model:

- `prev = None`
  → `prev` points to nothing

- `curr = head`
  → move `curr` to the first node (A)

Now the loop:

#### Step 1 (at node A)

- `nxt = curr.next`
  → store where A currently points (B)

- `curr.next = prev`
  → redirect A’s arrow to `None`
  (this _breaks_ A → B and creates A → None)

- `prev = curr`
  → move `prev` to A

- `curr = nxt`
  → move `curr` to B

State now:

```
A → None      B → C → None
^
prev          curr
```

#### Step 2 (at node B)

- save `C`
- reverse B → A
- move pointers

State:

```
B → A → None      C → None
^
prev              curr
```

#### Step 3 (at node C)

- save `None`
- reverse C → B
- move pointers

Final state:

```
C → B → A → None
^
prev
```

Return `prev`.

### What Actually Happened?

At no point did we “rebuild” the list.

We only performed two kinds of operations:

1. **Moved pointers** (`curr = ...`, `prev = ...`)
2. **Redirected arrows** (`curr.next = prev`)

This is exactly the same model described earlier:

- `head = X` → move a pointer
- `head.childN = Y` → redirect an edge

Reversing a linked list is just repeatedly **repointing edges while walking
through the graph**.

## Why This Mental Model Matters

This pointer-style interpretation clarifies several subtle points:

- Assignments (`=`) move references, not data
- Graph structure lives inside objects, not variables
- Mutations affect all references to the same node
- Traversal is just repeatedly “moving the pointer”
- Algorithms like reversal are just systematic edge rewiring

Without this model, it’s easy to misunderstand what Python is actually doing.

## Summary

You can think of graph manipulation in Python as a sequence of pointer operations:

- `head = X` → move pointer to node X
- `head.childN = Y` → create or update an edge from X to Y

Everything else—DFS, BFS, cycle detection, even linked list reversal—is built on top of these two ideas.

Once this clicks, graph code becomes much easier to reason about.
