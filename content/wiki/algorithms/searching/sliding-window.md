+++
title = 'Sliding Window'
date = 2024-10-01T16:17:08+02:00
draft = true
math = true
authors = ["Johnathan Jacobs"]
tags = ["search", "traverse", "dynamic-programming", "sliding-window"]
summary = "A technique to solve problems over a sub-range (window) of the input data."
+++

A technique to solve problems over a sub-range (window) of the input data.
Usually this window expands and contracts through the search.

- Time Complexity: $O(n)$
- Space Complexity: $O(1)$

## When to Use a Sliding Window

- Problem involves an ordered iterable
- Searching for a sub-range or sequence in the iterable
  (e.g. longest-, shortest-, max-, min-, target-sequence, etc.)
- There is a solution with a large time complexity,
  usually such as searching through all the sub-ranges
- Problem involves a stream of data, or real-time constraints
  (processing as new data comes in)

## Algorithm

- Time Complexity: $O(n)$
- Space Complexity: $O(1)$ (if you don't store the window) $O(k)$, otherwise

### Fixed-Size Sliding Window

Here, a window of a fixed size moves over the array by shifting
one position at a time.
The size of the window remains constant throughout the iteration.
Optimizations can be made wherein the window is shifted by more than one,
and the delta is used in aggregation instead of storing the elements in the window.

Use Cases:

- Finding the max/min aggregation of `n` consecutive elements in an array
  (e.g. max sum of `k` consecutive elements)
- Moving average or sum in a stream of data

### Dynamic-Sized Sliding Window

The window can grow or shrink based on the data.

Use Cases:

- Finding an optimal sub-sequence with some constraint (shortest with the sum>=target,
  Longest substring without repeating characters)

## Implementations

### Fixed-Sized Window Implementations

### Dynamic-Sized Window Implementations

## Optimizations

### Skipping Positions

### Alternative Window Representations

### Using the Window Delta in Aggregation
