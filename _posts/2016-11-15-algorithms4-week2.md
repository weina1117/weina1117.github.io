---
layout: post
title: Algorithm 4th chapter 2
---

Bags, Queues and Stacks
=======================

Fundamental data types
----------------------

* Value: collection of objects.
* Operations: insert, remove, iterate, test if empty.

Stack Examine the item most recently added. <---LIFO = "last in first out"

Queue Examine the item least recently added. <---FIFO = "first in first out"

### stacks

#### Arrary implementation of a stack.

* Use array s[] to store N items on stack.
* push(): add new item at s[N].
* pop(): remove item from s[N-1].

Defect. Stack overflows when N exceeds capacity.

#### Overflow and underflow.

* Underflow: throw exception if pop from an empty stack.

* Overflow: use resizing array for array implementation.

#### resizing arrays

 |           |    best    | worst | amortized |
 |-----------|------------|-------|-----------|
 |construct  |      1     |   1   |     1     |
 |push       |      1     |   N   |     1     | 
 |pop        |      1     |   N   |     1     |
 |size       |      1     |   1   |     1     |

##### resizing array vs. linked list

##### Linked-list implementation.
* Every operation takes constant time in the worst case.
* Uses Extra time and space to deal with the links.

##### Resizing array.
* Every operation take constant amortized time.
* Less waste space.

### queues

#### Array implementation of a queue.

* Use array q[] to store items in queue.
* enqueue(): add new item at q[tail].
* dequeue(): remove item from q[head].
* Update head and tail modulo the capacity.
* Add resizing array.

#### generics

#### iterators

#### applications

#### Dijkstra's two-stack algorithm.

* Value: push onto value stack.
* Operator: push onto operator stack.
* Left parenthesis: ignore.
* Right parenthesis: pop operator and two values; push the result of applying that operator to those values ontothe overand stack.
