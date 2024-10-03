+++
title = 'Sliding Window'
date = 2024-10-01T16:17:08+02:00
draft = false
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

C++23 ranges implementation of a max-sum-over-fixed-window.

```cpp
#include <vector>
#include <ranges>
#include <algorithm>

[[nodiscard]] int maxSumOverWindow(const std::vector<int>& data, std::size_t window_size) {
  return std::ranges::max(
    data
    | std::ranges::views::slide(window_size)
    | std::views::transform([](auto&& window) {
        return *std::ranges::fold_left_first(window, std::plus<int>());
    })
  );
}

int main() {
  volatile int res = maxSumOverWindow({1,2,3, 4, 8, 9, 51, 0}, 3);
  return res;
}
```

### Dynamic-Sized Window Implementations

C++ implementation of a dynamic-sized sliding window algorithm,
finding the smallest window with a sum greater than the target.

```cpp
#include <vector>
#include <ranges>
#include <algorithm>

[[nodiscard]] int minWindowWithMinSum(const std::vector<int>& data, int target) {
  auto left = data.begin();
  auto right = data.begin();
  int current_sum = 0;
  std::size_t min_window_size = data.size() + 1; // Start with invalid large size

  while (right != data.end()) {
    // Expand window by adding the element at `right`
    // We do this instead of storing the elements of
    // the window and re-aggregating them
    current_sum += *right;
    ++right;

    // Shrink window from the left if sum exceeds target
    while (current_sum > target) {
      auto window_size = static_cast<std::size_t>(std::ranges::distance(left, right));
      min_window_size = std::min(min_window_size, window_size);

      // Shrink window by removing element at `left`
      current_sum -= *left;
      ++left;
    }
  }

  return (min_window_size <= data.size())
  ? static_cast<int>(min_window_size)
  : 0;
}

int main() {
  return minWindowWithMinSum({1, 2, 3, 4, 5, 6, 7, 8, 9}, 15);
}
```

## Optimizations

- **Skipping Positions**:

  Depending on the problem, it might be possible to slide the window by more than
  one element.

- **Alternative Window Representations**
  - Hashset
  - Bitset/map
- **Using the Window Delta in Aggregation**:

  Instead of storing the window itself, it may be possible to only store the
  aggregation and then remove the effects of the removed element(s) of the window,
  and add the effects of the new element(s) in the window.
