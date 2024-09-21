+++
title = 'Stacks'
date = 2024-09-17T11:45:32+02:00
draft = false
math = true
tags = ["data-structure", "stack", "lifo"]
authors = ["Johnathan Jacobs"]
+++

Stacks are a Last In, First Out (LIFO) data structure.
The first element added to a stack, will be the last one removed.

## Queue Operations

- **Push**: Add an element to the top of the stack.
- **Pop**: Remove an element from the top of the stack.
- **Peek**: View the top element without removing it.
- **Empty**: Check if the stack is empty.

## Use Cases

Stacks are useful in situations where you need to reverse the order of elements
or where recursive processes are used.

- Function call stacks (tracking function calls in recursion): Each function call
  is pushed onto the stack, and when returning from the function, it is popped
  from the stack.
- Undo operations (e.g., in text editors): Every action is pushed onto the stack,
  when the user undoes an action, it is popped and reverted.
- [Depth-First Search (DFS)]({{< relref "wiki/algorithms/searching/depth-first-search" >}}) in graph traversal:
  Nodes are added to the stack and processed in a Last In, First Out manner, ensuring
  a deep traversal before exploring other branches.

## Complexity

- **Push**: $O(1)$ for time, as adding an element at the top is constant.
- **Pop**: $O(1)$ for time, as removing an element from the top is constant.
- **Peek**: $O(1)$ for time, since checking the top element doesn't involve iteration.
- **Space complexity**: $O(n)$, where $n$ is the number of elements in the stack.

## Implementation

The implementation here would optimally be a [Singly-Linked List]({{< relref "linked-lists#implementation" >}}),
where we push and pop from the top/front of the list.
