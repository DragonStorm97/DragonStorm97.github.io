+++
title = 'Dynamic Programming'
date = 2024-09-14T18:49:48+02:00
draft = false
math = true
+++

Dynamic Programming is an optimization strategy that usually results in recursive algorithms being turned into loops with data cached outside of it.

## Requirements

### Problem must have optimal substructure

The solution can be obtained by the combination of optimal solutions to its sub-problems.

### Problem must have overlapping sub-problems

Any solution solving the problem must not solve the same sub-problem multiple times
(i.e. do so without generating another sub-problem).
Fibonacci wouldn't work as it will repeatedly generate the same sub-problems
to have to be re-solved.

## Top-down approach

Memoize or store the solutions to sub-problems so
that we don't repeat work on already solved sub-problems.

## Bottom-up approach

Try solving the sub-problems first and use their solutions
to build-on and arrive to bigger sub-problems.

## Fibonacci Example

- Naive Fibonacci:

```go
func fib(n int) int {
  if n<= 1
    return n
  return fib(n-1) + fib(n-2)
}
```

- Top-down:
  We memoize the solutions to not repeat sub-problems.

```go
var m := map[int]int{0:0, 1:1}
func fib(n int) int {
  if _, exists := m[n]; !exists {
    m[n] = fib(n-1) + fib(n-2)
  }
  return m[n]
}
```

- Bottom-up:
  We calculate from the smallest value instead of the largest, to not repeat sub-problems.

```go
func fib(n int) int {
  if n == 0 {
    return 0
  }
  prev, cur := 0, 1
  for(i := 0; i < n-1; i++) {
    nw := prev + cur
    prev = cur
    cur = nw
  }
  return cur
}
```
