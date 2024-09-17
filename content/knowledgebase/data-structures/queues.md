+++
title = 'Queues'
date = 2024-09-17T11:44:31+02:00
draft = false
math = true
tags = ["data-structure", "queue", "linked-list", "fifo", "deque"]
+++

A queue is a linear **FIFO** (First In, First Out) data structure.
The first element added to a queue, will be the first one removed.

A **deque** (Double-Ended Queue), is a queue where elements could
be added and removed from either the front or the back.

## Queue Operations

- **Enqueue**: Add an element to the back of the queue.
- **Dequeue**: Remove an element from the front of the queue.
- **Peek**: Check the front element without removing it.
- **Empty**: Check if the queue is empty.

## Use Cases

Queues are often used in situations where elements need to be processed in the
order they arrive, such as:

- Task scheduling (e.g., CPU scheduling): Tasks can be added to the queue when
  they arrive, and processed in the order they were added.
- [Breadth-First Search (BFS)]({{< relref breadth-first-search.md >}}) traversal:
  Nodes in the graph can be added to a queue and processed level-by-level, ensuring
  nodes closer to the start are processed first.
- Message handling systems

## Complexity

- **Enqueue**: $O(1)$ for time, as adding an element at the back is constant.
- **Dequeue**: $O(1)$ for time, as removing an element from the front is constant.
- **Peek**: $O(1)$ for time, since checking the front element doesn't involve iteration.
- **Space complexity**: $O(n)$, where $n$ is the number of elements in the queue.

## Implementation

The implementation here would optimally be a [Singly-Linked List with a tail pointer]({{< relref "linked-lists#implementation" >}}),
where we push to the back of the list, and pop from the front.
