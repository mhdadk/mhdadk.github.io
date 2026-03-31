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

We can then change the values of the nodes that the $X$ and $Y$ pointers reference:

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

This new configuration is shown in Fig. 5.

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

In fact, from the perspective of pointer $X$, only node $7$, the node that pointer
$X$ is referencing, exists. All other nodes do not exist. This is because there
are no outgoing directed edges from node $7$.

Similarly, from the perspective of pointer $Y$, only node $23$ exists because
there are no outgoing directed edges from node $23$.

## A concrete example: reversing a linked list

We can see how this mental model is useful via an example. Consider the linked
list shown in Fig. 6.

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step1.svg"
   caption="Linked list example."
   fignum=6
   scale=60
%}

A linked list is just a very restricted graph: each node has exactly one outgoing
edge (traditionally called `next` instead of `children[0]`):

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None
```

We want to reverse this linked list so that it looks like Fig. 7.

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step2.svg"
   caption="Reverse of linked list shown in Fig. 6."
   fignum=7
   scale=60
%}

We can perform this reversal sequentially as follows. First, we introduce three
pointers: `prev, curr,` and `next` by writing `prev = None`, `curr = head`, and
`next = head.next`:

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step3.svg"
   fignum=8
   scale=60
%}

Note that we replaced `A` with `head` to keep this implementation
general: Next, move `curr`'s arrow to point to `prev` instead of `curr.next` by
writing `curr.next = prev`:

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step4.svg"
   fignum=9
   scale=60
%}

Then, move the `prev, curr`, and `next` pointers 1 step forward by writing
`prev = curr`, `curr = next`, and `next = next.next`:

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step5.svg"
   fignum=10
   scale=60
%}

Repeat this process until the `next` pointer references `None`:

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step6.svg"
   fignum=11
   scale=60
%}

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step7.svg"
   fignum=12
   scale=60
%}

{% include figure.html
   filename="graphs-in-python/reversing-linked-list-step8.svg"
   fignum=13
   scale=60
%}

Once the `next` pointer references `None`, we are done. In Python, we would
implement this logic as follows:

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    # edge case
    if head is None:
        return head

    # first iteration
    prev = None
    curr = head
    nxt = head.next
    curr.next = prev

    # second iteration onwards
    while nxt is not None:
        prev = curr
        curr = nxt
        nxt = nxt.next
        curr.next = prev
    return curr

# Build A -> B -> C -> None
head = ListNode("A", ListNode("B", ListNode("C")))
head_reversed = reverseList(head)

# Print result
curr = head_reversed
while curr:
    print(curr.val, end=" -> ")
    curr = curr.next
print("None")
```

## Why this mental model matters

This pointer-style interpretation clarifies several subtle points:

- Assignments (`=`) move references, not data.
- Graph structure lives inside objects, not variables.
- Mutations affect all references to the same node.
- Traversal through a graph is just repeatedly "moving the pointer".
- Algorithms like reversal are just systematic edge rewiring.

Without this model, it’s easy to get lost in the details of what Python is doing.

## Summary

You can think of graph manipulation in Python as a sequence of pointer operations:

- `head = X`: move pointer to node `X`
- `head.childN = Y`: create or update an edge from `X` to `Y`

Everything else including DFS, BFS, cycle detection, even linked list reversal,
is built on top of these two ideas.
