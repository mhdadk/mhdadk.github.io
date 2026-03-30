---

title: "Representing Graphs with Pointers in Python"
layout: post
excerpt: "A simple mental model for working with graphs in Python using pointer-like assignments and node references."
tags:

- algorithms
- data-structures
- leetcode
---

A surprisingly common source of confusion when first working with graphs in Python is how to mentally model what the code is actually *doing*. Unlike low-level languages, Python hides explicit pointers—but the underlying idea is still there: variables *reference* objects, and objects can reference each other.

This post lays out a clean way to think about graphs using a pointer-style interpretation, and then shows it in action by reversing a linked list.

## Preliminaries

We will treat a graph as a collection of **nodes**, where each node can have multiple outgoing edges (or “arrows”) to other nodes.

A minimal Python representation might look like this:

```python
class Node:
    def __init__(self):
        self.children = []
```

Each node stores a list of references to other nodes. The key is how we interpret assignments involving these references.

## The Pointer Interpretation

Consider the line:

```python
head = X
```

This does **not** copy anything. It simply makes `head` refer to the same node as `X`.

In pointer language:

> “Move the pointer `head` so that it points to node `X`.”

There is no duplication of structure—just a reassignment of where `head` points.

## Creating Edges

Now consider:

```python
head.childN = Y
```

(Or more concretely, something like `head.children[N] = Y`.)

This means:

> “Take the node currently pointed to by `head`, and make its Nth outgoing arrow point to node `Y`.”

This is the fundamental operation for building a graph.

### Example

```python
A = Node()
B = Node()
C = Node()

head = A
head.children.append(B)  # A → B
head.children.append(C)  # A → C
```

Interpretation:

* `head = A` → move pointer to node A
* `head.children.append(B)` → create an arrow from A to B
* `head.children.append(C)` → create another arrow from A to C

The structure now looks like:

```
A → B
 \
  → C
```

## Aliasing: Multiple Names, Same Node

A critical consequence of this model is **aliasing**.

```python
head = A
temp = head
```

Both `head` and `temp` point to the *same* node.

So:

```python
temp.children.append(B)
```

also modifies what you see through `head`.

Interpretation:

> Variables are just labels attached to nodes. Multiple labels can point to the same node.

## Mutability and Graph Structure

Because nodes are mutable, graph structure evolves through assignments:

```python
head = A
head.children.append(B)

head = B
head.children.append(C)
```

Interpretation:

1. Move `head` to A, add edge A → B
2. Move `head` to B, add edge B → C

Result:

```
A → B → C
```

The key idea is that **edges are created by mutating nodes, not by reassigning variables**.

## A Concrete Example: Reversing a Linked List

A linked list is just a very restricted graph: each node has exactly one outgoing edge (traditionally called `next` instead of `children[0]`).

```python
class Node:
    def __init__(self, value):
        self.value = value
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

* `prev = None`
  → `prev` points to nothing

* `curr = head`
  → move `curr` to the first node (A)

Now the loop:

#### Step 1 (at node A)

* `nxt = curr.next`
  → store where A currently points (B)

* `curr.next = prev`
  → redirect A’s arrow to `None`
  (this *breaks* A → B and creates A → None)

* `prev = curr`
  → move `prev` to A

* `curr = nxt`
  → move `curr` to B

State now:

```
A → None      B → C → None
^
prev          curr
```

#### Step 2 (at node B)

* save `C`
* reverse B → A
* move pointers

State:

```
B → A → None      C → None
^
prev              curr
```

#### Step 3 (at node C)

* save `None`
* reverse C → B
* move pointers

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

* `head = X` → move a pointer
* `head.childN = Y` → redirect an edge

Reversing a linked list is just repeatedly **repointing edges while walking through the graph**.

## Why This Mental Model Matters

This pointer-style interpretation clarifies several subtle points:

* Assignments (`=`) move references, not data
* Graph structure lives inside objects, not variables
* Mutations affect all references to the same node
* Traversal is just repeatedly “moving the pointer”
* Algorithms like reversal are just systematic edge rewiring

Without this model, it’s easy to misunderstand what Python is actually doing.

## Summary

You can think of graph manipulation in Python as a sequence of pointer operations:

* `head = X` → move pointer to node X
* `head.childN = Y` → create or update an edge from X to Y

Everything else—DFS, BFS, cycle detection, even linked list reversal—is built on top of these two ideas.

Once this clicks, graph code becomes much easier to reason about.
